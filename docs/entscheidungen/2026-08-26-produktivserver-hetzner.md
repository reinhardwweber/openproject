# Produktivserver auf Hetzner CX33 (26.08.2026)

Der bis dahin rein lokale Docker-Compose-Prototyp läuft jetzt zusätzlich auf einem
gemeinsam nutzbaren Server – perspektivisch als Ticketsystem für das AgentPilot-/
Drupal-11-Relaunch-Vorhaben (siehe stattreisen-Projektgedächtnis
`project_agentpilot-backlog-entwurf-drupal11-relaunch`).

## Server

- Hetzner Cloud CX33, Nürnberg, IPv4 `195.201.5.215`, IPv6 `2a01:4f8:c2c:62e2::1`
- Ursprünglich im Hetzner-Projekt `stattreisen` angelegt, noch am 26.08.2026 ins eigene
  Hetzner-Projekt `webteam` transferiert (Server-ID/IP unverändert). Cloud-Firewalls sind
  projektgebunden und wandern beim Transfer nicht mit – Firewall im Zielprojekt neu
  angelegt (gleiche 4 Regeln: TCP 22/80/443 + ICMP, je Any IPv4/IPv6), alte im
  `stattreisen`-Projekt verwaist zurückgelassen. Nach dem Transfer SSH-Zugriff und
  HTTPS-Erreichbarkeit erneut extern verifiziert.
- Erreichbar als `ssh openproject` (Key `~/.ssh/id_openproject_hetzner`, ohne Passphrase –
  bewusst anders als der Matomo-Key, der Automatisierung wegen)
- Ubuntu 26.04 LTS, Hostname `openproject`, Zeitzone Europe/Berlin
- SSH nur per Key (`PasswordAuthentication no`, `PermitRootLogin prohibit-password`,
  eigene Config als `/etc/ssh/sshd_config.d/00-hardening.conf` – Ubuntus
  `50-cloud-init.conf` setzt sonst `PasswordAuthentication yes` und gewinnt, da OpenSSH
  die erste Fundstelle nimmt)
- `unattended-upgrades` mit automatischem Reboot 04:30
- Hetzner-Cloud-Firewall `firewall open project`: TCP 22/80/443 + ICMP, je IPv4 und IPv6

## Stack

- Offizielles `opf/openproject-docker-compose` (Branch `stable/17`), direkt geklont nach
  `/opt/openproject` auf dem Server (nicht über dieses Repo/Submodule ausgerollt, um das
  Anlegen eines GitHub-Remotes für dieses Repo nicht zur Voraussetzung zu machen)
- `.env` mit frischen, serverseitig generierten Secrets (nicht die lokalen Test-Werte
  wie `p4ssw0rd` übernommen)
- **TLS-Architektur:** Der mitgelieferte `proxy`-Container terminiert selbst kein TLS
  (reines HTTP-Forwarding, siehe README des Compose-Repos). Deshalb zusätzlicher
  `edge`-Service (`caddy:2`, eigene Definition in `docker-compose.override.yml`) davor,
  der Port 80/443 hält, automatisch Let's-Encrypt-Zertifikate holt und an `proxy:80`
  weiterreicht. Der interne `proxy` ist dafür nur noch auf `127.0.0.1:18080` gebunden
  (`PORT=127.0.0.1:18080` in `.env`), nicht mehr auf `0.0.0.0:80`.
- Domain: `openproject.webteam.de` (A/AAAA bei Schlund/IONOS, `OPENPROJECT_HOST__NAME`
  entsprechend gesetzt, `OPENPROJECT_HTTPS=true`)
- `docker-compose.override.yml` außerdem: `web`-Healthcheck-`start_period` auf 150s
  hochgesetzt (siehe Fallstrick unten)

## Datenübernahme

Alle 5 lokalen Projekte (243 Work Packages) per `pg_dump -Fc` / `pg_restore` von der
lokalen Docker-Desktop-Instanz auf den Server übertragen, lokale Instanz blieb dabei
unverändert.

## Fallstricke (falls nochmal aufgesetzt wird)

1. **`autoheal` killt `web` im Bootschleifen-Loop nach Neustarts.** `AUTOHEAL_START_PERIOD`
   bezieht sich auf die Laufzeit des `autoheal`-Containers selbst, nicht auf die des
   überwachten Containers. War die Grace-Period von `autoheal` schon abgelaufen (z. B.
   weil der Stack schon länger läuft), killt es einen frisch (neu-)startenden `web` mitten
   im ca. 60–90s dauernden Boot, noch bevor der Healthcheck grün wird – Endlosschleife.
   Betrifft jeden künftigen Neustart, auch den automatischen 04:30-Reboot. Fix: Healthcheck-
   `start_period` für `web` per Override deutlich über die tatsächliche Bootzeit anheben
   (hier: 150s), damit Docker `web` gar nicht erst als unhealthy markiert.
2. **Leere `ports: []` in `docker-compose.override.yml` wird von Compose 5.5.0 nicht als
   "Ports entfernen" interpretiert** – der Wert aus der Basis-Datei (`${PORT:-8080}:80`)
   blieb wirksam und kollidierte mit `edge` auf Port 80. Zuverlässiger Weg: den zugrunde
   liegenden `PORT`-Wert in `.env` selbst ändern (hier auf `127.0.0.1:18080`), statt die
   Ports-Liste per Override zu leeren.
3. Ein am Portkonflikt gescheiterter Container (`failed to set up container networking`)
   blieb danach ganz ohne Netzwerkzuordnung hängen, auch nach dem Konfliktfix – bloßes
   `docker compose up -d <service>` reichte nicht, erst `docker compose rm -f -s <service>`
   gefolgt von `up -d` hat das Netzwerk sauber neu zugeordnet.
4. **Attachments ließen sich nicht hochladen** (Fehler still, kein OpenProject-Setting
   war fehlerhaft): `OPDATA=/var/openproject/assets` in der `.env` ist laut
   `.env.example` ein **Host-Pfad, kein benannter Docker-Volume-Name** – Compose bindet
   ihn deshalb 1:1 vom Host in den Container. Der Ordner wurde beim ersten Start
   `root:root`-eigen mit `755` angelegt, der App-Prozess läuft aber als `app` (UID 1000)
   und hatte damit kein Schreibrecht. Fix: `chown -R 1000:1000 /var/openproject/assets`
   direkt auf dem Host (nicht im Container, dort fehlt dafür die Berechtigung). Beim
   nächsten Neuaufsetzen vorsorglich direkt nach dem ersten Start prüfen, ebenso
   `/var/lib/postgresql/data` (dort korrekt `999:root`, vom Postgres-Image selbst verwaltet).
5. Zweimal ist beim Anzeigen der `.env` (zur Kontrolle) versehentlich ein Secret im
   Terminal-Output gelandet (`DATABASE_URL` enthält das Postgres-Passwort, ohne dass die
   Zeile das Wort "PASSWORD" enthält; `COLLABORATIVE_SERVER_SECRET` beim ersten Mal
   übersehen) – beide Male sofort rotiert, bevor sie je von einem laufenden Container
   genutzt wurden. Für künftige Kontrollen: `.env` nicht mehr ausgeben, sondern gezielt
   nur unkritische Keys einzeln abfragen.

## Noch offen

- Kein automatisiertes Backup auf dem neuen Server (der `control/backup`-Container aus dem
  Compose-Setup ist noch nicht eingerichtet)
- Login noch mit den aus der lokalen Instanz übernommenen Zugangsdaten
- Repo `openproject` selbst hat weiterhin kein GitHub-Remote (siehe `backlog/backlog.md`) –
  für dieses Server-Setup nicht nötig gewesen, da direkt aus dem offiziellen
  `opf/openproject-docker-compose`-Repo auf den Server geklont wurde

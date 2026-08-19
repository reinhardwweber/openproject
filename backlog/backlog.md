# Backlog – Projekt "openproject" (Setup, nicht Inhalt)

Aufgaben für den Aufbau dieses Ticketsystem-Prototyps selbst. Das inhaltliche stattreisen-Backlog bleibt unangetastet in `stattreisen/Projekt/backlog.md` bzw. Trello.

## Offen

- [ ] Docker Desktop lokal installieren (Stand 19.08.2026: nicht vorhanden)
- [ ] Fork von `jtauschl/openproject-ce-mcp` nach `reinhardwweber` anlegen
- [ ] Repo `openproject` unter `reinhardwweber` auf GitHub anlegen (privat), lokales Repo als `origin` verbinden
- [ ] Submodule einbinden: `infra/openproject` (opf/openproject-docker-compose, stable/17) und `tools/openproject-ce-mcp` (eigener Fork)
- [ ] OpenProject lokal per Docker Compose starten, Admin-Login sofort ändern
- [ ] MCP-Server konfigurieren (API-Token, `.env`), gegen Claude Code testen
- [ ] Erste Testdaten/Testprojekt in OpenProject anlegen, MCP-Tools end-to-end durchspielen (lesen + schreiben mit Preview-Confirm)
- [ ] Freigaben/Arbeitsweise-Abschnitt in `CLAUDE.md` konkretisieren, sobald erste Routine erkennbar ist

## Später (nicht jetzt)

- [ ] Migration/Ablösung von `stattreisen/Projekt/backlog.md` + Trello, falls sich der Prototyp bewährt
- [ ] Entscheidung über gemeinsam genutzten Server statt lokaler Einzelinstanzen

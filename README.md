# openproject

Eigenständiges Projekt (unabhängig von `stattreisen`) zum Aufbau einer lokalen OpenProject-Community-Edition-Instanz mit MCP-Anbindung für Claude Code. Ziel: ein Ticketsystem, gegen das mehrere Personen mit eigenen Claude-Code-Sessions auf demselben Doku-/Code-Stand arbeiten können.

**Phase (Stand 19.08.2026): lokaler Prototyp.** Die OpenProject-Instanz läuft vorerst nur lokal per Docker Compose auf einzelnen Rechnern, nicht als gemeinsam genutzter Server. Dieses Repo (Doku, Backlog, Tests, Infra-Konfiguration) ist aber von Anfang an gemeinsam nutzbar — jede:r klont dasselbe Repo und sieht denselben Stand.

## Ordnerstruktur

- `docs/` – Entscheidungsdokumentation und Architekturüberblick
- `backlog/` – Aufgaben für dieses Setup-Projekt selbst (nicht das inhaltliche stattreisen-Backlog)
- `infra/openproject/` – Git-Submodule des offiziellen `opf/openproject-docker-compose`-Repos (Branch `stable/17`); lokale `.env` darin, nicht versioniert
- `tools/openproject-ce-mcp/` – Git-Submodule des eigenen Forks von `jtauschl/openproject-ce-mcp`; `origin` = Fork, `upstream` = Original, damit Beiträge zurück möglich bleiben
- `tests/` – Integrationstests gegen die lokale Instanz
- `.claude/agents/` – ggf. Subagent-Rollen für einen späteren Multi-Agent-Workflow

## Setup (Kurzfassung)

1. Docker Desktop installieren (lokal noch nicht vorhanden, Stand 19.08.2026 geprüft).
2. Dieses Repo klonen, danach Submodule holen:
   ```
   git submodule update --init --recursive
   ```
3. In `infra/openproject/`: `.env.example` nach `.env` kopieren, `SECRET_KEY_BASE` setzen, dann `OPENPROJECT_HTTPS=false docker compose up -d --build --pull always`. Erreichbar unter `http://localhost:8080`, Standard-Login `admin`/`admin` (sofort ändern).
4. In `tools/openproject-ce-mcp/`: gemäß dessen README installieren, `.env` mit `OPENPROJECT_BASE_URL` und eigenem API-Token (aus dem OpenProject-Profil) anlegen, MCP-Client (Claude Code) darauf verweisen.
5. Details und Hintergrund siehe `docs/`.

## Secrets

`.env`-Dateien (Docker-Secrets, API-Token) werden grundsätzlich nicht committet — pro Person eine eigene lokale Kopie. Siehe `docs/entscheidungen/` zur Begründung und ggf. künftig 1Password-Credential-Brokering.

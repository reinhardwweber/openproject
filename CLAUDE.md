# openproject – Projektregeln für Claude Code

## Kontext

Dieses Repo baut eine lokale OpenProject-Community-Edition-Instanz samt MCP-Server-Anbindung auf, als künftiges Ticketsystem und als Testumgebung für Agent-gestützte Backlog-Arbeit (Bezug: AgentPilot-Überlegungen unter Claude Cowork / mait-business-plan-Projektgedächtnis).

Eigenständiges technisches Projekt, unabhängig vom `stattreisen`-Projektordner (eigenes Repo, eigenes CLAUDE.md, keine Vermischung).

## Stack (Stand 19.08.2026)

- OpenProject Community Edition, Docker Compose (offizielles Repo `opf/openproject-docker-compose`, Branch `stable/17`), als Git-Submodule unter `infra/openproject/`
- MCP-Server: `jtauschl/openproject-ce-mcp` (Python, `uv`, MIT-Lizenz), eigener Fork unter GitHub-Account `reinhardwweber`, als Git-Submodule unter `tools/openproject-ce-mcp/` (Remotes: `origin` = eigener Fork, `upstream` = Original — für PRs gegen die korrekten Release-Branches, z. B. `release/0.3.7` / `release/0.4.0`, nicht gegen `main`)
- Phase: lokaler Prototyp (kein gemeinsam genutzter Server); Repo selbst ist von Anfang an gemeinsam nutzbar

## Ordnerstruktur

Siehe `README.md`. Kurzfassung: `docs/` (Entscheidungen, Architektur), `backlog/` (Aufgaben für dieses Setup-Projekt), `infra/openproject/` (Server-Submodule), `tools/openproject-ce-mcp/` (MCP-Server-Submodule), `tests/`, `.claude/agents/`.

## Secrets

Niemals `.env`-Dateien, API-Token, DB-Passwörter oder `SECRET_KEY_BASE` committen. Jede Person legt sich lokal eine eigene `.env` aus der jeweiligen `.env.example` an. Bei Bedarf künftig 1Password-Credential-Brokering statt Klartext (siehe Claude-Cowork-Projektgedächtnis `secret-handling-agenten-1password.md`).

## Arbeitsweise / Freigaben

Noch keine Vorab-Freigaben definiert (Stand 19.08.2026, Projekt gerade erst angelegt) — bis auf Weiteres vor Commit/Push, Submodule-Änderungen und allem, was über lokale Docker-Instanzen hinausgeht, kurz Rückmeldung einholen. Diese Sektion nach den ersten Arbeitszyklen konkretisieren (analog zum Vorgehen im `stattreisen`-Projekt).

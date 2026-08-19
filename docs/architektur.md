# Architekturüberblick

```
openproject/                          (dieses Repo)
├── infra/openproject/                 Submodule: opf/openproject-docker-compose (stable/17)
│                                       → docker compose up → OpenProject CE unter localhost:8080
├── tools/openproject-ce-mcp/           Submodule: eigener Fork von jtauschl/openproject-ce-mcp
│                                       → spricht APIv3 der lokalen Instanz, stellt MCP-Tools bereit
│                                       → origin = Fork (reinhardwweber), upstream = jtauschl/openproject-ce-mcp
└── Claude Code (lokal, pro Person)     verbindet sich als MCP-Client mit tools/openproject-ce-mcp
```

Datenfluss: Claude Code → MCP-Client-Konfiguration → `openproject-ce-mcp`-Server (lokaler Prozess) → OpenProject-APIv3 (`localhost:8080`) → Postgres (Docker-Volume `compose_pgdata`).

Alle drei Bausteine sind unabhängig versioniert (eigene Historie je Submodule), aber über dieses Repo als ein gemeinsamer Stand auschecken- und teilbar. Secrets (`.env` in `infra/openproject/` und `tools/openproject-ce-mcp/`) liegen bewusst außerhalb der Git-Historie beider Submodule und dieses Repos.

Hintergrund/Begründung der Werkzeugwahl: siehe `entscheidungen/`.

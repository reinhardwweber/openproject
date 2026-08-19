# Entscheidung: OpenProject + Community-MCP-Server als Ticketsystem

Stand: 19.08.2026 (siehe auch Claude-Cowork-Projektgedächtnis `ticketsystem-openproject-mcp.md` im mait-business-plan-Projekt für die ausführliche Recherche)

## Kurzfassung

OpenProject wurde als Ticketsystem gewählt: Open Source, self-hostbar (passt zur DSGVO-Haltung), aktives Ökosystem rund um KI-Integration.

Zwei MCP-Wege existieren:

- **Nativer Hersteller-MCP-Server** (seit OpenProject 17.2, März 2026): technisch sauber, aber nur lesend und an kostenpflichtige Enterprise-Stufen gebunden (ab "Professional", Cloud ab 10,95 €/Nutzer/Monat, Mindestabnahme 25 Nutzer). Für die Community Edition nicht verfügbar.
- **Community-Open-Source-MCP-Server** gegen die freie APIv3: gewählt wurde `jtauschl/openproject-ce-mcp` (132 Tools, Schreiben inkl. nicht umgehbarem Preview-then-Confirm-Muster, MIT-Lizenz, aktiv gepflegt). Alternative `AndyEverything/openproject-mcp-server` verworfen (explizit als "Do not use it productively — WIP" markiert).

## Warum Community-Edition statt Enterprise

Kein Schreibzugriff im nativen MCP-Server, Mindestabnahme von 25 Nutzern (Cloud) unpassend für die aktuelle Projektgröße, und die freie APIv3 der Community Edition reicht für den `jtauschl`-Server vollständig aus.

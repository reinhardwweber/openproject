# Contributor-Fähigkeit von jtauschl/openproject-ce-mcp

Stand: 19.08.2026 (Details siehe Claude-Cowork-Projektgedächtnis `ticketsystem-openproject-mcp.md`)

- Solo-Maintainer (@jtauschl), aber reaktionsschnell und kooperativ.
- Parallele Release-Branches: `release/0.3.7` (Wartung, flache Architektur) und `release/0.4.0` (laufender Rewrite, Port/Adapter/Service). PRs müssen gegen den passenden Branch gehen, nicht gegen `main`.
- **Beleg für Agent-unterstützte Beiträge:** PR #24 ("🤖 Generated with Claude Code") — ein externer Contributor fand einen echten Bug (Auflösung von User-Referenzfeldern über verlinkte statt eingebettete `allowedValues`), belegte ihn mit eigenen Tests und Verifikation gegen eine echte Instanz. Maintainer hat den Fix per Cherry-Pick in beide Release-Branches übernommen, Autorenschaft erhalten, sich bedankt und den Fix sogar in die neue Architektur portiert.
- Zweiter externer PR (#1) wurde vom Autor selbst zurückgezogen (eigener Fork), keine Ablehnung durch den Maintainer.
- Einschränkung: Solo-Maintainer bleibt Bus-Factor-Risiko; keine öffentlich sichtbare Policy zu KI-generierten Beiträgen (aber faktisch akzeptiert).

## Konsequenz fürs eigene Vorgehen

Bei eigenen Fixes/Erweiterungen: Fix + eigene Tests + Verifikation gegen die lokale Instanz, PR aus dem eigenen Fork (`reinhardwweber/openproject-ce-mcp`, Submodule unter `tools/openproject-ce-mcp/`) gegen den korrekten Upstream-Release-Branch, nicht gegen `main`.

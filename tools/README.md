# tools/openproject-ce-mcp – Platzhalter

Wird als Git-Submodule des eigenen Forks eingebunden, sobald der Fork existiert:

```
git submodule add https://github.com/reinhardwweber/openproject-ce-mcp.git tools/openproject-ce-mcp
cd tools/openproject-ce-mcp
git remote add upstream https://github.com/jtauschl/openproject-ce-mcp.git
```

Danach lokale `.env` mit `OPENPROJECT_BASE_URL` und eigenem API-Token (aus dem OpenProject-Profil, sobald die Instanz läuft) anlegen — nicht committen.

Noch nicht ausgeführt (Stand 19.08.2026) — Fork wird gerade angelegt.

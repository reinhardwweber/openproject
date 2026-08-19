# infra/openproject – Platzhalter

Wird als Git-Submodule des offiziellen Repos eingebunden:

```
git submodule add -b stable/17 https://github.com/opf/openproject-docker-compose.git infra/openproject
```

Danach gemäß dessen eigener README: `.env.example` → `.env` kopieren, `SECRET_KEY_BASE` setzen, `OPENPROJECT_HTTPS=false docker compose up -d --build --pull always`. Erreichbar unter `http://localhost:8080` (Standard-Login `admin`/`admin`, sofort ändern). Daten liegen in benannten Docker-Volumes (`compose_opdata`, `compose_pgdata`), nicht in diesem Ordner.

Noch nicht ausgeführt (Stand 19.08.2026) — Docker Desktop muss vorher lokal installiert werden.

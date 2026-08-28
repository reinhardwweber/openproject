# Review-Anweisungen

> Beispiel/Vorlage. Diese Datei steuert ausschließlich das automatisierte Pull-Request-Review (z. B. das native Code-Review-Feature oder eine eigene Review-Subagent-Rolle), nicht die allgemeine Projektarbeit – dafür ist `CLAUDE.md` zuständig. Wird als höchste Priorität in den Systemprompt jedes Review-Agenten eingefügt.

## Was hier "Important" (kritisch) bedeutet

Reserviere die höchste Stufe für Befunde, die Verhalten sichtbar brechen, Daten preisgeben oder ein Rollback erschweren würden:

- fehlerhafte Geschäftslogik, die zu falschen Ergebnissen führt
- ungeschützte oder nicht mandantengetrennte Datenbankabfragen
- personenbezogene Daten in Logs, Fehlermeldungen oder Analytics
- fehlende Auth-/Berechtigungsprüfung an neuen Endpunkten
- Migrationen, die nicht rückwärtskompatibel sind

Stil, Benennung und reine Refactoring-Vorschläge sind höchstens "Nit" (geringfügig).

## Nit-Obergrenze

Maximal fünf Nits pro Review ausgeben. Bei mehr Funden: "plus N weitere ähnliche Punkte" in der Zusammenfassung nennen statt jeden einzeln aufzulisten. Wenn alle Funde nur Nits sind, die Zusammenfassung mit "Keine blockierenden Probleme" beginnen.

## Nicht melden

- Alles, was bereits über Linter/Formatter/CI abgedeckt ist
- generierte Dateien und Lockfiles
- test-interner Code, der Produktionsregeln absichtlich verletzt

## Immer prüfen

- Neue Endpunkte/Routen haben einen Integrationstest
- Log-Zeilen enthalten keine E-Mail-Adressen, Namen oder Nutzer-IDs
- Datenbankabfragen sind auf den jeweiligen Mandanten/Nutzer beschränkt
- Neue Abhängigkeiten sind in der Pull-Request-Beschreibung begründet

## Verifikationsanspruch

Ein Befund braucht einen Beleg – einen konkreten `Datei:Zeile`-Verweis im Code, ein Testergebnis oder einen tatsächlichen Reproduktionsschritt, keine reine Vermutung aus Funktionsnamen. Wo möglich: den betreffenden Testfall wirklich ausführen bzw. den Endpunkt wirklich ansprechen, statt den Code nur zu lesen und zu beurteilen.

## Eskalation an einen Menschen

Folgende Fälle werden nicht vom Review-Agenten selbst entschieden, sondern explizit als "menschliche Prüfung erforderlich" markiert und dürfen ohne diese Prüfung nicht gemerged werden:

- jede Änderung mit "Important"-Befund
- widersprüchliche Einschätzungen zwischen mehreren Review-Rollen (z. B. Security-Review sagt "unkritisch", Architektur-Review sagt "kritisch")
- Änderungen an Authentifizierung, Zahlungsabwicklung, personenbezogener Datenverarbeitung oder Produktionsdaten-Migrationen – unabhängig vom Ausgang des automatisierten Reviews
- jede Story, bei der die Spezifikation im Backlog erkennbar unvollständig oder mehrdeutig war

## Mindestkompetenz und Freigabe-Kennzeichnung

- Menschliche Freigabe erfordert: Werkzeugausgaben lesen und einordnen können, die eigene fachliche Urteilsgrenze erkennen.
- "Kann ich nicht beurteilen" ist ein zulässiges Ergebnis – es eskaliert (siehe oben), keine Freigabe unter Unsicherheit.
- Jeder Important-Befund trägt eine Herkunftskennzeichnung: `[Werkzeug]` oder `[Einschätzung]`.
- Eine Freigabe unter Vertretung (Reviewer ≠ ursprünglich vorgesehene Person) oder ohne volles fachliches Urteil wird im Kommentar ausdrücklich als solche gekennzeichnet – die Vertretungsregel selbst steht in Story 1.1.

## Zusammenfassungs-Format

Review-Text mit einer einzeiligen Bilanz beginnen, z. B. `1 kritisch, 3 Nits`, und bei Fehlen kritischer Funde explizit mit "Keine kritischen Probleme" einleiten – der Autor soll die Form der Rückmeldung sofort erfassen können.

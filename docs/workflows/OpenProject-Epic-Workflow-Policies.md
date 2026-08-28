# Epic Workflow Policies — OpenProject

Status: **Live** — umgesetzt am 26.08.2026 auf `openproject.webteam.de`, für alle Epics in den Boards [Epic-Board Stattreisen Web](https://openproject.webteam.de/projects/stattreisen-web/boards/14) und [Epic-Board AI Proof of Concept](https://openproject.webteam.de/projects/ai-it-poc/boards/17).

## Zweck

Dieses Dokument beschreibt, was jede Spalte des Epic-Boards bedeutet und unter welcher Bedingung ein Epic in sie hinein- bzw. aus ihr herausbewegt wird. Diese Regeln sind **explizite Policies**: Sie stehen bewusst hier schriftlich, damit alle, die mit Epics arbeiten — nicht nur wer sie zuletzt verschoben hat — dieselbe Erwartung daran haben, was ein Status bedeutet und wann ein Wechsel angebracht ist.

Die Spalten in der Reihenfolge des Boards: **New → Refinement → Ready → In Progress → Developed → Review → Done**, mit den Sonderspalten **On Hold** und **Rejected**.

---

## 1. New

**Bedeutung:** Unbearbeitete Idee. Noch niemand hat sich inhaltlich damit befasst.

**Eintritt:** Jedes neu angelegte Epic startet automatisch hier.

**Austritt:** Nach Refinement, sobald jemand beginnt, Akzeptanzkriterien zu schärfen.

**Policy:** Keine Zeitbuchung — hier ist noch keine Arbeit geleistet.

---

## 2. Refinement

**Bedeutung:** Akzeptanzkriterien werden geschärft; das Epic wird greifbar gemacht.

**Eintritt:** Aus New, sobald die inhaltliche Auseinandersetzung beginnt. Auch Rücksprung aus On Hold, wenn ein zurückgestelltes Epic wieder aufgenommen wird.

**Austritt:** Nach Ready, sobald Umfang und Kriterien klar genug sind, um es einzuplanen. Oder nach Rejected, wenn sich in der Schärfung zeigt, dass das Epic nicht angegangen wird.

**Policy:** **Rejected wird ausschließlich von hier aus erreicht** — eine Ablehnung ist immer das Ergebnis einer inhaltlichen Prüfung, nie eine Spontanentscheidung aus einem anderen Status heraus.

---

## 3. Ready

**Bedeutung:** Fertig geschärft, wartet nur noch auf freie Kapazität.

**Eintritt:** Aus Refinement, sobald das Epic einplanbar ist.

**Austritt:** Nach In Progress, sobald jemand die Arbeit tatsächlich aufnimmt.

**Policy:** **Reine Warteschlange — keine Zeitbuchung.** Ein Epic „liegt" hier nur; die Liegezeit soll nicht in die Aufwandszahlen einfließen.

---

## 4. In Progress

**Bedeutung:** Aktive Umsetzung läuft.

**Eintritt:** Aus Ready, wenn die Arbeit beginnt. Auch Rücksprung aus Done, wenn ein bereits akzeptiertes Epic wieder geöffnet wird (siehe Done).

**Austritt:** Nach Developed, sobald die Umsetzung abgeschlossen ist.

**Policy:** Da das Team klein ist, sollten nicht zu viele Epics gleichzeitig hier stehen. Das Tool erzwingt aktuell kein technisches Limit (siehe „Offene Punkte" unten) — die Zurückhaltung ist eine Team-Absprache, keine Systemregel.

---

## 5. Developed

**Bedeutung:** Umsetzung fertig, wartet auf Review.

**Eintritt:** Aus In Progress, sobald implementiert ist.

**Austritt:** Nach Review, sobald sich jemand der Prüfung annimmt.

**Policy:** **Reine Warteschlange — keine Zeitbuchung**, aus demselben Grund wie bei Ready: Wartezeit auf einen Reviewer ist keine geleistete Arbeit.

---

## 6. Review

**Bedeutung:** Cross-Review bzw. Abnahme läuft — **immer mit menschlicher Freigabe**, kein automatischer Durchlauf.

**Eintritt:** Aus Developed.

**Austritt:** Nach Done, sobald die Prüfung besteht. Oder nach On Hold, wenn sich zeigt, dass das Epic grundsätzlich überdacht werden muss statt nur nachgebessert zu werden.

**Policy:**
- Zeit wird unter dem Aktivitätstyp **„Review"** gebucht.
- Solange noch Nacharbeit nötig ist, um die Prüfung zu bestehen, **bleibt das Epic in Review** — dafür werden zusätzliche Stories/Tasks unter dem Epic angelegt, das Epic selbst wandert nicht hin und her.
- Es gibt bewusst **keine eigene „Escalated"-Spalte**: Auf Epic-Ebene endet Review immer mit einer klaren menschlichen Entscheidung. Eine Situation, die eine Entscheidung außerhalb des unmittelbaren Teams braucht (z. B. Freigabe von externer Stelle), läuft über On Hold, nicht über eine Eskalationsspalte.

---

## 7. Done

**Bedeutung:** Abgenommen.

**Eintritt:** Aus Review, sobald die Prüfung besteht.

**Austritt:** Zurück nach In Progress, falls das Epic wieder geöffnet werden muss (z. B. ein Problem wird nach der Abnahme entdeckt).

**Policy:** Eine Wiedereröffnung wird unter dem Aktivitätstyp **„Rework after merge"** gebucht — damit bleibt sichtbar, dass es sich um Nacharbeit an bereits Abgenommenem handelt, nicht um reguläre Erstumsetzung.

---

## 8. On Hold

**Bedeutung:** Bewegt sich gerade nicht — entweder von außen blockiert, oder es fehlt eine Entscheidung, die keine reine fachliche Review-Entscheidung ist (z. B. Freigabe von außerhalb des unmittelbaren Teams), oder das Epic muss nach Review grundsätzlich überdacht werden.

**Eintritt:** Aus Review (wenn Überdenken nötig ist) oder aus jedem anderen Status, sobald eine externe Blockade oder eine ausstehende Nicht-Review-Entscheidung auftritt.

**Austritt:** Zurück nach Refinement, sobald das Hindernis beseitigt bzw. die Entscheidung getroffen ist.

**Policy:** Die Wiederaufnahme führt **immer über Refinement**, nicht direkt zurück in Review — „Überdenken" heißt, Umfang und Akzeptanzkriterien nochmal anzusehen, nicht nahtlos weiterzumachen, wo man aufgehört hat.

---

## 9. Rejected

**Bedeutung:** Terminal — es wurde entschieden, das Epic nicht anzugehen.

**Eintritt:** Ausschließlich aus Refinement (siehe dortige Policy).

**Austritt:** Keiner vorgesehen. Ob ein zurückgestelltes (On Hold) statt abgelehntes Epic ebenfalls hierher wandern können soll, ist noch nicht festgelegt.

---

## Übergreifende Policies

- **Alle Statuswechsel sind im System technisch erlaubt, in beide Richtungen.** Die oben beschriebenen Pfade sind der vorgesehene Ablauf — keine erzwungene Regel. Diese Offenheit ist eine bewusste Entscheidung, weil zu enge Workflow-Einschränkungen in der Vergangenheit zu Reibung geführt haben. Zeigt sich in der Praxis, dass bestimmte Sprünge (z. B. Spalten überspringen, rückwärts springen außerhalb der vorgesehenen Pfade) ein echtes Problem sind, wird die Workflow-Matrix gezielt für genau diese Status-Paare eingeschränkt — nicht pauschal.
- **Warteschlangen-Spalten (Ready, Developed) sind zeitbuchungsfrei.** Das hält Liegezeiten aus den Aufwandszahlen heraus.
- **WIP-Empfehlung ohne technisches Limit:** Für In Progress und Review gilt die Team-Absprache, wenig gleichzeitig offen zu halten. Ein hartes WIP-Limit im Tool ist auf dieser OpenProject-Instanz für Action-Boards derzeit nicht verfügbar (kein entsprechender Menüpunkt, von der API stillschweigend verworfen) — die Begrenzung ist Selbstdisziplin im Team, keine Systemsperre.

---

## Umgesetzte Konfiguration (Referenz)

- Statustyp Epic nutzt genau die neun oben beschriebenen Status plus „Specified" (Altlast aus einem einzelnen Demo-Epic im irrelevanten Scrum-Testprojekt, taucht auf keinem echten Board auf).
- Workflow-Matrix für Epic: alle Übergänge zwischen allen neun Status, beide Richtungen, für die Rollen Member und Project admin.
- Beide Epic-Boards zeigen die neun Spalten in obiger Reihenfolge.

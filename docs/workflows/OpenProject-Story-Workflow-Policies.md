# Story Workflow Policies — OpenProject

Status: **Live** — umgesetzt am 26.08.2026 auf `openproject.webteam.de`, für alle Stories in den Boards [Story-Board Stattreisen Web](https://openproject.webteam.de/projects/stattreisen-web/boards/10) und [Story-Board AI Proof of Concept](https://openproject.webteam.de/projects/ai-it-poc/boards/18).

**Vollständige Bereinigung statt additiver Erweiterung:** Der erste Umsetzungsversuch beließ die alten 13 Status additiv neben den neuen neun, um die rund 150 historischen Tickets in „Stattreisen Web" nicht anfassen zu müssen. Reinhard hat das zurückgewiesen: Eine Statusauswahl, die nicht mit den Board-Spalten übereinstimmt, ist für die Planung unbrauchbar. Daher wurde vollständig migriert: Alle 186 Stories (beide Projekte plus die paar im irrelevanten Demo-„Scrum project") wurden auf die neun Ziel-Status umgestellt (Closed/Tested → Done, Specified/Confirmed → Ready, In specification → Refinement, In testing → Review; New/In progress/Review/Rejected blieben unverändert), anschließend wurden die alten 13 Status komplett vom Story-Typ entfernt. Der Story-Typ kennt jetzt ausschließlich die neun Ziel-Status — Statusauswahl und Board-Spalten stimmen exakt überein, in beiden Projekten.

Spec reference: the underlying acceptance criteria and rationale for this workflow are recorded in user story [#190](https://openproject.webteam.de/work_packages/190). Keep this document in sync with that story if it changes.

## Purpose

This document describes what each column of the story board means and under which condition a story moves into or out of it. As with the epic board, these are explicit policies, written down so that everyone working with stories — not just whoever last moved one — shares the same expectation of what a status means and when a transition is warranted.

The columns in board order: **New → Refinement → Ready → In Progress → Review → Done**, with the branch columns **Rework**, **Escalated**, and **Rejected**.

---

## 1. New

**Meaning:** Raw backlog item. Nobody has engaged with its content yet.

**Entry:** Every newly created story starts here automatically.

**Exit:** To Refinement, once someone starts sharpening its acceptance criteria.

**Policy:** No time booking — no work has happened yet.

---

## 2. Refinement

**Meaning:** Acceptance criteria are being written and sharpened; the story becomes concrete enough to schedule and implement. Maps to activity type **Specification**.

**Entry:** From New, once work on its content begins.

**Exit:** To Ready, once scope and acceptance criteria are clear enough to schedule. Or to Rejected, if refinement shows the story isn't needed.

**Policy:** **Rejected is reached exclusively from here** — a rejection is always the result of looking at the content, never a snap decision from another status.

---

## 3. Ready

**Meaning:** Fully specified, waiting only for free capacity.

**Entry:** From Refinement, once the story is schedulable.

**Exit:** To In Progress, once work actually starts.

**Policy:** **Pure queue — no time booking.** A story just sits here; queue time should not flow into the effort numbers.

---

## 4. In Progress

**Meaning:** Active implementation, by an implementer agent plus the person responsible.

**Entry:** From Ready, when work starts — this also starts an implementer agent session on a branch named with the story ID. Also re-entered from Rework (changes requested, or the test pipeline failed) and from Done (a reopened story).

**Exit:** To Review, once implementation is finished and a pull request is opened.

**Policy:** Keep few stories open here at once — a team agreement, not a system limit. (Same finding as the epic board rollout: this OpenProject instance does not enforce a hard WIP limit on action boards; the API silently drops it.)

---

## 5. Review

**Meaning:** Cross-review / approval in progress, PR open. Always ends in a required human reviewer decision — never an automatic merge on agent approval alone. Maps to activity type **Review**.

**Entry:** From In Progress, once the PR is opened. On entry: automated review agents run per `REVIEW.md`; the deputy reviewer (whoever did *not* specify this story) is assigned as the required human reviewer; the story is auto-deployed to a staging/preview environment so the reviewer can click through the actual change, not just read the diff.

**Exit:** To Done, once it passes. To Rework, if changes are requested or the automated test pipeline fails. To Escalated, if the reviewer can't judge with confidence, or a gate conflict arises.

**Policy:**
- Reading tool and test output is explicitly part of what "reviewing" means here — stated outright, not left implicit.
- **Merging (exit to Done) requires the automated test pipeline to be green** — a hard gate, consistent with the pilot's existing rule that the test pipeline is a merge precondition, not an add-on. The pipeline runs in the background while the story sits here; it is not a separate board state.
- **A pipeline failure while a story is in Review automatically sends it to Rework** — it does not sit silently waiting for a human to notice.
- "I can't judge this" is a valid, expected outcome at this level (unlike at epic level) — it goes to Escalated instead of being guessed at.

---

## 6. Rework

**Meaning:** Review requested changes, or the test pipeline failed; back with the implementer.

**Entry:** From Review — either an explicit change request, or automatically on a pipeline failure.

**Exit:** To In Progress, once picked back up.

**Policy:** The same implementer continues here; this loop does not reassign the deputy reviewer.

---

## 7. Escalated

**Meaning:** The reviewer can't judge with confidence, or there's a gate conflict. A real, expected outcome at story level — unlike the epic board, where review always ends in a confident human decision, review here can be led by an agent or by a reviewer without full technical confidence. Maps to activity type **Escalation**.

**Entry:** From Review.

**Exit:** Not yet defined.

**Policy:** **Open point:** who is notified and who ultimately decides from here is not yet defined. Needed before this state can be wired into automation.

---

## 8. Done

**Meaning:** Merged and accepted — the code is correct and approved. This does **not** mean it is live for site visitors.

**Entry:** From Review, once it passes (tests green, human sign-off given).

**Exit:** Back to In Progress, if reopened (a problem found after acceptance).

**Policy:**
- A reopening is booked under activity type **Rework after merge**.
- **Going live is a separate, batched action, owned by Epic 10 (Umgebungen, Deployment und Betrieb) — not a story-board state.** Done stories accumulate until a deliberate release step publishes them; this board does not represent that step.

---

## 9. Rejected

**Meaning:** Terminal — decided the story isn't needed.

**Entry:** Exclusively from Refinement (see policy there).

**Exit:** None defined.

**Policy:** Mirrors the epic board's rule — no rejection from any status other than Refinement.

---

## Cross-cutting policies

- **All status changes are technically allowed in the system, in both directions.** The paths described above are the intended flow, not an enforced rule — same policy as the epic board, deliberately, because overly tight workflow restrictions have caused friction before. If a specific jump turns out to be a real problem in practice, the workflow matrix gets restricted for exactly that pair — not across the board.
- **Ready is the only pure no-booking queue at story level** — unlike the epic board, there's no separate "implemented, waiting for review" column here, since review begins as soon as the PR opens.
- **WIP is a team agreement, not a system limit** — no hard WIP limit is available for action boards on this OpenProject instance.
- **Testing:** the automated pipeline is a hard precondition for Review → Done, and a pipeline failure while in Review auto-triggers Rework. What is actually tested and how much remains Epic 9's job (Teststrategie und Umsetzungsrahmen), not fixed by this document.
- **Deployment:** Done means merged and accepted, not live. Release to production is a separate, batched action under Epic 10. The one deployment-related automation this board does own is deploying each PR to staging on entering Review.
- **Escalation ownership is not yet defined** — open item, listed under column 7.
- **Backlog connector not yet built** — none of the agent automation triggers described above fire by themselves yet. OpenProject status changes aren't natively watched; that needs an MCP connector or a scheduled script, which doesn't exist yet.

---

## Umgesetzte Konfiguration (Referenz)

- Status-Typ User story nutzt ausschließlich die neun oben beschriebenen Status (Rework und Escalated neu angelegt; New, Refinement, Ready, In progress, Review, Done, Rejected bereits vorhanden). Die alten 13 Status (In specification, Specified, Confirmed, Scheduled, Developed, In testing, Tested, Test failed, Closed, On hold, plus To be scheduled — nie zugeordnet) wurden vom Typ entfernt, nachdem alle betroffenen Work Packages migriert waren.
- Workflow-Matrix für User story: alle Übergänge zwischen den neun Ziel-Status, beide Richtungen, für die Rollen Member und Project admin — analog zum Epic-Typ.
- Beide Story-Boards (Stattreisen Web und AI Proof of Concept) zeigen die neun Spalten in obiger Reihenfolge.
- Migration: 99 von 186 Work Packages vom Typ User story wurden per API auf einen neuen Status umgestellt (Mapping siehe oben); die übrigen 87 waren bereits auf einem der neun Ziel-Status.
- Statusanlage und Status-Entfernung vom Typ über die Browser-Admin-UI (Statuses/Workflows haben keine v3-API), Work-Package-Migration und Board-Umbau (Queries + Grid-Widgets) über die OpenProject-API — durchgeführt von Claude Code.

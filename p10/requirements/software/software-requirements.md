# Software Requirements — P10 "Aufgaben bearbeiten im HMI" (extension of platform baseline)

*Extends SWR-001–076. Language: English (D011). Status `draft` until G1 (T-0002); verification coverage lands with sprint 1. v1.0 Sprint 0, T-0001.*

| ID | Requirement | Trace | Verification | Prio | Status |
|---|---|---|---|---|---|
| SWR-077 | Mission Control shall offer an editor for **open** tickets (status not `done`/`rejected`) covering title, type, priority, role, sprint, cadence, status and body. Every write shall pass the same validation as `board.py` (field schema, vocabularies, allowed status transitions, decision-request extras) and shall be rejected with the identical German message on violation; tickets in a closed state shall be read-only except through the allowed `done → in_progress` reopening transition. | STK-020 | Unit tests (valid edit, each rejection case, read-only closed ticket) + UI checklist | high | draft |
| SWR-078 | Every accepted edit shall be written to the ticket file and committed to the enclosing git repository as a single commit whose message names the ticket and the origin ("Mensch via HMI"), with `BOARD.md` regenerated in the same commit; a failing write or commit shall leave the working copy unchanged and report the failure in plain German. | STK-020 | Unit tests (commit created, board regenerated, rollback on failure) + `git log` checklist | high | draft |
| SWR-079 | Tickets shall carry an optional `labels:` list of freely chosen labels (including team/project assignment); the board shall show them per card and offer a filter over the labels present, combinable with the existing filters. Labels shall be editable in the HMI, validated against a conservative character set, and tickets without the field shall behave exactly as before. | STK-020 | Unit tests (parsing, validation, filter logic, absent field unchanged) + UI checklist | high | draft |
| SWR-080 | Before writing, the editor shall verify that the ticket on disk still matches the state the form was loaded from (content fingerprint) and, on mismatch, refuse the write and report the conflict in plain German naming the ticket and offering to reload — so a concurrent routine session and the human never silently overwrite each other. | STK-020 | Unit tests (unchanged file writes, modified file refuses, message content) + forced-conflict checklist | high | draft |
| SWR-081 | Write access to tickets shall require the same PIN protection as the configurator (LAN operation, ADR-006 delta); read access shall remain unauthenticated. Each edit shall be visible in the ticket detail as a history entry (timestamp, origin, changed fields), independent of the git history. | STK-020 | Unit tests (PIN required for write, read unaffected, history entry written) + UI checklist | medium | draft |

## Traceability

STK-020 ← SWR-077–081 (complete; no orphans). Status stays `draft` until the G1 decision (T-0002) — DoD applied 2026-08-16 (RM), review by QM pending with G1.

## Open points for sprint 1

- **ADR required:** second write path to tickets next to the script route (validation reuse, commit identity, conflict strategy) — ADR-007 candidate, architecture picture drift gate applies.
- "New project" stays out of the type vocabulary (intake route: letter → qualification → G0 DR); the wish from pm/N-0013 is served by a label instead — to be confirmed at G1.

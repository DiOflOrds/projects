# Software Requirements — P10 "Aufgaben bearbeiten im HMI" (extension of platform baseline)

*Extends SWR-001–076. Language: English (D011). v1.1 Sprint 1 (T-0003): every requirement moved to `reviewed` individually, together with the unit tests that verify it (`platform/tests/test_p10_editor.py`) — see the traceability note below.*

| ID | Requirement | Trace | Verification | Prio | Status |
|---|---|---|---|---|---|
| SWR-077 | Mission Control shall offer an editor for **open** tickets (status not `done`/`rejected`) covering title, type, priority, role, sprint, cadence, status and body. Every write shall pass the same validation as `board.py` (field schema, vocabularies, allowed status transitions, decision-request extras) and shall be rejected with the identical German message on violation; tickets in a closed state shall be read-only except through the allowed `done → in_progress` reopening transition. | STK-020 | Unit tests (valid edit, each rejection case, read-only closed ticket) + UI checklist | high | reviewed |
| SWR-078 | Every accepted edit shall be written to the ticket file and committed to the enclosing git repository as a single commit whose message names the ticket and the origin ("Mensch via HMI"), with `BOARD.md` regenerated in the same commit; a failing write or commit shall leave the working copy unchanged and report the failure in plain German. | STK-020 | Unit tests (commit created, board regenerated, rollback on failure) + `git log` checklist | high | reviewed |
| SWR-079 | Tickets shall carry an optional `labels:` list of freely chosen labels (including team/project assignment); the board shall show them per card and offer a filter over the labels present, combinable with the existing filters. Labels shall be editable in the HMI, validated against a conservative character set, and tickets without the field shall behave exactly as before. | STK-020 | Unit tests (parsing, validation, filter logic, absent field unchanged) + UI checklist | high | reviewed |
| SWR-080 | Before writing, the editor shall verify that the ticket on disk still matches the state the form was loaded from (content fingerprint) and, on mismatch, refuse the write and report the conflict in plain German naming the ticket and offering to reload — so a concurrent routine session and the human never silently overwrite each other. | STK-020 | Unit tests (unchanged file writes, modified file refuses, message content) + forced-conflict checklist | high | reviewed |
| SWR-081 | Write access to tickets shall require the same PIN protection as the configurator (LAN operation, ADR-006 delta); read access shall remain unauthenticated. Each edit shall be visible in the ticket detail as a history entry (timestamp, origin, changed fields), independent of the git history. | STK-020 | Unit tests (PIN required for write, read unaffected, history entry written) + UI checklist | medium | reviewed |

## Traceability

STK-020 ← SWR-077–081 (complete; no orphans). DoD applied 2026-08-16 (RM).

**G1 erteilt: D001/G1a am 2026-08-16 08:18** (T-0002) — Sprint 1 ist beauftragt.

*Korrektur der ursprünglichen Zusage (PM-Beschluss B027): Hier stand „Status stays `draft` until
the G1 decision". Wörtlich befolgt hätte das jetzt alle fünf SWRs auf `reviewed` gesetzt — mit dem
Ergebnis, dass die SWR↔Test-Matrix sie als „manuelle Abnahme dokumentiert" ausgewiesen hätte,
obwohl nichts davon gebaut, getestet oder abgenommen ist. Das Lücken-Gate wäre grün geblieben und
hätte fünf leere Anforderungen gedeckt — genau die Art blinder Fleck, die heute schon zweimal
aufgefallen ist (p9/T-0007, pm/T-0017). Deshalb: **Jede SWR wechselt einzeln auf `reviewed`, wenn
Sprint 1 sie mit ihrem Nachweis liefert.** Die G1-Freigabe ist damit nicht abgeschwächt — sie
beauftragt den Sprint; der Status der Anforderung bezeugt weiterhin nur, was tatsächlich
verifiziert ist.*

**Sprint-1-Nachweis (T-0003, 2026-08-16):** Jede der fünf Anforderungen steht jetzt einzeln auf
`reviewed`, weil sie einzeln belegt ist — 33 Unit-Tests in `platform/tests/test_p10_editor.py`
(Gesamtsuite 263, vorher 230), jeder mit SWR-Bezug im Docstring, damit die Matrix sie zuordnet.
Der Wechsel erfolgte damit **nach** dem Nachweis und nicht mit der Freigabe (B027).

## Open points for sprint 1 — erledigt

- **ADR:** `platform/architecture/adr/adr-007-ticket-schreibpfad-hmi.md` (Regeln bleiben in
  `board.py`, Fingerabdruck statt Sperre, Commit mit Rücknahme, PIN über den vorhandenen
  Schreibschutz). Architekturbild neu generiert, Drift-Gate grün.
- **„Neues Projekt" bleibt Label, nicht Ticket-Typ** — mit G1a bestätigt und so umgesetzt: das
  Typ-Vokabular (`board.TYPEN`) ist unverändert, Labels sind frei wählbar.

## Nicht umgesetzt (bewusst, Abgrenzung unverändert)

Ticket **anlegen** und **löschen** im HMI (Nummernvergabe bleibt bei Skript/Session) sowie das
Bearbeiten erledigter Tickets außer der Wiedereröffnung. Labels erscheinen **nicht** als Spalte im
generierten `BOARD.md` — Begründung in ADR-007 Punkt 8 (Formatänderungen am Board haben am 16.08.
bereits alle `board-check`-Workflows rot gemacht; das gehört gebündelt mit `pm/T-0013`).

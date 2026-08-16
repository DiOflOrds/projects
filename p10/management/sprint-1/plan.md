# Sprint-1-Plan P10 — Umsetzung + Abnahme

*2026-08-16, PL. Nach G1 (D001/G1a, 08:18). Erster Sprint im Sammel-Repo `projects` (pm/D003).*

| Ticket | Inhalt | Rolle | Schätzung (E5) |
|---|---|---|---|
| T-0003 | ADR-007 + Umsetzung SWR-077–081: `board.aktualisiere`/`fingerprint`/`zeitpunkt`/Label-Validierung (Skript-Route), Fassade `backend/tickets.py`, Endpunkte `GET /api/ticket/editor` + `POST /api/ticket`, Editor-Ansicht und Label-Filter im HMI, 33 Tests, Architekturbild | arch/dev/test | 120 min |
| T-0004 | DR: G4 Sprint 1 + P10-Abnahme (Baseline `p10-v1.0`) | pl | 10 min |

## Reihenfolge (bewusst)

Erst die Regeln in `board.py`, dann die Fassade, dann das Formular. Umgekehrt wäre die
Versuchung groß gewesen, im Server „schnell" zu validieren — genau Risiko R2.

## Risiken aus Sprint 0 — Stand am Sprint-Ende

| # | Risiko | Stand |
|---|---|---|
| R1 | Schreibkonflikt mit der 30-Minuten-Routine-Session | **behandelt**: SWR-080 als Fingerabdruck-Prüfung umgesetzt, Konflikt in einem Test real erzwungen (Skript-Route schreibt dazwischen) |
| R2 | Zwei Validierungswege laufen auseinander (HMI vs. `board.py`) | **ausgeschlossen durch Bauweise**: Es gibt keinen zweiten Prüfweg — `backend/tickets.py` ruft `board.aktualisiere`; Tests vergleichen die Meldungstexte mit denen der Skript-Route |
| R3 | Erster verschachtelter Projektordner — Werkzeuge sehen evtl. nur Top-Level | **erledigt** in p9/T-0007 + pm/T-0017; Preflight und Matrix erfassen `projects/p10` |

## Offen bei Sprint-Ende

Die Abnahme (G4) ist Klasse A und liegt als `p10/T-0004` in der Inbox. Die Baseline
`p10-v1.0` wird **erst nach** der Entscheidung gesetzt — das Team taggt nichts vorab.

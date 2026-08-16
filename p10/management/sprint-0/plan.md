# Sprint-0-Plan P10 — Anforderungen

*2026-08-16, PL. G0 übernommen (D000/P10a, pm/D005). Erster Projektordner im Sammel-Repo `projects` (pm/D003).*

| Ticket | Inhalt | Rolle | Schätzung (E5) |
|---|---|---|---|
| T-0001 | SWE.1: STK-020 + SWR-077–081 (Editor, Commit, Labels, Konflikt, PIN/Historie) als Entwurf + Auftrag/Plan | rm/pl | 35 min |
| T-0002 | DR: G1 — SWR-Set freigeben (Inbox) | pl | 5 min |

**Sprint 1 danach:** ADR-007 (zweiter Schreibpfad auf Tickets), Backend-Schreib-Endpunkt mit Validierung/Fingerprint/Commit, Label-Feld in `board.py` + Board-Filter, Editor-Ansicht im HMI (Desktop + Handy), PIN-Gate, Tests, Abnahme (G4, Baseline `p10-v1.0`).

## Risiken

| # | Risiko | Gegenmaßnahme |
|---|---|---|
| R1 | Schreibkonflikt mit der 30-Minuten-Routine-Session (beide schreiben dieselben Ticket-Dateien) | SWR-080 (Fingerprint-Prüfung vor dem Schreiben) ist Abnahmekriterium, nicht Kür; erzwungener Konflikt als Stichprobe |
| R2 | Zwei Validierungswege laufen auseinander (HMI vs. `board.py`) | SWR-077 verlangt ausdrücklich Wiederverwendung der `board.py`-Prüfungen statt Nachbau — Prüfpunkt im ADR |
| R3 | Erster verschachtelter Projektordner — Werkzeuge greifen evtl. nur auf Top-Level-Repos | in dieser Session als Befund p9/T-0007 behoben (preflight-Board-Runde + Matrix-Quellen), Tests ergänzt |

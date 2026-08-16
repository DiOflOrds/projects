# Sprint-1-Report P10 — „Aufgaben bearbeiten im HMI"

*2026-08-16, Routine-Session. Tickets T-0003 (in_review) und T-0004 (G4-DR, offen).*

## Ergebnis

Mission Control kann Tickets schreiben. Bis heute war das HMI für Aufgaben bewusst nur lesend;
wer etwas ändern wollte, musste dem Team einen Brief schreiben und auf die nächste Session warten.
Jetzt geht es direkt — mit denselben Regeln, die auch die Skript-Route durchsetzt.

| Anforderung | Geliefert | Nachweis |
|---|---|---|
| SWR-077 Editor + Validierung | Formular im Ticket-Detail für offene Tickets; erledigte sind Archiv (nur Wiedereröffnung) | 8 Tests, u. a. gegen die Meldungstexte von `board.py` |
| SWR-078 Commit + Rücknahme | ein Commit je Änderung, `BOARD.md` mit im selben Commit, Identität „Mensch via HMI" | 4 Tests, inkl. simuliertem Commit-Fehlschlag |
| SWR-079 Labels | freie Mehrfach-Labels, Pillen auf Karte und Detail, Board-Filter | 6 Tests |
| SWR-080 Konflikt | Inhalts-Fingerabdruck, Ablehnung + Klartext + „Ticket neu laden" | 6 Tests, Konflikt real erzwungen |
| SWR-081 PIN + Historie | POST läuft durch den vorhandenen Schreibschutz (ADR-006), Lesen bleibt frei; Historienzeile im Ticket | 9 Tests |

**263 Tests grün** (vorher 230) · Matrix **87 SWRs / 0 Lücken** · Architekturbild neu generiert,
Drift-Gate grün · **0,00 € API** · ~115 min Ist gegen 120 min Schätzung (−4 %).

## Was die Arbeit geprägt hat

Die eigentliche Entscheidung war nicht das Formular, sondern **wo die Regeln leben**. Der bequeme
Weg wäre gewesen, im Server „schnell" zu prüfen — und damit eine zweite Kopie der Validierung
anzulegen. Genau das ist Risiko R2 aus Sprint 0 und die Lesson vom Vormittag (jede Kopie derselben
Logik ist ein künftiger Befund). Deshalb liegt der Schreibpfad in `board.py`, und
`backend/tickets.py` ist wirklich nur Fassade: Projektauflösung, HTTP-Codes, Commit, Rücknahme.

Als Nebeneffekt ist eine bestehende Doppelung verschwunden: Die Zeitstempel-Formatierung aus
SWR-084 lag in `inbox.py`; sie wurde für die Änderungshistorie ein zweites Mal gebraucht.
`inbox.entscheidungszeitpunkt` delegiert jetzt an `board.zeitpunkt` — Entscheidungen und
Ticket-Änderungen datieren aus einer Quelle.

Beim Konflikt-Schutz war die Wahl zwischen Sperre und Fingerabdruck schnell klar: Auf einem Mount,
auf dem sich Dateien nicht löschen lassen (Störung R7, heute Vormittag), wäre eine liegengebliebene
Sperrdatei ein schlimmeres Problem als der Konflikt, den sie verhindern soll.

## Bewusst nicht getan

- Labels erscheinen **nicht** im generierten `BOARD.md` (ADR-007 Punkt 8) — eine Formatänderung am
  Board hat am 16.08. bereits alle `board-check`-Workflows rot gemacht.
- Tickets anlegen/löschen im HMI bleibt draußen (Nummernvergabe), Bearbeiten erledigter Tickets
  ebenfalls.
- Die **Baseline `p10-v1.0` ist nicht gesetzt** — G4 ist Klasse A und liegt als `p10/T-0004` in
  der Inbox. Das Team taggt nichts vorab.

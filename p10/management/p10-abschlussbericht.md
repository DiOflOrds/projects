# Abschlussbericht P10 — „Aufgaben bearbeiten im HMI"

*2026-08-16. Abgenommen mit **G4a (D002, 2026-08-16 10:02, via Inbox)**. Baseline `p10-v1.0` auf `projects` und `platform`. Zehntes abgeschlossenes Projekt; erstes Projekt, das vollständig als Ordner im Sammel-Repo `projects` gelaufen ist (pm/D003).*

## Auftrag und Ergebnis

Auftrag (pm/D005 → D000/P10a): direkter Schreibzugriff auf offene Tickets im HMI, freie Mehrfach-Labels, Typ selbst setzen, Konflikterkennung gegen die parallel laufende Routine-Session. Quelle: die Briefe pm/N-0013 und pm/N-0014.

Geliefert in zwei Sprints an einem Tag:

| Anforderung | Ergebnis |
|---|---|
| SWR-077 Editor + Validierung | Formular im Ticket-Detail für offene Tickets (Titel, Typ, Prio, Rolle, Sprint, Takt, Status, Reviewer, Frist, Fließtext); erledigte Tickets bleiben Archiv — nur Wiedereröffnung |
| SWR-078 Commit + Rücknahme | ein Commit je Änderung, `BOARD.md` im selben Commit, Identität „Mensch via HMI"; scheitert der Commit, werden **beide** Dateien zurückgeschrieben |
| SWR-079 Labels | beliebig viele frei gewählte Labels, Pillen auf Board-Karte und im Detail, Label-Filter kombinierbar mit den vorhandenen Filtern |
| SWR-080 Konflikt | Inhalts-Fingerabdruck vor jedem Schreiben; bei Abweichung Klartext-Meldung und „Ticket neu laden" statt stillem Überschreiben |
| SWR-081 Schutz + Historie | Schreiben über den vorhandenen Schreibschutz (ADR-006), Lesen bleibt PIN-frei; Historienzeile im Ticket, unabhängig von Git lesbar |

**263 Tests grün** (vorher 230) · Matrix **87 SWRs / 0 Lücken** · Architekturbild neu generiert, Drift-Gate grün · **0,00 € API** · Aufwand ~150 min über beide Sprints (35 + ~115) gegen 155 min geschätzt.

## Was dieses Projekt gelehrt hat

**Ein zweiter Zugang ist noch kein zweites Regelwerk.** Der bequeme Bau wäre gewesen, die Eingabeprüfung im Server zu schreiben — Risiko R2 des Sprint-0-Plans, und derselbe Fehlertyp, der am 16.08. zweimal Befund war (dieselbe Pfadauflösung an vier Stellen, `p9/T-0007` und `pm/T-0017`). Stattdessen liegt der Schreibpfad in `board.py`; `backend/tickets.py` ist Fassade. Beim Bau fiel auf, dass die Falle schon einmal zugeschnappt war: Die Zeitstempel-Formatierung aus SWR-084 lag in `inbox.py` und wurde für die Änderungshistorie ein zweites Mal gebraucht — sie hat jetzt eine Quelle (`board.zeitpunkt`).

**Der Betrieb hat die Architekturentscheidung mitbestimmt.** Gegen Schreibkonflikte wären Dateisperren der Lehrbuchweg gewesen. Am selben Vormittag hatte ein nicht löschbarer `.git/index.lock` eine ganze Session unverbuchbar gemacht (`pm/T-0023`) — eine liegengebliebene Sperre wäre also schlimmer als der Konflikt, den sie verhindern soll. Der Fingerabdruck kommt ohne Zustand aus.

**Die Nachweisfrage am Ende der Kette.** Der Konflikt-Test stellt den Fall real her: die Skript-Route schreibt zwischen Laden und Speichern. Der Rücknahme-Test lässt `git commit` scheitern und vergleicht Ticket **und** BOARD.md byteweise mit dem Stand davor — nicht „die Funktion tut, was sie soll", sondern „das System steht danach so da, wie es soll".

## Abgrenzung — bewusst nicht geliefert

- **Tickets anlegen und löschen im HMI** (Nummernvergabe bleibt bei Skript/Session) — CR-Kandidat.
- **Labels als Spalte im generierten `BOARD.md`** — eine Formatänderung am Board hat am 16.08. alle `board-check`-Workflows rot gemacht; gehört gebündelt mit `pm/T-0013`/`pm/T-0021`. Im DR ausdrücklich benannt.
- **`frist` wurde ohne Auftrag mit-editierbar** gemacht, damit eine Terminverschiebung nicht in die Skript-Route zwingt. Im DR benannt, mit G4a angenommen.

## Offene Punkte nach der Abnahme

- **`pm/T-0022`** (Projekt-Pool: Kandidat anlegen und starten) ist entsperrt und kann auf diesem Schreibpfad aufsetzen. Achtung: Ein Kandidat zu *starten* ist eine Projektbeauftragung und damit Klasse A — der Knopf darf den Antrag vorbereiten, nicht die Entscheidung ersetzen.
- Die sieben Stichproben aus `p10/T-0004` stehen als Betriebsnachweis offen; sie werden nicht vom Team abgehakt.

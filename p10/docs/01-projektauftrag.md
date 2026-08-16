# Projektauftrag P10 — „Aufgaben bearbeiten im HMI" (v1.0, G0 übernommen)

*2026-08-16, PL. **G0-Äquivalent: pm/T-0011, Option P10a (pm/D005 → p10/D000)** — kein Doppel-G0 (bewährtes P7/P9-Muster). Quellen: Briefe pm/N-0013 („aufgaben sollen dem team/projekt zugeordnet und mehrfach gelabelt werden können … als bug / CR / neues projekt markierbar") und pm/N-0014 („ich würde gerne alle offenen tasks editieren können, aktuell kann ich diese nur lesen").*

## Was und Warum

Mission Control ist für Tickets heute bewusst **nur-lesend**: geschrieben wird ausschließlich über die Skript-Route (`board.py`) und über Sessions. Wer eine Aufgabe umpriorisieren, umbenennen oder einordnen will, muss dem Team einen Brief schreiben und auf die nächste Session warten. P10 gibt dem Auftraggeber den **direkten Schreibzugriff auf offene Tickets** — mit denselben Regeln, die auch das Skript durchsetzt.

Der Kern ist deshalb nicht ein Formular, sondern ein **zweiter Schreibpfad neben der Skript-Route** (ADR-Kandidat): gleiche Validierung, gleiche Status-Übergänge, Git-Commit je Änderung mit erkennbarer Identität, PIN-Schutz im LAN, Historie — und **Konflikterkennung**, weil die 30-Minuten-Routine-Session in dieselben Dateien schreibt (Risiko-Hinweis aus pm/T-0011, ausdrücklich im Umfang).

Dazu kommen die beiden Ordnungswünsche aus N-0013: **freie Mehrfach-Labels** je Ticket (inkl. Team-/Projekt-Zuordnung) mit Filter im Board, und das **Setzen des Typs** (task / problem / change-request / …) im HMI. „Neues Projekt" wird bewusst **kein** Ticket-Typ — dafür gibt es den Intake-Weg (Brief → Qualifizierung → G0-DR); der Wunsch wird über ein Label abgebildet.

## Abnahmekriterien

1. **Editor:** Ein offenes Ticket lässt sich im HMI ändern (Titel, Typ, Prio, Rolle, Sprint, Status über die erlaubten Übergänge, Fließtext); ungültige Eingaben werden mit derselben Meldung abgelehnt wie bei `board.py` (Stichprobe Browser + Handy).
2. **Commit:** Jede Änderung landet als eigener Git-Commit mit erkennbarer Identität („Mensch via HMI"), BOARD.md wird mitgezogen (Stichprobe: `git log` zeigt die Änderung).
3. **Labels:** Ein Ticket trägt beliebig viele Labels inkl. Team-/Projekt-Zuordnung; das Board filtert danach (Stichprobe).
4. **Konflikt:** Ändert die Routine-Session ein Ticket, während es im HMI offen ist, meldet das HMI den Konflikt in Klartext-Deutsch — nie stilles Überschreiben (Stichprobe: erzwungener Konflikt).
5. **Schutz:** Schreibende Aktionen sind PIN-geschützt wie der Konfigurator; nur-lesender Zugriff bleibt ohne PIN.
6. Requirements-first (STK-020, SWR-077–081, Matrix 0 Lücken), Gates als Inbox-DRs, Aufwandsschätzung je Planning, 0,00 € API.

## Rahmen

2 Sprints. **S0 (heute):** Anforderungen + G1-DR. **S1:** ADR (zweiter Schreibpfad), Umsetzung Backend/Frontend, Tests, Abnahme (G4, Baseline `p10-v1.0`).

**Umsetzung als Ordner `projects/p10`** — erster Vollzug von pm/D003 (Sammel-Repo). Der Auftraggeber legt **kein** GitHub-Repo an; Voraussetzung bleibt das einmalig angelegte Repo `DiOflOrds/projects`.

## Abgrenzung (nicht im Umfang)

- Bereits erledigt und nicht Teil von P10: Takt-Kennzeichnung (SWR-074), Ausblenden alter erledigter Aufgaben (SWR-075), Inbox-Zähler (SWR-076).
- Ticket **anlegen** im HMI und Löschen: nicht im Umfang (Anlage bleibt Session-/Skript-Route, damit die Ticket-Nummernvergabe eindeutig bleibt) — CR-Kandidat nach der Abnahme.
- Bearbeiten **erledigter** Tickets: nicht im Umfang (Archiv bleibt unangetastet; Wiedereröffnung läuft über den erlaubten Übergang `done → in_progress`).

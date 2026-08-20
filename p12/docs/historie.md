# Historie: P12 „Markdown-Renderer für Briefe/Reports" — Chronik und Lessons Learned

*Projektgedächtnis (Konzept Kap. 5, Template `projekt-historie.md`). Pflicht-Lektüre jeder Rollen-Instanz. Chronik: PL; LeLe: COACH.*

## Steckbrief

- **Auftrag:** Briefe und Sprint-Reports formatiert darstellen wie den Digest — über den vorhandenen Renderer, mit Ticket-Links im Inline-Pass statt eines zweiten Textwegs (steckbrief.yaml)
- **Gegründet:** 2026-08-16 18:04, G0a via Inbox über den „Starten"-Knopf (T-0001 → D000); Pool-Kandidat #7, Quelle P7-LeLe
- **Profil / Datenklasse:** entwicklung / intern
- **Status:** aktiv

## Chronik

| Datum | Ereignis | Beleg |
|---|---|---|
| 2026-08-16 | G0a — Projekt beauftragt (der Knopf hat nichts entschieden, die Freigabe kam vom Auftraggeber) | T-0001, D000 |
| 2026-08-20 | Sprint 24: T-0011 steht bei der vierten Berührung — benannt und nicht begründet | PROJEKTSTATUS Sprint 24 |

## Lessons Learned (projektbezogen)

| # | Lehre (ein Satz) | Quelle | Übernommen nach |
|---|---|---|---|
| 1 | Ein Werkzeug-Knopf darf gründen, aber nie entscheiden — Freigaben kommen über die Inbox. | T-0001 / pm/T-0037 | B029-Umfeld |

## Offene Fäden

- **T-0011:** vierte Berührung ohne Begründung im Plan — vor der nächsten Terminierung Entscheid oder Schnitt.
- Umstellung der sieben `preMitLinks`-Aufrufstellen steht aus; Reihenfolge-Empfehlung: erst Inline-Pass um Ticket-Links erweitern, dann Ansichten einzeln.

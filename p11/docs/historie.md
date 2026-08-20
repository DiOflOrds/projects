# Historie: P11 „Widget-Dashboard" — Chronik und Lessons Learned

*Projektgedächtnis (Konzept Kap. 5, Template `projekt-historie.md`). Pflicht-Lektüre jeder Rollen-Instanz. Chronik: PL; LeLe: COACH. Verweist auf Decision-Log/Reports, dupliziert nicht.*

## Steckbrief

- **Auftrag:** Kompakte, nicht scrollbare Übersicht aller Projekte/Teams als konfigurierbare Widgets; Mail-Widget hinter dem PIN-Lesegate (steckbrief.yaml)
- **Gegründet:** 2026-08-16, G0a via Inbox (pm/T-0033 → pm/D007 → p11/D000); Pool-Kandidat #13
- **Profil / Datenklasse:** entwicklung / intern (Auflage: Mail-Inhalte nur zur Laufzeit, nie ins Repo — sonst `sensibel` + Remote-Verlust)
- **Status:** aktiv

## Chronik

| Datum | Ereignis | Beleg |
|---|---|---|
| 2026-08-16 | G0a — Projekt beauftragt; Requirements-first: STK-021 + SWR-092–096 als Entwurf | pm/D007, D000, T-0001 |
| 2026-08-16 | G1a — Anforderungs-Baseline | D001, T-0002 |
| 2026-08-17 | Layout-Entscheid LAY-a (FHD-Entwurf) | D002, T-0006, architecture/layout-entwurf-fhd.md |
| 2026-08-17 | Sprint 19: T-0011 gebaut (SWR-151, liest `/api/widgets`) → `/api/dashboard` ohne Leser; Rückbau-Entscheid Option B (Klasse C) | T-0014 |
| 2026-08-20 | Sprint 24: Rückbau vollzogen — `GET /api/dashboard`, `aggregation.dashboard`, `KACHEL_FELDER` entfernt; SWR-135 auf Layout-Hälfte zurückgeschnitten (v1.61); Wächter als Paar mit echter Auswertung | T-0015, SWR-135, L-2026-08-20by |

## Lessons Learned (projektbezogen)

| # | Lehre (ein Satz) | Quelle | Übernommen nach |
|---|---|---|---|
| 1 | Eine Prüfung, die nur Abwesenheit misst, ist nach einem Kahlschlag ebenfalls grün — Wächter brauchen echte Auswertung, nicht nur Fehl-Listen. | T-0015 | L-2026-08-20by |
| 2 | Ein Endpunkt ohne Leser und ohne benannten künftigen Leser ist Rückbau-Kandidat — die Messung entscheidet, nicht die Meinung. | T-0014 | Decision-Log (Klasse-C-Muster) |

## Offene Fäden

- **T-0016:** Frontend-Hälfte des Rückbaus — gezählt (4 Bausteine, 11 von 111 JS-Zusicherungen), nicht mitgenommen.
- Bewusste Nicht-Entscheidung aus T-0001: Gruppierung/Favoriten bei >14 Einheiten — Kriterium steht (SWR-092), Weg offen.

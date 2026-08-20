# Projektplan: P11 „Widget-Dashboard" (v1.0, Setup-Nachzieh T-0017)

*2026-08-21, PL@p11. Nachgezogen nach Konzept 04 Kap. 4 (Template projektplan.md); Quellen: `docs/01-projektauftrag.md`, SWR-092–096, `docs/historie.md`. Reviewer: QM + PM.*

## 1. Ziele

| # | Ziel | Erfolgskriterium | Quelle |
|---|---|---|---|
| Z1 | Kompakte, nicht scrollbare Übersicht aller Einheiten als konfigurierbare Widgets | SWR-092/093 erfüllt (FHD, Persistenz) | G0 (D000) |
| Z2 | Detailseite je Einheit per Deep-Link | SWR-094 | G0 |
| Z3 | Mail-Widget nur zur Laufzeit hinter PIN, nie im Repo | SWR-095 (Auflage Gründungs-DR) | G0 |
| Z4 | Ehrliche Leere: „keine Daten" statt geratener Null | SWR-096 | G0 |

## 2. Phasen und Meilensteine

| Phase | Inhalt | Meilenstein | Stand |
|---|---|---|---|
| 0 | Requirements + Layout-Entwurf | G1 (D001), LAY-a (D002) | done |
| 1 | Bau Widgets/Detailseite | SWR-151 gebaut | done |
| 2 | Rückbau `/api/dashboard` (Option B) | Backend-Hälfte done (T-0015) | **Frontend-Hälfte offen: T-0016** |
| 3 | Abnahme | G3/G4 | offen |

## 3. Aufgabenstruktur und Workflows

Profil `entwicklung`: Requirements-first (B027: draft bis G1), Reviews nach Rollenmatrix, Wächter als Paare (L-2026-08-20by). Offene Arbeit: T-0016 (Frontend-Rückbau, 4 Bausteine / 11 von 111 JS-Zusicherungen — gezählt, nicht geschätzt), danach G3-Vorbereitung.

## 4. Team und Rollen

Core Team implizit (Konzept 04). Keine Abweichungen vom Default. Projektspezifische Rollen: keine — der fachliche Auftraggeber ist P14 Dashboard (DASH-RED).

## 5. Infrastruktur

Repo `projects/p11` (intern) · Plattform-Pflicht (board.py, Mission Control) · Auflage: Mail-Inhalte nie ins Repo (sonst sensibel + Remote-Verlust).

## 6. Timeline

Nächster Sprint: T-0016 (Frontend-Rückbau). Danach: G3-Vorlage. Kein fester Endtermin — Abnahme entscheidet der Auftraggeber.

## 7. Risiken

| Risiko | Wirkung | Maßnahme | Eigentümer |
|---|---|---|---|
| Frontend-Rückbau löscht Träger fremder Funktionen | Cockpit bricht | Wächter-Paar + Zähltest vor Löschung | DEV/TEST |
| Mail-Inhalt landet versehentlich im Repo | Datenklasse kippt, Remote weg | QM-Stichprobe je Sprint (qm-plan.md) | QM |

## 8. Berichtsweg und Monitoring

PL berichtet je Sprint an PM (Cockpit + Report); Chronikzeile je Sprint in `docs/historie.md`; alle Tickets (auch rollen-eigene) laufen über das Board.

# Projektplan: P12 „Markdown-Renderer für Briefe/Reports" (v1.0, Setup-Nachzieh T-0013)

*2026-08-21, PL@p12. Quellen: `docs/01-projektauftrag.md` (B050), `docs/historie.md`, Decision-Log. Reviewer: QM + PM.*

## 1. Ziele

| # | Ziel | Erfolgskriterium | Quelle |
|---|---|---|---|
| Z1 | Briefe und Sprint-Reports laufen über den einen Renderer | Aufrufstellen umgestellt, Ticket-Links im Inline-Pass erhalten (SWR-098) | G0 (D000) |
| Z2 | Zwei Textwege werden einer — kein Feature-Verlust | Vollständigkeitsnachweis über die NEUEN Textsorten | G0 |

## 2. Phasen und Stand (gemessen 2026-08-21)

| Phase | Inhalt | Stand |
|---|---|---|
| 1 | Inline-Pass vereinheitlicht (tlinks entfiel, SWR-098) | done (Sprint 19) |
| 2 | Briefe + Reports über Block-Renderer | done — **G4 erteilt für p12-v1.0** (T-0010, D003, Option A) |
| 3 | Folgepunkt: Ticket-Body, DR-Body, 2 Dokumentenansichten | **T-0011: done seit Sprint 25** — ⚠ Korrektur zur Ticket-Herkunft von T-0013: Der Sprint-24-Befund „vierte Berührung, nicht begründet" ist ÜBERHOLT; die Sessions 25–27 haben T-0011 geschlossen. Rest-Zustand: 4 Rohtext-Ansichten sind gezählt und abgesichert (`test_renderweg_zaehlung.ROHTEXT_ANSICHTEN = 4`) |

## 3. Aufgabenstruktur und Workflows

Profil `entwicklung`. Es gibt **keine offene Bauarbeit** — das Projekt ist nach G4 im Zustand „abgeschlossen bis auf bewusst offengehaltene Rohtext-Ansichten" (deren Zahl ein Test hält). Neue Arbeit hier entsteht nur per CR.

## 4. Team und Rollen

Core Team implizit; keine Abweichungen, keine projektspezifischen Rollen.

## 5. Infrastruktur

Repo `projects/p12` (intern), Plattform-Pflicht. Renderer-Code lebt in `platform` (app.js) — Änderungen dort sind Plattform-Lieferungen, P12 war der Auftrag.

## 6. Timeline

Keine geplanten Sprints. Kandidat für Status `abgeschlossen` im Steckbrief — **Empfehlung an PM/Auftraggeber** (Klasse B/A): Abschluss formalisieren, sobald T-0013 done ist.

## 7. Risiken

| Risiko | Wirkung | Maßnahme | Eigentümer |
|---|---|---|---|
| Rohtext-Zahl ändert sich nebenbei | zwei Darstellungsgrade driften unbemerkt | Zähltest bleibt (4, rot bei Änderung) | TEST |

## 8. Berichtsweg

PL berichtet an PM; Chronik in `docs/historie.md`.

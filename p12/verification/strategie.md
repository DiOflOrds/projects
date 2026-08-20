# Verifikationsstrategie: P12 (v1.0, Setup-Nachzieh T-0013, TEST@p12)

*2026-08-21. Rückblickend dokumentiert (Projekt nach G4) — beschreibt, was gilt, nicht was geplant wäre.*

## Träger

| Gegenstand | Zusicherung | Ort |
|---|---|---|
| Renderer-Vollständigkeit (Briefe) | `renderer_vollstaendigkeit.test.cjs` (57 Briefe) | platform/tests/js |
| Rohtext-Ansichten-Zahl | `test_renderweg_zaehlung.ROHTEXT_ANSICHTEN = 4` — rot bei Nebenbei-Änderung | platform/tests |
| Inline-Pass (Ticket-Links, SWR-098) | JS-Zusicherungen des Inline-Passes | platform/tests/js |

## Grundsatz

Der Vollständigkeitsnachweis muss über die **jeweilige Textsorte** laufen (T-0011-Lehre:
„ein Nachweis über den falschen Bestand ist grün und sagt nichts") — wer eine weitere
Ansicht umstellt, baut den Nachweis für **deren** Bestand, nicht für Briefe.

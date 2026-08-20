# Verifikationsstrategie: P11 (v1.0, Setup-Nachzieh T-0017, TEST@p11)

*2026-08-21. Ebenen, Abdeckungsanspruch, Automatisierung für P11.*

## Ebenen und Träger

| Ebene | Träger | Lauf |
|---|---|---|
| JS-Zusicherungen (Widgets, Cockpit-Anteile) | `platform/tests/js/` via `js_tests.py` | vor jedem Push (Skript-Route) |
| Python-Unit (Backend-Anteile /api/widgets) | `platform/tests/` | CI + Sprint-Lauf |
| SWR↔Test-Matrix | `trace_matrix.py` | je Sprint; jede reviewed-SWR ohne Abdeckung ist gemeldete Lücke |

## Grundsätze (aus dem Betrieb dieses Projekts)

1. **Wächter sind Paare** — Abwesenheit + weiterlaufender Dienst (T-0015, L-2026-08-20by).
2. **Teststrecken werden umgedreht, nie gelöscht** (L-2026-08-20bz) — der Rückbau T-0016 senkt die Zusicherungszahl von 111 um die 11 gezählten, der Rest bleibt.
3. Verifiziert wird gegen SWR-Verifikationskriterien (SWR-092–096, Stand nach v1.61-Rückschnitt), nicht gegen die Implementierung.

## Aktueller Stand (gemessen 2026-08-21)

111 JS-Zusicherungen; offene Verifikationsarbeit hängt an T-0016 (11 Zusicherungen der Kachelhälfte). Manuelle Prüfungen: keine — alles automatisiert.

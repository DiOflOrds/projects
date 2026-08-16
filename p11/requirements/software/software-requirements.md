# Software Requirements — P11 "Widget-Dashboard" (extension of platform baseline)

*Extends SWR-001–091. Language: English (D011). v1.0, 2026-08-16 (RM) — all requirements `draft`
until G1 (B027).*

| ID | Requirement | Trace | Verification | Prio | Status |
|---|---|---|---|---|---|
| SWR-092 | Mission Control shall offer a dashboard view that renders every project and team as a compact widget on one page, laid out so that the page fits a 1920×1080 viewport **without vertical scrolling** for the full set of projects and teams currently present. All figures shown shall come from the existing `aggregation.cockpit` payload; the dashboard shall not introduce a second collection path. | STK-021 | Unit tests (layout budget computed from the widget set, no second data path) + browser checklist at FHD | high | draft |
| SWR-093 | Each widget shall be individually switchable (on/off) and configurable in what it shows; the setting shall be stored server-side like the configurator settings and shall survive a server restart. Widgets switched off shall not be fetched or rendered, and the remaining widgets shall re-fill the freed space. | STK-021 | Unit tests (persistence, off-widget not rendered, layout after switch-off) + UI checklist | high | draft |
| SWR-094 | The dashboard shall provide a detail page per project and team, reachable by deep link, showing that entity's full cockpit content (tasks, open decisions with deadline traffic light, overdue tasks, open letters, baseline, KPI). The start page shall link to it from each widget. | STK-021 | Unit tests (deep link resolves, content equals cockpit entry) + UI checklist | high | draft |
| SWR-095 | The dashboard shall offer a mail widget showing the summary of `team-mail` for the current cadence. Its content shall be rendered **at runtime only, behind the existing PIN read gate**; without a valid PIN the widget shall show its frame and a plain German note instead of content. No digest text or mail excerpt shall ever be written to a repository, to a cache file or to a log by this feature. | STK-021 | Unit tests (PIN required for content, no content without PIN, nothing written to disk) + `git status` checklist after use | high | draft |
| SWR-096 | A widget shall render exactly the fields defined by the widget contract (`team-dashboard/T-0001`). A project or team that does not deliver a field shall be shown with a plain German "keine Daten" marker for that field instead of an empty cell, a zero or a failing page — so that a missing contribution is visible as missing and not as a value. | STK-021 | Unit tests (missing field marked, no exception, zero vs. missing distinguished) + checklist against a project without contract fields | medium | draft |

## Traceability

STK-021 ← SWR-092–096 (complete; no orphans). DoD applied 2026-08-16 (RM).

**Alle fünf Anforderungen stehen auf `draft` (B027).** G0a beauftragt das Projekt; sie bezeugt
nicht, dass etwas gebaut, getestet oder abgenommen ist. Stünden sie jetzt auf `reviewed`, würde
die SWR↔Test-Matrix sie mangels Unit-Tests als „manuelle Abnahme dokumentiert" ausweisen und das
Lücken-Gate bliebe grün, obwohl nichts dahintersteht — genau der blinde Fleck aus `p9/T-0007`,
`pm/T-0017` und `p10` (B027). Jede SWR wechselt **einzeln** auf `reviewed`, sobald Sprint 1 sie
mit ihrem Nachweis liefert.

## Offene Punkte für Sprint 0 / Sprint 1

- **Die Entwurfsfrage aus `pm/T-0033` Punkt 2 ist offen und gehört in Sprint 0:** Bei 14
  Projekten/Teams passt nicht jedes als volle Kachel auf eine FHD-Seite. Gruppieren? Nur
  Auffälliges zeigen? Wählbare Favoriten? SWR-092 formuliert das Kriterium („passt ohne
  Scrollen"), **nicht** den Weg dorthin — der wird entworfen, bevor gebaut wird. Ein Dashboard,
  das „kompakt" verspricht und dann doch scrollt, wäre der nächste Befund statt der Lieferung.
- **ADR für Sprint 1 offen:** Wo liegt die Layout-/Widget-Logik, damit nicht neben dem Cockpit
  eine zweite Aufbereitung derselben Daten entsteht (Prüffrage aus B033)?
- **SWR-096 hängt am Widget-Vertrag** (`team-dashboard/T-0001`, Frist 23.08.). Die Anforderung
  beschreibt bewusst das **Verhalten bei fehlenden Feldern**, nicht die Feldliste — die kommt aus
  dem Vertrag und ist Eingangsbedingung für Sprint 1, nicht Inhalt dieses Dokuments.

## Nicht im Umfang (Abgrenzung, siehe Projektauftrag)

Zugriff aus dem Internet (Runbook Kap. 10 — eigener Klasse-A-Entscheid), Ersatz oder Umbau des
Org-Cockpits aus P9, neue Kennzahlen oder eine zweite Datenquelle.

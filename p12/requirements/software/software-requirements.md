# Software Requirements — P12 "Markdown renderer for letters and reports" (extension of platform baseline)

*Extends SWR-001–096. Language: English (D011). v1.0, 2026-08-16 (RM) — all requirements `draft`
until G1 (B027).*

| ID | Requirement | Trace | Verification | Prio | Status |
|---|---|---|---|---|---|
| SWR-097 | Mission Control shall render letters (message and reply in the team chat) and sprint reports through the **same** Markdown renderer already used for digest and charter (SWR-059/060), with headings, paragraphs, bold/italic/code, ordered and unordered lists, pipe tables and horizontal rules. After the change the frontend shall contain **exactly one** function that turns Markdown text into DOM; no second render path shall exist beside it. | STK-022 | Static check on the render path (single renderer, call sites enumerated) + UI checklist on a letter with a list and a report with a table | high | draft |
| SWR-098 | Ticket references of the form `T-nnnn` shall be rendered as links to the ticket detail view (SWR-040) **from within the renderer's inline pass**, so that they work in running text, in list items and in table cells alike. The link behaviour shall not be provided by a second wrapper around the raw text. | STK-022 | Tests over running text, list item and table cell + UI sample on `p7/management/sprint-2/report.md` | high | draft |
| SWR-099 | The renderer shall not drop text it does not understand. Fenced code blocks (```) shall be rendered verbatim in monospace without inline interpretation, and any line matching no known pattern shall be rendered as paragraph text. Verification shall assert that the rendered text content contains every non-whitespace character of the source except the markup characters themselves. | STK-022 | Completeness check run over the full stock of sprint reports and letters (currently 21 reports, 38 letters) | high | draft |
| SWR-100 | The renderer shall build its output exclusively through DOM API calls; it shall never assign `innerHTML` or load an external library (ADR-002). Markup contained in the source text — including `<script>` and `<img onerror=...>` — shall appear as literal text. This applies in particular to letters, whose text is free input written by a human through the chat form. | STK-022 | Static check for `innerHTML`/library imports on the render path + test with a letter containing markup | high | draft |
| SWR-101 | Rendering shall change presentation only. No feature of P12 shall write rendered or source text to a repository, a cache file or a log, and the existing PIN read gate for sensitive team content shall keep applying unchanged. | STK-022 | Test asserting nothing is written during rendering + `git status` checklist after use of the views | medium | draft |

## Traceability

STK-022 ← SWR-097–101 (complete; no orphans). DoD applied 2026-08-16 (RM).

**G1 erteilt: D001/G1a am 2026-08-16 19:11** (T-0002) — das Anforderungs-Set ist freigegeben und
Sprint 1 beauftragt (`p12/T-0003`).

**Alle fünf Anforderungen stehen weiterhin auf `draft` (B027) — auch nach G1.** Das ist kein
Versehen: G1a beauftragt den Sprint, es verifiziert keine Anforderung. G0a beauftragt das Projekt; sie bezeugt
nicht, dass etwas gebaut, geprüft oder abgenommen ist. Stünden sie jetzt auf `reviewed`, würde die
SWR↔Test-Matrix sie mangels Tests als „manuelle Abnahme dokumentiert" ausweisen und das
Lücken-Gate bliebe grün, obwohl nichts dahintersteht — der blinde Fleck aus `p9/T-0007`,
`pm/T-0017` und `p10`. Jede SWR wechselt **einzeln** auf `reviewed`, sobald Sprint 1 sie mit ihrem
Nachweis liefert.

## Offene Punkte für Sprint 1

- **Die Teststrecke existiert nicht** (R5 im Sprint-0-Plan). SWR-098/099/100 verlangen Prüfungen
  an JavaScript-Code, und es gibt in der gesamten Organisation **keinen einzigen JS-Test** — das
  ist Pool-Kandidat **#8** („JS-Frontend-Tests", Quelle P3-R1). Wie geprüft wird, ist die **erste**
  Entscheidung in Sprint 1 und gehört in den ADR, nicht in die Umsetzung. Die Verifikationsspalte
  oben nennt deshalb *was* nachgewiesen wird, nicht *womit* — das wäre eine Zusage über ein
  Werkzeug, das es noch nicht gibt (B038-Familie).
- **ADR-Delta zu ADR-002 offen:** Wo genau liegt die Ticket-Erkennung im Inline-Pass, und wie wird
  verhindert, dass beim Umbau ein zweiter Renderpfad zurückbleibt (Prüffrage aus B033)?
- **Reihenfolge in Sprint 1:** erst der Vollständigkeitsnachweis aus SWR-099 gegen den
  **Bestand**, dann die Umstellung — ein Renderer, der Text schluckt, fällt sonst erst am Brief
  auf, in dem es darauf ankam.

## Nicht im Umfang (Abgrenzung, siehe Projektauftrag)

Ticket-Bodys, DR-Bodys und die beiden Dokumentenansichten (benannter Folgepunkt, wird mit dem
G4-Antrag zur Entscheidung gestellt); neuer Markdown-Umfang über Bestand + Code-Blöcke hinaus;
jede Bibliothek (ADR-002, zudem Klasse A als neuer externer Dienst); Umbau des Digest-/Charter-Wegs.

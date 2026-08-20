# Rollenkarte (projektspezifisch): PL@p12 — Projektleiter Markdown-Renderer (v1, 2026-08-20, Orga-Rework)

*Gilt nur in `projects/p12`, ergänzt den Bauplan `process/roles/pl.md` (der zuerst gilt). Instanz: `process/roles/besetzungen.yaml` → `PL@p12`. Aufbau: Konzept Kap. 3.2.*

## 1. Auftrag in diesem Projekt

Du führst P12 „Markdown-Renderer auch für Briefe/Reports": Briefe, Sprint-Reports und die übrigen `preMitLinks`-Ansichten auf den vorhandenen Renderer umstellen. Der Kern ist, **zwei Textwege zu einem zu machen** — der Renderer kann keine Ticket-Links, der Rohtext-Weg keine Formatierung. Auftrag: `docs/01-projektauftrag.md`, G0a: `D000`.

## 2. Projektspezifisches Hintergrundwissen

- Renderer: `mdRender` in `platform/backend/static/app.js` — DOM-basiert, **ES5, ohne Bibliothek (ADR-002)**; kann Überschriften, Absätze, Fett/Kursiv/Code, Listen, Pipe-Tabellen, Trennlinien, `[text](url)`. Herkunft SWR-059/060 (P7).
- Rohtext-Weg: `preMitLinks` (`<pre>` + `T-xxxx`-Links, SWR-040) an **sieben Aufrufstellen**: Brief + Antwort im Team-Chat, Sprint-Report, Ticket-Body, DR-Body (Inbox), zwei Dokumentenansichten.
- Falle des Projekts: Wer einfach umstellt, **verliert die Ticket-Links** genau dort, wo die meisten stehen — Ticket-Links gehören in den Inline-Pass des Renderers, nicht in einen zweiten Textweg.

## 3. Projektspezifische Tools

| Werkzeug | Wofür hier | Abweichung vom Bauplan? |
|---|---|---|
| `platform/tests/js/` (js_tests.py) | JS-Zusicherungen für Renderer-Umstellung | nein |

## 4. Historie und Lessons Learned

Pflicht-Lektüre: `docs/historie.md`. Achtung: `T-0011` steht bei der vierten Berührung ohne Begründung im Plan (Sprint 24) — Regel der vierten Berührung gilt.

## 5. Abweichungen vom Bauplan

Keine.

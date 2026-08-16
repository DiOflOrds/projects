# Sprint-0-Plan P12 — Anforderungen

*2026-08-16, PL. G0 erteilt (`D000`/G0a, 18:04). Dritter Projektordner im Sammel-Repo `projects`
(`pm/D003`) und das **erste Projekt, das über den „Starten"-Knopf** (`pm/T-0022` Teil 2) entstanden
ist — Ordner und G0-Antrag kamen vom Werkzeug, die Freigabe vom Auftraggeber.*

| Ticket | Inhalt | Rolle | Schätzung (E5) |
|---|---|---|---|
| T-0001 | G0-DR (vom Knopf erzeugt) — entschieden G0a, verbucht | pl | — |
| T-0001 (Sprint-0-Arbeit) | SWE.1: STK-022 + SWR-097–101 als Entwurf, Projektauftrag v1.0 mit messbaren Abnahmekriterien, Abgrenzung, Risiken | rm/pl | 30 min |
| T-0002 | DR: G1 — SWR-Set freigeben (Inbox) | pl | 5 min |

**Sprint 1 danach** — Inhalt: ADR-Delta zu ADR-002 (ein Renderpfad, Ticket-Erkennung im
Inline-Pass), **Entscheidung über die Teststrecke (R5, erster Schritt)**, Vollständigkeitsnachweis
gegen den Bestand, Umstellung von Brief/Antwort und Sprint-Report, Tests, Abnahme (G4, Baseline
`p12-v1.0`).

**Keine Eingangsbedingung außerhalb des Projekts.** Anders als P11 wartet P12 auf nichts und
niemanden — der Renderer, die Aufrufstellen und der Textbestand sind alle da.

## Risiken

| # | Risiko | Gegenmaßnahme |
|---|---|---|
| R1 | **Ein zweiter Renderpfad bleibt stehen** — die Umstellung hängt Briefe/Reports auf `mdRender` um, `preMitLinks` bleibt für den Rest liegen, und ab da driften zwei Darstellungen derselben Texte auseinander (B033) | Abnahmekriterium 1 verlangt **genau eine** Funktion; die Aufrufstellen werden im Review aufgezählt, der Prüfpunkt steht im ADR — nicht als Vorsatz, sondern als Gate |
| R2 | **Ticket-Links gehen bei der Umstellung still verloren.** `mdRender` kennt sie nicht; ein Report sieht danach besser aus und ist schlechter benutzbar, und niemand merkt es, weil nichts kaputtgeht — der Link wird einfach Text | SWR-098 verlangt die Erkennung **im Inline-Pass** und den Nachweis in Fließtext, Liste **und** Tabellenzelle; Abnahmekriterium 2 nennt einen realen Report als Stichprobe |
| R3 | **Freitext aus dem Chat wird umgedeutet oder ausgeführt.** Briefe sind menschliche Eingabe: ein `\|` am Zeilenanfang wird zur Tabelle, ein `#` zur Überschrift — und Markup im Brief darf unter keinen Umständen als Element entstehen | SWR-100 verbietet `innerHTML` und Bibliotheken und verlangt eine Probe mit `<script>`/`<img onerror=…>` im Brieftext; die DOM-Bauweise aus P7 bleibt, sie wird nur nicht aufgeweicht |
| R4 | **Stiller Textverlust.** Der Renderer verwirft heute, was er nicht kennt — Code-Blöcke mit ``` gehören dazu, und `platform/N-0002` enthält welche. Ein Brief, aus dem der Befehl verschwindet, um den es ging, ist schlimmer als ein unformatierter Brief | SWR-099 macht Vollständigkeit zum **prüfbaren** Kriterium (jedes nicht-leere Zeichen der Quelle außer Auszeichnung) und lässt den Nachweis gegen den **ganzen Bestand** laufen (21 Reports, 38 Briefe), nicht gegen ein Beispiel |
| R5 | **Die Teststrecke für die eigenen Abnahmekriterien existiert nicht.** SWR-098/099/100 verlangen Prüfungen an JavaScript — die Organisation hat **329 Python-Tests und null JS-Tests**; „JS-Frontend-Tests" ist Pool-Kandidat **#8** (P3-R1) und damit ein eigenes, nicht beauftragtes Vorhaben | **Erste** Arbeit in Sprint 1 ist die Entscheidung *wie geprüft wird*, im ADR und vor jeder Umstellung: Kandidat #8 vorziehen, die Renderregeln in eine ohne Browser prüfbare Form bringen, oder — begründet — auf dokumentierte manuelle Stichproben zurückfallen. **Nicht** gelöst wird es dadurch, dass „Tests" im Kriterium steht und am Ende eine Stichprobe reicht (B027/B038) |
| R6 | **Scope-Kriechen zu Ticket- und DR-Bodys.** Sie stehen daneben, sehen gleich aus und wären „schnell mitgemacht" — treffen aber auf den Editor aus P10 (formatiert lesen vs. roh bearbeiten) | Im Auftrag ausdrücklich abgegrenzt und als **benannter** Folgepunkt geführt; er wird mit dem G4-Antrag zur Entscheidung gestellt, statt geräuschlos zu verschwinden (B029) |

## Benannter Folgepunkt (nicht Teil von P12)

**Ticket-Bodys, DR-Bodys und die beiden Dokumentenansichten** (`dateiKarten` und die
Requirements-Ansicht) laufen weiter über den Rohtext-Weg. Nach der Umstellung von Briefen und
Reports ist der Renderer für sie technisch bereit; die offene Frage ist nicht technisch, sondern
eine Entwurfsfrage am P10-Editor: **Was passiert, wenn man einen formatiert gelesenen Ticket-Body
bearbeiten will?** Dieser Punkt wird mit dem G4-Antrag von P12 zur Entscheidung vorgelegt.

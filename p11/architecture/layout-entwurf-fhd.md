# Layout-Entwurf: 16 Kacheln ohne Scrollen auf FHD

*2026-08-17, PL (Sprint 5, `p11/T-0005`). Entwurfsfrage aus dem Projektauftrag,
Risiko R2. Gerechnet gegen `platform/backend/static/index.html` (Stylesheet, Z. 11–92)
und gegen den **echten** Payload aus `aggregation.cockpit_alle` — nicht gegen eine
Testwelt (L-2026-08-16h Regel 1).*

## Das Wichtigste

1. **Es passt — aber nur außerhalb des heutigen Textkorridors.** `main` ist auf
   `max-width: 62rem` (992 px) begrenzt. In dieser Spalte passen die 16 Kacheln bei
   **keiner** Anordnung auf eine FHD-Seite. Über die volle Breite passen sie mit
   Reserve: **7 Spalten × 3 Reihen**.
2. **⚠ Der Befund ist ein anderer als die Frage.** Nicht die Zahl der Kacheln sprengt
   das Budget, sondern **ein Feld**: `letzte_baseline` ist im Vertrag ein `string`
   **ohne Längengrenze** und trägt im echten Payload bis zu **300 Zeichen** (p1). In
   einer 240-px-Spalte sind das ~430 px — mehr als das gesamte Reihenbudget.
3. **Der Grund dafür ist alt und hat einen Namen.** Das Feld trägt **zwei Tatsachen
   unter einem Namen** (Tag *und* Annotation, so wie `git tag -n1` sie ausgibt). Das
   ist B033, und es gehört in den Vertrag, nicht ins Widget →
   `team-dashboard/T-0002`.
4. **Damit ist die Layoutfrage nicht aus der Kachelzahl allein entscheidbar** — und
   genau das verlangt SWR-092 („layout budget computed from the widget set").
5. **Vorgelegt wird das Verlassen des Korridors** (`p11/T-0006`, Frist 19.08.,
   Default LAY-a). Es verändert den Blick auf die Fläche, und der Projektauftrag sagt
   für diesen Fall: vorlegen.

---

## 1. Das vertikale Budget

Alle Maße aus dem Stylesheet, 1 rem = 16 px (kein `font-size` auf `html`).

| Posten | Rechnung | px |
|---|---|---|
| Viewport | SWR-092: 1920×1080 | **1080** |
| `header` | `padding:.7rem` (22,4) + höchstes Kind: `input#pin` `padding:.45rem` (14,4) + Rand 2 + Zeile ~19 | −58 |
| `nav` | `padding:0 .6rem .6rem` (9,6) + Knopf `padding:.45rem` (14,4) + Zeile ~17 | −42 |
| `main` | `padding:1rem` oben und unten | −32 |
| **Für Kacheln** | | **≈ 948** |

**Ehrlich zur Grenze (B027):** 1080 px ist die *Bildschirm*höhe. Ein maximiertes
Chrome-Fenster hat davon rund 130 px weniger für den Viewport. SWR-092 sagt
„1920×1080 viewport", also wird die Anforderung wörtlich genommen — aber wer die Seite
am echten Rechner prüft, hat weniger Platz. Das ist eine Reserve, die der Entwurf
**nicht** hat und die beim Bau gemessen gehört.

## 2. Die Höhe einer Kachel heute

`cockpitKarte` (app.js, Z. 208–320), Maße aus dem Stylesheet:

| Element | Rechnung | px |
|---|---|---|
| `.karte` | `padding:.8rem` ×2 = 25,6 + Rand 2 + `margin-bottom:.8rem` = 12,8 | 40 |
| `h3` | 1rem, `margin:.1rem 0 .4rem` | 27 |
| `.zeile` × 7 | .9rem ≈ 17,3 + `margin:.15rem` ×2 = 4,8 → 22 je Zeile | 154 |
| Knopf „Zum Board" | `padding:.5rem 1rem` | 35 |
| **Summe** | | **≈ 256** |

Sieben Zeilen sind der Normalfall: Status/fertig, Beschreibung, Aufgaben, Statuszahlen,
Team-oder-Digest, Baseline, KPI. Kacheln mit offenen DRs, offenen Briefen, überfälligen
Tickets oder fälligen Takten haben **mehr**.

## 3. Warum es im heutigen Korridor nicht geht

`main { max-width: 62rem }` → 992 px, abzüglich `padding` 32 → **960 px** nutzbar.

| Anordnung | Spalten | Reihen (16 Kacheln) | Höhe | passt in 948? |
|---|---|---|---|---|
| heute: gestapelt | 1 | 16 | 4096 px | **nein** (Faktor 4,3) |
| Raster im Korridor, `minmax(15rem,1fr)` | ⌊(960+12,8)/252,8⌋ = **3** | 6 | 1536 px | **nein** |
| Raster im Korridor, `minmax(11rem,1fr)` | 5 | 4 | 1024 px | **nein**, knapp |
| **volle Breite**, `minmax(15rem,1fr)` | ⌊(1888+12,8)/252,8⌋ = **7** | 3 | 768 px | **ja**, 180 px Reserve |

**Die 62rem sind kein Zufall und kein Versehen.** Sie sind eine Lesbarkeitsentscheidung
für Fließtext — Briefe, Markdown-Ansichten, Ticketdetails. Für eine Übersichtsfläche
ist derselbe Wert eine Fessel: er verschenkt bei FHD **928 px**, also fast die halbe
Breite. Der Entwurf schlägt deshalb **nicht** vor, den Korridor abzuschaffen, sondern
ihn **je Ansicht** zu setzen: Dashboard breit, alles andere unverändert.

## 4. ⚠ Der Befund: ein Feld macht die Kachel unbegrenzt

Gemessen am echten Payload, `len(letzte_baseline)`:

| Eintrag | Zeichen | Eintrag | Zeichen |
|---|---|---|---|
| p1 | **300** | p0 | 90 |
| p3 | 251 | p8 | 53 |
| p2 | 231 | p9 | 50 |
| p4 | 208 | p11, p12 | 0 (`""`) |
| p10 | 169 | pm, team-dashboard, team-mail | `null` |
| p5 / p7 | 129 / 128 | platform | 93 |

Beispiel p1, ungekürzt:

> `p1-v1.0  Abschluss-Baseline P1: Mission Control 2.0 (G4/D009). Sprints 0-3, STK-013 +
> SWR-025-033, 3 Inbox-Entscheidungen, Mail-Kanal live. QM-Mitzeichnung: Kriterien K1-K5
> mit Evidenz im Abschlussbericht; …`

In einer 240-px-Spalte bei .9rem sind 300 Zeichen rund **25 Zeilen ≈ 430 px** — allein
für ein Feld, gegen ein Reihenbudget von 303 px. Die Kachel von p1 wäre höher als die
Reihe, in der sie steht.

**Der Vertrag begrenzt das Falsche.** `aufgaben` trägt `max_eintraege: 3` mit
ausgeschriebener Begründung (die volle Liste gehört auf die Detailseite, SWR-094).
`letzte_baseline` trägt `typ: string` und **keine Grenze** — das Feld, das tatsächlich
unbegrenzt wächst.

**Und die Ursache ist nicht die Länge, sondern die Vermischung.** Der Wert kommt aus
`git tag -n1` und enthält **zwei Tatsachen unter einem Namen**: den Tag (`p1-v1.0`) und
seine Annotation (den Abnahmetext). Ein Widget braucht den Tag; die Detailseite braucht
beides. Solange beides ein String ist, muss jeder Leser dieselbe Trennung selbst
erfinden — und die erste, die es tut, ist die zweite Liste, gegen die ADR-P11-001
Punkt 3 geschrieben ist.

**Deshalb wird es nicht im Widget gelöst.** Ein `.slice(0, 20)` im Dashboard wäre eine
eigene Regel neben dem Vertrag, genau das, was `T-0003` ausdrücklich verbietet
(„die Behelfsregel **aus der YAML** — nicht eine eigene im Widget"). Die Frage geht an
den Vertragsinhaber: **`team-dashboard/T-0002`**, Frist 19.08.

## 5. Der Entwurf

1. **Raster statt Stapel**, `grid-template-columns: repeat(auto-fit, minmax(15rem, 1fr))`
   — dieselbe Technik wie `.spalten`, die seit P3 im Stylesheet steht. Keine neue
   Bauart, kein neues Konzept.
2. **Volle Breite nur für das Dashboard.** `main` bekommt eine Modifikatorklasse; die
   62rem bleiben für jede andere Ansicht.
3. **Die Kachel ist die kurze Fassung**, die Detailseite die lange (SWR-094). Auf die
   Kachel gehören: Name, Status, offene Aufgaben (Zahl), überfällig/Takt-fällig, wenn
   vorhanden, Baseline-**Tag**, KPI. Beschreibung und Aufgabenliste sind Kandidaten für
   die Detailseite — sie tragen zusammen bis zu 200 Zeichen.
4. **Kein Gruppieren, kein „nur Auffälliges", keine Favoriten.** Die drei Optionen aus
   dem Projektauftrag lösen ein Problem, das bei 16 Einträgen nicht besteht — und die
   Organisation hat für den Überlauf bereits eine **freigegebene** Antwort:
   **SWR-093**, Widgets einzeln abschaltbar, die übrigen füllen den Platz nach. Eine
   zweite Überlaufmechanik daneben wäre B033.
5. **Mobil unverändert.** Die `@media (max-width:640px)`-Regel setzt `.spalten` schon
   heute auf eine Spalte; das Dashboard erbt das Muster.

## 6. Wie lange der Entwurf trägt

Bei 7 Spalten und 268 px Zeilenhöhe (256 + Abstand) passen ⌊948/268⌋ = **3 Reihen**,
also **21 Einträge**. Heute sind es 16.

* Bis **21**: passt mit allen Widgets an — SWR-092 ist erfüllt, wie es dasteht.
* Ab **22**: SWR-092 ist verletzt, solange alle Widgets an sind. Dann greift SWR-093
  (abschalten) — aber das ist eine Handlung des Betrachters und **keine** Erfüllung von
  SWR-092, der ausdrücklich „for the full set … currently present" sagt.

**Das ist eine widerlegbare Zahl und kein Versprechen.** Sie hängt an der Kachelhöhe von
256 px, und die hängt an Punkt 4 dieses Dokuments. Wird `letzte_baseline` nicht
begrenzt, ist die 21 falsch — dann liegt die Grenze schon heute unter 16.

## 7. Was dieser Entwurf ausdrücklich nicht ist

Er ist **gerechnet, nicht gemessen**. Kein Browser hat diese Seite gesehen; Mission
Control läuft am Host, nicht in dieser Sandbox — dieselbe Grenze wie bei SWR-105/107/108.
Die Zahlen stammen aus dem Stylesheet und aus dem echten Payload, die Zeilenhöhen sind
aus `font-size` und `margin` abgeleitet und können je Schriftart um einige Prozent
abweichen. Die Prüfung im Browser ist Teil der Verifikation von SWR-092 beim Bau und
steht dort, nicht hier.

# ADR-P12-001 — Delta zu ADR-002: ein Renderweg, und die Ticket-Erkennung im Inline-Pass

**Status:** angenommen · **Datum:** 2026-08-17 (Sprint 18) · **Rolle:** PL ·
**Ticket:** `projects/p12/T-0009` (Teil b von `p12/T-0005`) · **Verifiziert durch:**
`platform/tests/test_renderweg_zaehlung.py`

## Kontext — was ADR-002 offenlässt

ADR-002 („Frontend ohne Build, PWA") verbietet Bibliotheken und Bundler. Daraus ist ein
**eigener** Markdown-Renderer entstanden (`mdRender`/`mdInline` in
`platform/backend/static/app.js`, SWR-059/060). ADR-002 sagt, **womit** gerendert wird — es
sagt nicht, **wie viele** Wege es geben darf. Genau darin liegt das Risiko von P12:

> **P12 ist eine Zusammenführung. Endet sie mit zwei Wegen statt einem, hat sie ihr Ziel
> verfehlt — auch wenn alles funktioniert.**

`p12/T-0008` hat den heutigen Renderer **gemessen** (57 Briefe, zeichenweise Bilanz): er
verliert kein Nutzzeichen. Dieses Delta baut darauf auf und beantwortet die drei
Entwurfsfragen, die T-0008 ausdrücklich offen gelassen hat.

## Befund 1 — die Ticket-Erkennung sitzt heute NEBEN dem Renderer, nicht darin

Gemessen am Bestand (2026-08-17):

| Weg | Funktion | Was er kann | Was er nicht kann |
|---|---|---|---|
| A | `mdRender` → `mdInline` | Überschriften, Absätze, Listen, Tabellen, `**fett**`, `*kursiv*`, `` `code` ``, `[text](http…)` | **kein** `T-nnnn` |
| B | `tlinks(text, projekt)` | `T-nnnn` → Deep-Link | **kein** Markdown — nur reiner Text |

Das ist wörtlich der Zustand, den **SWR-098** verbietet: *„The link behaviour shall not be
provided by a second wrapper around the raw text."* `tlinks` **ist** dieser Wrapper.

> **Zwei Wege, von denen jeder genau kann, was der andere nicht kann, sehen aus wie
> Arbeitsteilung und sind eine Lücke: ein `T-0042` in einem Listenpunkt ist heute kein Link,
> und ein `**fett**` in einer Entscheidungszeile ist heute kein Fettdruck.**

⚠⚠ **Und der Bestand gibt den Beweis selbst.** In `app.js`, Zeile 1106:

```js
tlinks(String(text).replace(/\*\*/g, ""), projekt)
```

Der Aufrufer **entfernt die Sternchen**, bevor er den Text an Weg B gibt — weil Weg B
Fettdruck nicht kann und die Sternchen sonst als Zeichen dastünden.

> **Ein Aufrufer, der Markup wegwirft, damit der Renderer nicht daran scheitert, ist keine
> Notlösung an einer Randstelle. Er ist der Beleg, dass der falsche Weg genommen wurde — und
> er hat die Lücke reparieren müssen, statt sie zu melden.**

`tlinks` wird an **vier** Stellen aufgerufen (Zeilen 126, 829, 943, 1106), eine davon über
den Zwischenschritt `preMitLinks`.

## Entscheidung 1 — die Erkennung wandert in den Inline-Pass, an EINE Stelle

Die Ticket-Erkennung wird ein **Zweig des Inline-Musters** in `mdInline` — dort, wo heute
`[text](url)`, `**fett**`, `*kursiv*` und `` `code` `` entschieden werden. Damit wirkt sie
in Fließtext, Listenpunkten **und** Tabellenzellen, ohne dass eine Stelle davon weiß.

⚠ **Reihenfolge im Muster ist Teil der Entscheidung:** `` `code` `` gewinnt gegen `T-nnnn`.
Ein `T-0042` in Backticks ist ein **Zitat** und darf kein Link werden — sonst verlinkt die
Dokumentation über den Renderer ihre eigenen Beispiele.

⚠ **Das Ziel kommt weiter aus `Regeln.ticketRoute`** (SWR-150, Sprint 18). Der Inline-Pass
baut **keine** Route: er erkennt eine Nummer und fragt die eine Stelle, ob sie ein Ziel hat.
Und weil der Text nicht sagt, aus welchem Projekt eine Nummer kommt, bleibt es
`Regeln.textRefAnnahme` — eine **benannte Annahme**, im `title` sichtbar. Ein Renderer, der
eine Vermutung wie eine Auskunft darstellt, ist der Zustand, gegen den SWR-114 gebaut ist.

## Entscheidung 2 — die Regel gegen den zweiten Renderpfad ist eine PRÜFUNG, kein Vorsatz

DoD 2 von `T-0009` verlangt das ausdrücklich, und die Bauform ist im Haus belegt: SWR-134
hält die Git-Schreibwege mit einem Zähltest über den Syntaxbaum bei **einem**; SWR-146 hat
einen Altbestand von drei Inline-Regeln mit einem **eingefrorenen Zähler** auf null gezogen.

Festgelegt ist deshalb `platform/tests/test_renderweg_zaehlung.py`:

1. **Genau eine** Funktion in `app.js` erzeugt aus Markdown-Text DOM (`mdRender`). Ein
   zweiter Renderer macht die Prüfung rot — auch einer, der „nur für Briefe" gedacht ist.
2. **`mdInline` ist der einzige Inline-Pass**, und `mdRender` ruft ihn an jeder Stelle auf,
   an der Text zu Kindknoten wird.
3. ⚠ **Der Altbestand ist ein eingefrorener Zähler und keine Warnung.** Aufrufe von
   `tlinks` außerhalb des Renderers: heute **4**, festgeschrieben. Kommt einer dazu, ist die
   Prüfung rot; nimmt `p12/T-0006` sie weg, muss die Zahl **gesenkt** werden — und dieses
   Senken ist der Nachweis der Zusammenführung.
   > **Ein Altbestand, der als Warnung dasteht, wächst. Einer, der als Zahl dasteht, kann
   > nur sinken.**
4. **Kein `innerHTML`** auf dem Renderweg (SWR-100) — Briefe sind freier Text eines
   Menschen.

⚠ **Was diese Prüfung NICHT kann:** sie sieht Funktionen und Aufrufe, nicht Absichten. Wer
den zweiten Weg **innerhalb** von `mdRender` unterbringt, kommt durch. Das ist die bewusste
Grenze — eine Prüfung, die auch das könnte, müsste den Renderer verstehen, und dann wäre sie
der dritte Weg.

## Entscheidung 3 — Code-Zäune: Vollständigkeit ist erfüllt, Darstellung nicht

`p12/T-0008` hat gemessen: der Inhalt von ```` ``` ````-Zäunen **geht nicht verloren**, er
landet als **Absatztext**. SWR-099 verlangt **beides** — vollständig **und** verbatim in
Monospace. Der zweite Teil ist offen, und er wird hier **beantwortet statt übernommen**:

Der **Block-Pass** von `mdRender` bekommt einen Zweig **vor** Überschrift, Tabelle und
Liste: eine Zeile, die mit ```` ``` ```` beginnt, öffnet einen Block; alle Zeilen bis zum
schließenden Zaun gehen **unverändert** in ein `<pre><code>` und **nicht** durch den
Inline-Pass.

⚠ **Drei Festlegungen, die man leicht übersieht:**

1. **Der Inline-Pass wird übersprungen, nicht nur „vorsichtig" angewandt.** In einem
   Codeblock ist `**` ein Sternchenpaar und `T-0042` eine Zeichenfolge. Ein Link im
   Codebeispiel ist ein Fehler, der wie eine Verbesserung aussieht.
2. **Zeilenumbrüche bleiben.** Der Absatzpfad fügt Zeilen mit `" "` zusammen (`absatz.join(" ")`).
   Genau dieses Zusammenfügen ist der heutige Verlust — nicht an Zeichen, sondern an
   **Struktur**. Der Block-Pass fügt mit `"\n"`.
3. ⚠ **Ein nicht geschlossener Zaun beendet den Block am Textende** und verschluckt nichts.
   Andernfalls entstünde aus einem vergessenen Zaun ein unsichtbarer Rest — ein
   Vollständigkeitsverlust, den die Bilanz aus T-0008 **finden** würde, und dann wäre die
   Reparatur von SWR-099 die Ursache eines neuen Befundes.

**Umsetzung: `p12/T-0006`.** Dieses Delta baut nicht um — es legt fest, was der Umbau tun
muss und woran er gemessen wird.

## Folgen

- `p12/T-0006` hat ab hier eine Vorlage und keine Entwurfsfrage mehr.
- `SWR-097`–`SWR-100` haben ab hier **Prüfungen** statt nur den Status `draft`.
- ⚠ Der eingefrorene Zähler (3) ist eine **Zusage an einen künftigen Lauf**: er darf sinken
  und nicht steigen. Wer ihn erhöht, muss es begründen — und sieht dabei, dass er es tut.

## Verworfen

- **Ein zweiter Renderer „nur für Briefe".** Er wäre die bequemste Lösung und genau das
  Scheitern, das P12 verhindern soll.
- **`tlinks` behalten und `mdInline` zusätzlich um `T-nnnn` erweitern.** Dann gäbe es die
  Erkennung **zweimal**, und beide Fassungen müssten für immer gleich bleiben — B033.
- **Die Regel als Satz in ADR-002 statt als Prüfung.** Ein Satz, den keine Prüfung liest,
  altert lautlos (`L-2026-08-17ag`, an einem Tag dreimal aufgelaufen).


---

## Nachtrag Sprint 19 (`p12/T-0006`) — der Zähler ist eingelöst, und was er nicht wusste

**`ALTBESTAND_TLINKS_AUFRUFE`: 4 → 0.** `tlinks` ist nicht „nicht mehr aufgerufen", sondern
**entfallen**. Die drei Zusicherungen, die dieses Delta als *Befund* eingefroren hatte, sind
rot geworden — **genau die drei und keine vierte** — und **umgedreht** statt gelöscht.

> **Ein Test, der den heutigen Zustand festhält, ist erst dann etwas wert, wenn der Lauf,
> der ihn rot macht, ihn auch umdreht. Sonst ist er eine Warnung mit Zeitstempel.**

### ⚠⚠ Befund 1 — dieses Delta hat eine Stelle übersehen, und der Zähltest konnte es nicht sehen

Entscheidung 3 beschreibt den Zaun-Zweig **im Block-Pass** und ist darin vollständig. Sie
sagt nichts über die **Fortsetzungsregel** des vorherigen Blocks — und die entscheidet, ob
der neue Zweig überhaupt erreicht wird. Absatz- und Listenpfad sammeln Folgezeilen, bis eine
Zeile einem bekannten Muster entspricht; ```` ``` ```` stand in diesem Muster nicht. Ein
Zaun **nach** einem Absatz wurde samt Inhalt in den Absatz gezogen, und der Zaun-Zweig war
toter Code.

> **Ein neuer Block-Zweig ist erst dann erreichbar, wenn die Fortsetzungsregel des
> vorherigen Blocks ihn kennt. Beide Stellen sind richtig, wenn man sie einzeln liest.**

⚠ **Gefunden hat es der Verhaltenstest, nicht der Zähltest** — und das ist die Lehre, nicht
der Fehler: `test_renderweg_zaehlung.py` liest den Quelltext und sah den Zweig stehen. Er
konnte nicht sehen, dass ihn niemand erreicht.

> **Ein Muster im Quelltext ist eine Absicht. Erst der Knoten, der danach dasteht, ist ein
> Befund.**

⚠ Der erste JS-Verhaltenstest, der es fand, hatte den Zaun am **Textanfang** — dort war er
grün. Rot wurde er erst mit einer Zeile davor. *Ein Beispiel, das die Stelle nicht trifft,
ist grün und sagt nichts.*

### ⚠ Befund 2 — die Prüfstrecke hätte fast einen zweiten Ladeweg bekommen

Der Inline-Pass fragt seit dieser Umstellung `Regeln` nach dem Ticketziel (SWR-150). Der
Nachweis-Harnisch lud aber nur `app.js` und nicht `regeln.js` — der Bestandstest wurde rot
mit `Regeln is not defined`. Der bequeme Weg wäre ein Ersatz-`Regeln` im Test gewesen: eine
**zweite Antwort** auf genau die Frage, gegen die SWR-150 gebaut ist. Geladen wird deshalb
die echte `regeln.js`, in derselben Reihenfolge wie `index.html` (Zeile 167/168).

Und weil damit ein zweiter Testlauf denselben Harnisch braucht, liegt er jetzt **einmal** in
`platform/tests/js/_app_laden.cjs`. *Zwei Harnische, die `app.js` in zwei leicht verschiedene
Mini-DOMs laden, sind zwei Aussagen darüber, was der Renderer vorfindet — und sie wären an
dem Tag verschieden, an dem es darauf ankommt.*

### ⚠⚠ Entscheidung 4 (neu) — die vier Rohtext-Ansichten: warum 0 und nicht 1

Ticket-Body, DR-Body und die zwei Dokumentenansichten sind laut Projektauftrag **nicht im
Umfang**. Sie holten ihre Ticketlinks aber aus `tlinks`. Damit standen zwei Wege offen, und
**keiner davon ließ diese vier Ansichten unberührt**:

- Zähler bei **1** stehen lassen → `tlinks` lebt weiter → **SWR-098 bleibt unerfüllt**, und
  die Zusammenführung endet mit zwei Wegen.
- Zähler auf **0** → die vier holen ihre **Inline**-Regeln aus dem einen Inline-Pass;
  ihre **Blockstruktur** bleibt Rohtext im `<pre>`.

Gewählt ist das Zweite. `preMitLinks` bleibt und ist **kein zweiter Renderweg**: es erzeugt
aus Markdown-Blöcken kein DOM. Der Unterschied ist die ganze Begründung, und er ist als Zahl
festgehalten (`ROHTEXT_ANSICHTEN = 4`).

⚠ **Zwei Einzelheiten, die man beim Nachbauen falsch macht:**

1. **Der Inline-Pass läuft dort ZEILENWEISE.** Auf ein ganzes Dokument losgelassen, spannt
   ein einzelnes `*` in Zeile 3 einen `<em>` bis zum nächsten `*` in Zeile 90 — *ein Muster,
   das über Zeilengrenzen greift, findet in einem langen Text immer ein Paar.* In `mdRender`
   bekommt der Inline-Pass nie mehr als einen Absatz; diese Eigenschaft musste hier von Hand
   hergestellt werden.
2. **`blocked_by` ist eine LISTE und kein Fließtext.** Sie durch eine Textsuche zu schicken
   war der bequeme Weg. *Die Liste weiß, wo ihre Kennungen anfangen und aufhören; eine
   Textsuche muss es raten.* Sie geht jetzt über `ticketLink` je Element.

⚠ **Der Rest von Entscheidung 4 ist NICHT entschieden**, sondern vorgelegt: ob die vier
Ansichten über den **Block**-Renderer laufen sollen, steht in `p12/T-0010` (G4-DR, Klasse A)
zur Entscheidung. *Der Unterschied zwischen „inline mitrendern" und „über den Block-Renderer
führen" ist nicht kosmetisch: das Zweite macht aus `## Ziel` eine Überschrift.*

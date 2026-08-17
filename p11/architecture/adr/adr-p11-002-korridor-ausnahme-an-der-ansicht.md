# ADR-P11-002: Die Korridor-Ausnahme sitzt an der Dashboard-Ansicht, nicht am Korridor

*2026-08-17, ARCH (Sprint 9, `p11/T-0007`). Status: **angenommen** — die Fläche ist vom
Auftraggeber entschieden (`D002`, LAY-a, 2026-08-17 08:11 via Inbox, DR `p11/T-0006`).
Kontext: P11 „Widget-Dashboard" (STK-021, SWR-092–096). Ergänzt ADR-P11-001.*

## Kontext

Mission Control ist heute **überall** an einen Textkorridor von 62rem gebunden. Das ist
kein Zufall, sondern ein Abnahmekriterium des Projektauftrags: lange Textzeilen sind
schlecht lesbar, und der Korridor erzwingt eine Zeilenlänge, die man lesen kann.

Das Dashboard hat einen anderen Bedarf. 16 Kacheln passen bei 1920×1080 **nur dann** auf
eine Seite, wenn sie breiter sein dürfen als der Korridor. Der Konflikt stand seit
Sprint 5 als Decision Request beim Auftraggeber (`p11/T-0006`) mit drei Optionen:

| Option | Inhalt | Ausgang |
|---|---|---|
| **LAY-a** | breit **nur** im Dashboard, alles andere unverändert | **gewählt** (Default) |
| LAY-b | überall breit | verworfen |
| LAY-c | Korridor behalten und scrollen | verworfen — hätte das erste Abnahmekriterium des Projektauftrags aufgegeben und wäre als eigener DR zurückgekommen |

**Der Auftraggeber hat LAY-a gewählt** — zwei Tage vor der Frist. Damit ist die **Fläche**
entschieden.

## ⚠ Was damit NICHT entschieden war

Die Entscheidung sagt, **dass** das Dashboard breiter sein darf. Sie sagt nicht, **wo diese
Ausnahme im Code lebt** — und das ist keine Formalie, sondern die Frage, wer in einem Jahr
noch weiß, dass es eine Ausnahme gibt.

Zwei Bauformen liefern in der ersten Woche dasselbe Bild:

* **(A) Ausnahme am Korridor.** Die globale Regel bekommt einen Sonderfall („der Korridor
  gilt, außer im Dashboard"). Die Regel steht dann an einer Stelle und kennt eine Liste von
  Ausnahmen.
* **(B) Ausnahme an der Ansicht.** Die globale Regel bleibt unangetastet; die
  Dashboard-Ansicht setzt ihre eigene Breite und weicht damit sichtbar ab.

## Entscheidung

**Die Ausnahme sitzt an der Dashboard-Ansicht (B).** Der 62rem-Korridor bleibt als globale
Regel unverändert und kennt keinen Sonderfall.

### Begründung

**1. Eine Regel mit Ausnahmeliste wird zur Frage.** Sobald der Korridor einen Sonderfall
kennt, muss jede künftige Ansicht beantworten: *„gilt er hier?"* Die Antwort steht dann
nicht mehr in der Ansicht, sondern in einer Liste woanders — und Listen, die niemand liest,
wachsen. Bleibt die Regel ausnahmslos, ist die Frage für jede neue Ansicht **schon
beantwortet**, und nur wer bewusst abweicht, muss das in seiner eigenen Datei hinschreiben.

**2. Das ist dieselbe Regel wie in ADR-P11-001, zum zweiten Mal angewandt.** Dort lautete
sie: die Widget-Logik liegt **am Rand**, nicht in einer zweiten Aufbereitung in der Mitte.
Hier: die Abweichung liegt **an der abweichenden Ansicht**, nicht als Sonderfall in der
gemeinsamen Regel. Beide Male ist die Frage, ob eine Besonderheit die Mitte anfassen darf —
und beide Male ist die Antwort nein. Dass dieselbe Regel zweimal trägt, ist der eigentliche
Grund, sie als ADR zu führen und nicht als Kommentar.

**3. Der Auftraggeber hat die engste Option gewählt.** LAY-a ist von den drei Optionen die
mit der **kleinsten Reichweite** — nur das Dashboard, alles andere unverändert. Bauform (A)
würde diese Wahl technisch aufweichen: eine Ausnahmefähigkeit im Korridor ist eine Tür, und
LAY-b wäre danach nur noch ein weiterer Eintrag in einer Liste. Bauform (B) hält die
Entscheidung dort eng, wo sie eng gemeint war.

## Woran man erkennt, dass die Regel gebrochen wurde

*(Ohne diesen Abschnitt wäre dieser ADR eine Absichtserklärung und keine Entscheidung —
B038: eine Regel, deren Bruch niemand bemerkt, ist keine.)*

1. **Die Korridor-Definition nennt eine Ansicht beim Namen.** Taucht in der globalen
   Breitenregel das Wort „dashboard" (oder ein anderer Ansichtsname) auf, ist die Ausnahme
   in die Mitte gewandert. Das ist maschinell prüfbar und gehört als Test zu `T-0008`.
2. **Eine zweite Ansicht weicht ab, ohne es selbst zu sagen.** Findet sich außerhalb der
   Dashboard-Ansicht eine Breitenangabe, die den Korridor überschreibt, ist entweder LAY-b
   durch die Hintertür eingetreten oder jemand hat eine Entscheidung getroffen, die dem
   Auftraggeber nie vorgelegt wurde.
3. **Der Korridorwert steht an mehr als einer Stelle.** Dann gibt es zwei Wahrheiten über
   dieselbe Zahl (B033), und die Ausnahme ist nicht mehr entscheidbar.

## Folgen

* `T-0008` baut die Dashboard-Ansicht mit **eigener** Breitenangabe und rührt die globale
  Regel nicht an; die Prüfungen aus dem Abschnitt oben werden dort Tests.
* **Offen und ausdrücklich nicht hier entschieden:** ob 16 Kacheln bei 1920×1080
  tatsächlich lesbar auf eine Seite passen. Das ist eine **Messung am Gebauten** und keine
  Architekturfrage; sie steht als DoD-Punkt 3 in `T-0008`. Sie hier vorwegzunehmen wäre die
  Bündelung, vor der B025 warnt — und eine Zahl ohne Grundlage (L-2026-08-17q).
* Ergibt die Messung, dass es **nicht** reicht, ist das eine neue Frage an den
  Auftraggeber und **kein** Anlass, LAY-a stillschweigend zu LAY-b auszuweiten.

## Quelle der Fläche

`D002` im `p11/management/decisions/decision-log.md`: *Mensch (E. John, via Inbox),
2026-08-17 08:11, **LAY-a***, auf den DR `p11/T-0006`. Die Fläche kommt damit vom
Auftraggeber und nicht aus dem Entwurf — festgehalten, damit ein späterer Leser die
Entscheidung nicht für eine Entwurfsannahme hält und sie „verbessert".

# Projektauftrag P12 — „Markdown-Renderer auch für Briefe/Reports" (v1.0, G0 erteilt)

*2026-08-16, PL. **G0: `p12/T-0001`, Option G0a (`p12/D000`, 2026-08-16 18:04)**. Herkunft:
Projekt-Pool Technik-Kandidat **#7**, Quelle **P7-LeLe** (`p7/management/p7-abschlussbericht.md`).
Der Ordner und der G0-Antrag entstanden über den „Starten"-Knopf (`pm/T-0022` Teil 2) — der Knopf
hat nichts entschieden, die Freigabe kam vom Auftraggeber über die Inbox (Playbook Kap. 16).*

## Kurzfassung (B050)

Seit P7 gibt es einen eigenen Markdown-Renderer im HMI — benutzt wird er an **zwei** Stellen
(Digest, Charter). Briefe, Sprint-Reports und alles Übrige stehen weiter als Rohtext im `<pre>`.
P12 stellt **Briefe und Reports** auf den vorhandenen Renderer um. Der eigentliche Inhalt der
Arbeit ist nicht „schöner", sondern **zwei Textwege zu einem machen**: der Renderer kann heute
keine Ticket-Links, der Rohtext-Weg kann keine Formatierung — wer einfach umstellt, verliert die
Ticket-Links genau dort, wo die meisten stehen. Kein neuer Renderer, keine Bibliothek.

## Was und Warum

Der Renderer stammt aus **SWR-059** (P7, aus einem Sprint-1-Befund des Auftraggebers) und wurde
mit **SWR-060** um Links erweitert: `mdRender` in `platform/backend/static/app.js`, DOM-basiert,
ES5, **ohne Bibliothek** (ADR-002). Er beherrscht Überschriften, Absätze, Fett/Kursiv/Code,
Listen, Pipe-Tabellen, Trennlinien und `[text](https://…)`.

Aufgerufen wird er an genau **zwei** Stellen: dem Digest-Verlauf und der Charter im Team-Tab.
Alles andere läuft über `preMitLinks` — Rohtext in einem `<pre>`, mit `T-xxxx` als Link (SWR-040).
Betroffen sind **sieben Aufrufstellen**: Brief und Antwort im Team-Chat, Sprint-Report,
Ticket-Body, DR-Body in der Inbox und zwei Dokumentenansichten. In diesen Ansichten erscheinen
Tabellen als Pipe-Zeichen, Überschriften als `##`, Fettdruck als Sternchen.

**Der Kern ist die Zusammenführung zweier Textwege, nicht die Optik.** Es gibt heute zwei
Darstellungswege, und jedem fehlt genau die Fähigkeit des anderen:

| Weg | kann | kann nicht |
|---|---|---|
| `mdRender` (SWR-059/060) | Formatierung, externe Links | **keine Ticket-Links** |
| `preMitLinks`/`tlinks` (SWR-040) | Ticket-Links | **keine Formatierung** |

Wer Briefe und Reports einfach auf `mdRender` umhängt, **verliert die Ticket-Links dort, wo die
meisten Verweise stehen** — Sprint-Reports und DR-Bodys sind voll von `T-xxxx`. Ein Umbau, der
eine Fähigkeit gegen eine andere tauscht, ist keine Lieferung, sondern ein Tausch. P12 führt die
Ticket-Erkennung deshalb **in den Inline-Pass des vorhandenen Renderers** und lässt keine zwei
Wege nebeneinander stehen (Prüffrage aus B033: „Welche Regel wäre ich versucht, hier noch einmal
zu schreiben — und wo steht sie schon?").

**Neu gegenüber P7 ist die Herkunft des Textes.** Digest und Charter kommen aus dem Repo. Briefe
kommen aus dem Eingabefeld des Team-Chats — **Freitext eines Menschen**. Damit werden zwei Dinge
zu Anforderungen, die bei P7 keine sein mussten: dass nichts als HTML ausgeführt wird, und dass
nichts verschwindet, was der Renderer nicht kennt.

## Abnahmekriterien

1. **Ein Renderer, kein zweiter.** Nach P12 gibt es im Frontend **genau eine** Funktion, die
   Markdown in DOM verwandelt; die Ticket-Erkennung liegt in ihrem Inline-Pass. `preMitLinks` ist
   entfernt oder ein Aufruf genau dieses Renderers. *Stichprobe:* `grep -c "function md"` und
   Aufrufstellen-Liste im Review; Prüfpunkt im ADR von Sprint 1.
2. **Ticket-Links überleben die Umstellung.** Eine Kennung `T-xxxx` ist nach der Umstellung ein
   klickbarer Link auf die Detailansicht — **im Fließtext, in einer Listenzeile und in einer
   Tabellenzelle**. *Stichprobe:* `p7/management/sprint-2/report.md` (Tabelle mit Ticket-IDs) im
   Reports-Tab.
3. **Briefe und Reports erscheinen formatiert.** Team-Chat (Nachricht **und** Antwort) und
   Sprint-Reports zeigen Überschriften, Listen, Fett/Kursiv/Code, Tabellen und Trennlinien so wie
   der Digest heute. *Stichprobe:* ein Brief mit Liste und Fettdruck, ein Report mit Tabelle.
4. **Kein Text geht verloren.** Was der Renderer nicht kennt, wird **angezeigt statt verworfen** —
   namentlich Code-Blöcke mit ``` (heute nicht unterstützt; `platform/N-0002` enthält welche).
   *Nachweis:* automatisierte Prüfung „der gerenderte Textinhalt enthält jedes nicht-leere Zeichen
   der Quelle außer den Auszeichnungszeichen", angewandt auf alle 21 Sprint-Reports und alle 38
   Briefe des Bestands.
5. **Kein `innerHTML`, keine Bibliothek** (ADR-002 unverändert). *Nachweis:* statischer Prüfpunkt
   auf den Renderpfad **und** eine Probe mit einem Brief, der `<script>` sowie
   `<img onerror=…>` im Klartext enthält — beides erscheint als **Text**, nicht als Element.
6. **Requirements-first:** STK-022, SWR-097–101, Matrix **0 Lücken**; Gates als Inbox-DRs;
   Aufwandsschätzung je Planning; **0,00 € API**.

## Rahmen

**S0 (jetzt):** Anforderungen als `draft`, Auftrag, Risiken, G1-DR.
**S1 (nach G1):** ADR-Delta zu ADR-002 (ein Renderpfad, Ticket-Erkennung im Inline-Pass),
**Entscheidung über die Teststrecke** (siehe R5), Umsetzung, Tests, Abnahme (G4, Baseline
`p12-v1.0`).

**Umsetzung als Ordner `projects/p12`** (`pm/D003`, Sammel-Repo) — kein neues GitHub-Repo, kein
PAT-Update. Der Code liegt in `platform` (`backend/static/app.js`), wie bei P7.

## Abgrenzung (nicht im Umfang)

- **Ticket-Bodys, DR-Bodys und die beiden Dokumentenansichten** werden in Sprint 1 **nicht**
  umgestellt. Der Kandidat sagt „Briefe/Reports", und der Ticket-Body ist zugleich der
  Arbeitsgegenstand des Editors aus P10: „formatiert lesen, roh bearbeiten" ist eine eigene
  Entwurfsfrage mit eigenem Risiko. **Sie sind damit nicht erledigt und nicht vergessen** — sie
  stehen als benannter Folgepunkt in `management/sprint-0/plan.md` und werden mit dem G4-Antrag
  ausdrücklich zur Entscheidung gestellt (B029: was geräuschlos verschwindet, sieht aus, als hätte
  es nie jemand gewollt).
- **Kein neuer Markdown-Umfang** über den Bestand plus Code-Blöcke hinaus: keine Fußnoten, keine
  Bilder, kein eingebettetes HTML.
- **Keine Bibliothek** — ADR-002 gilt unverändert; ein npm-Paket wäre zudem ein neuer externer
  Dienst und damit Klasse A.
- **Der Digest-/Charter-Weg wird nicht umgebaut.** Er läuft schon über den Renderer und bekommt
  die Ticket-Links kostenlos mit; die Auflage aus `pm/T-0031` (Digest-Inhalt nie in ein Repo,
  `team-mail` ist `sensibel`) bleibt unberührt — P12 ändert **wie** gerendert wird, nicht **was**
  gespeichert wird.

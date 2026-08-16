# Projektauftrag P11 — „Widget-Dashboard" (v1.0, G0 erteilt)

*2026-08-16, PL. **G0: `pm/T-0033`, Option G0a (`pm/D007`, 2026-08-16 15:55) → `p11/D000`** — kein
Doppel-G0 (Muster P7/P9/P10). Quellen: Pool-Kandidat #13 des Auftraggebers, Brief `pm/N-0027`,
Gründungs-DR `pm/T-0031` (→ `pm/D006`/TG-a, `team-dashboard`).*

## Was und Warum

Mission Control zeigt den Zustand der Organisation heute im **Org-Cockpit** (P9): eine Kachel je
Projekt/Team, untereinander, mit Statuszahlen, Aufgaben, offenen Entscheidungen und — seit
SWR-091 — überfälligen Tickets. Bei inzwischen **14 Projekten und Teams** ist das eine Liste, die
man **scrollt**; einen Blick auf das Ganze gibt es nicht.

P11 baut daneben eine **Übersichtsfläche, die auf eine FHD-Seite passt**: jedes Projekt/Team als
kompaktes **Widget**, einzeln konfigurierbar und abschaltbar, dazu eine **eigene Seite je
Projekt/Team** für den Detailblick und ein **Mail-Widget** mit der Tageszusammenfassung von
`team-mail`.

**Fachlicher Auftraggeber und Abnehmer ist `team-dashboard`** (`pm/D006`/TG-a). Gebaut wird von
ASPICE auf der Plattform-Fläche — „verwalten, koordinieren, updaten" ist die Daueraufgabe des
Teams, „bauen" ist SW-Entwicklung und gehört in ein Projekt (so ausdrücklich in `pm/T-0031`
Punkt 2 offengelegt und mit G0a bestätigt).

**Der Kern ist Darstellung, nicht Datenbeschaffung.** `aggregation.cockpit` liefert bereits
Status, Aufgaben, wiederkehrende und überfällige Aufgaben, offene Entscheidungen mit
Frist-Ampel, offene Briefe, Baseline und KPI. Eine zweite Datenquelle daneben wäre die Falle aus
B033 („Welche Regel wäre ich versucht, hier noch einmal zu schreiben — und wo steht sie schon?")
und würde garantiert auseinanderdriften. P11 fügt deshalb **keine** neue Sammelstelle hinzu.

## Abnahmekriterien

1. **Kompakt und nicht scrollbar:** Die Startseite passt bei **1920×1080** ohne vertikales
   Scrollen — bei allen aktuell vorhandenen Projekten/Teams. Gekürzt wird durch **Entwurf**
   (Gruppierung/Auswahl, siehe Sprint 0), nicht durch abgeschnittene Widgets. Stichprobe im
   Browser bei FHD.
2. **Widgets konfigurierbar und abschaltbar:** Je Widget lässt sich an/aus und der gezeigte
   Inhalt einstellen; die Einstellung überlebt einen Neustart des Servers. Stichprobe:
   abschalten, Seite neu laden, Widget ist weg; wieder an, Widget ist zurück.
3. **Startseite plus Detailseite:** Zu jedem Projekt/Team gibt es eine eigene Seite mit dem
   vollen Inhalt, per Deep-Link erreichbar. Stichprobe: Deep-Link auf ein Projekt öffnet direkt
   dessen Seite.
4. **Mail-Widget:** Die Tageszusammenfassung von `team-mail` erscheint **nur zur Laufzeit
   gerendert und nur hinter dem bestehenden PIN-Lesegate**. Stichprobe: ohne PIN kein Inhalt,
   mit PIN Inhalt; `git log`/`git status` in `projects` zeigt **keinen** Digest-Text.
5. **Eine Datenquelle:** Alle Zahlen der Fläche stammen aus `aggregation.cockpit`. Nachweis:
   kein zweiter Sammelpfad im Code; Prüfpunkt im ADR von Sprint 1.
6. Requirements-first (STK-021, SWR-092–096, Matrix 0 Lücken), Gates als Inbox-DRs,
   Aufwandsschätzung je Planning, 0,00 € API.

## Rahmen

**S0 (jetzt):** Anforderungen als `draft`, Auftrag, Risiken, G1-DR.
**S1 (nach dem Widget-Vertrag):** ADR (Fläche und Widget-Rendering), Umsetzung Backend/Frontend,
Tests, Abnahme (G4, Baseline `p11-v1.0`).

**Umsetzung als Ordner `projects/p11`** (`pm/D003`) — kein neues GitHub-Repo, kein PAT-Update.

## Eingangsbedingung (nicht Teil von P11)

Der **Widget-Vertrag** aus `team-dashboard/T-0001` (Frist 23.08.) legt fest, welche Felder ein
Projekt/Team für ein Widget liefert. Er ist **Eingangsbedingung für Sprint 1**: ohne ihn hätte
P11 nichts zu rendern. Die im Pool-Kandidat notierte Voraussetzung *„Die Projekte haben eine
Widget Kompatibilität"* existiert **nicht** — sie zu schaffen ist die Arbeit von
`team-dashboard`, nicht von P11.

## Abgrenzung (nicht im Umfang)

- **„Vom Handy aus dem Internet"** — Runbook Kap. 10 gilt unverändert: nur Heim-LAN, nie
  Port-Forwarding. Der Weg ohne Guardrail-Bruch (VPN ins Heimnetz) ist ein eigener
  Klasse-A-Entscheid und kommt als eigener DR, wenn der Auftraggeber ihn will. **Innerhalb** des
  Heim-LANs bleibt die Fläche wie die übrigen vom Handy aus erreichbar.
- **Das Org-Cockpit aus P9 wird nicht ersetzt** und nicht umgebaut. P11 ist eine zweite
  Ansicht auf dieselben Daten; wer die lange Liste will, behält sie.
- **Kein Digest-Inhalt in ein Repo.** Auflage aus `pm/T-0031` Punkt 1: `team-mail` ist
  `sensibel`. Der erste committete Digest-Inhalt macht `team-dashboard` `sensibel` und kostet
  den GitHub-Remote. Das Mail-Widget rendert deshalb ausschließlich zur Laufzeit.
- **Keine zweite Datenquelle**, keine neuen Kennzahlen. Was das Cockpit nicht kennt, zeigt P11
  nicht; fehlt etwas, ist das ein CR am Cockpit, nicht ein Umgehungsweg im Dashboard.

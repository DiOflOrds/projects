# Sprint-0-Plan P11 — Anforderungen

*2026-08-16, PL. G0 erteilt (D000/G0a, `pm/D007`). Zweiter Projektordner im Sammel-Repo
`projects` (`pm/D003`), erstes Projekt mit einem **Team** als fachlichem Auftraggeber
(`team-dashboard`, `pm/D006`).*

| Ticket | Inhalt | Rolle | Schätzung (E5) |
|---|---|---|---|
| T-0001 | SWE.1: STK-021 + SWR-092–096 (kompakte Fläche, Widget-Konfiguration, Detailseite, Mail-Widget hinter PIN, Vertragstreue) als Entwurf + Auftrag/Plan | rm/pl | 30 min |
| T-0002 | DR: G1 — SWR-Set freigeben (Inbox) | pl | 5 min |

**Sprint 1 danach** — Eingangsbedingung: der **Widget-Vertrag** aus `team-dashboard/T-0001`
(Frist 23.08.). Inhalt: ADR (Fläche und Widget-Rendering ohne zweite Aufbereitung neben dem
Cockpit), Layout-Entwurf zur Frage „was passt auf eine FHD-Seite", Backend-Endpunkt(e) auf
`aggregation.cockpit`, Widget-Konfiguration mit Persistenz, Detailseiten, Mail-Widget hinter dem
PIN-Lesegate, Tests, Abnahme (G4, Baseline `p11-v1.0`).

## Risiken

| # | Risiko | Gegenmaßnahme |
|---|---|---|
| R1 | **Zweite Datenquelle neben `aggregation.cockpit`** — die Falle aus B033; zwei Aufbereitungen derselben Zahlen driften auseinander, und zwar still | SWR-092 verlangt ausdrücklich die vorhandene Quelle; „kein zweiter Sammelpfad" ist Prüfpunkt im ADR und Abnahmekriterium 5, nicht Kür |
| R2 | **„Nicht scrollbar" wird beim Bau stillschweigend aufgeweicht** (Widgets werden kleiner, bis es doch scrollt oder abschneidet) | Der Entwurf (Gruppieren/Auswahl/Favoriten) fällt in Sprint 0 **vor** dem Bau; Abnahmekriterium 1 ist eine Browser-Stichprobe bei FHD mit dem realen Bestand, nicht mit einer Auswahl |
| R3 | **Digest-Inhalt landet doch in einem Repo** (Cache, Logzeile, Snapshot) und macht `team-dashboard` `sensibel` — Verlust des GitHub-Remotes | SWR-095 verbietet Repo, Cache-Datei **und** Log ausdrücklich; Stichprobe ist `git status` nach der Nutzung, nicht nur die Ansicht |
| R4 | **Der Widget-Vertrag verzögert sich** (`team-dashboard/T-0001`, Frist 23.08.) und Sprint 1 startet ohne Feldliste | Der Vertrag ist als Eingangsbedingung benannt, nicht als Annahme; SWR-096 beschreibt bewusst nur das Verhalten bei **fehlenden** Feldern, damit Sprint 1 nicht auf eine geratene Liste baut |
| R5 | **Doppelte Fläche:** Dashboard und Org-Cockpit erklären dasselbe unterschiedlich | Abgrenzung im Auftrag: P9 wird nicht ersetzt und nicht umgebaut; P11 ist eine zweite Ansicht auf dieselben Daten |

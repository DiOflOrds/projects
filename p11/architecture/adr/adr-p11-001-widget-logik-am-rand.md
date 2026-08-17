# ADR-P11-001: Die Widget-Logik liegt am Rand, nicht in einer zweiten Aufbereitung

*2026-08-17, ARCH (Sprint 4, `p11/T-0004`). Status: vorgeschlagen — verbindlich mit dem
G4-DR aus `p11/T-0003`. Kontext: P11 „Widget-Dashboard" (STK-021, SWR-092–096) nach
G1a/D001. Innerhalb von ADR-001/002/006 der Plattform.*

## Kontext

Das Dashboard soll jedes Projekt und Team als kompaktes Widget auf **einer** Seite zeigen.
Die Daten dafür gibt es schon: `aggregation.cockpit_alle` liefert für alle 16 Einträge
dieselben 17 Felder, und der Widget-Vertrag (`team-dashboard/vertrag/widget-vertrag-v2.yaml`)
erklärt sie für normativ. SWR-092 verlangt ausdrücklich, dass **kein zweiter Erhebungsweg**
entsteht.

Der Satz ist leicht zu unterschreiben und leicht zu unterlaufen. Ein „zweiter Weg" beginnt
selten mit einer zweiten Datenquelle — er beginnt mit einer zweiten **Aufbereitung**
derselben Quelle, die anfangs nur umsortiert und irgendwann rundet, filtert und beschriftet.
Dann sagen Cockpit und Dashboard verschiedene Dinge über denselben Bestand, und niemand kann
sagen, welches recht hat. Genau dieses Muster hat in dieser Organisation dreimal einen Befund
erzeugt: B033, B059 und B064 — zuletzt vier Wochen alt.

Die Entscheidung ist deshalb nicht *ob* eine Quelle, sondern **wo die Verwandlung von Daten
in Bild stattfinden darf**.

## Entscheidung

1. **Der Server bereitet für das Dashboard nichts auf.** Kein `backend/dashboard.py`, kein
   Widget-Endpunkt, keine widget-spezifische Sicht auf den Payload. Das Dashboard holt
   `GET /api/cockpit` — denselben Aufruf, den das Cockpit macht — und rendert daraus im
   Browser. **Verworfen:** eine serverseitige Widget-Aufbereitung. Sie sähe sauber aus
   („Trennung von Belangen") und wäre die zweite Aufbereitung, die SWR-092 verbietet: ab
   dem ersten Feld, das sie anders benennt oder anders rundet als `cockpit`, ist die Drift
   da und niemandem aufgefallen.

2. **Ein Abruf, nicht einer je Kachel.** Der Vertrag hält fest, dass `GET /api/cockpit`
   **immer** alle Einträge liefert und es einen Einzelabruf nicht gibt. Das Dashboard holt
   den Gesamtpayload einmal und verteilt ihn auf die Kacheln — auch für die Detailseite
   (SWR-094), die damit keinen eigenen Abruf braucht. **Verworfen:** ein Abruf je Widget.
   Bei 16 Kacheln wären das 16 Anfragen für eine Antwort, die es in einer gibt, und 16
   Gelegenheiten, dass zwei Kacheln verschiedene Stände zeigen.

3. **Die Marke „keine Daten" entsteht an genau einer Stelle im Frontend.** Seit SWR-108
   sagt der Payload selbst, was er meint: `null` heißt „nicht geliefert", der leere Wert des
   Typs heißt „echte Null". Diese Übersetzung ins Deutsche gehört in **eine** Hilfsfunktion
   in `app.js`, die die Cockpit-Karte und jedes Widget gemeinsam benutzen. **Das ist eine
   Auflage an den Bau und keine Beschreibung des Ist-Standes:** die drei Fälle stehen heute
   ausgeschrieben in `cockpitKarte` (SWR-108, Sprint 4). Wer sie fürs Widget abschreibt, hat
   die zweite Liste — nur in JavaScript statt in YAML. Der erste Schritt von SWR-096 ist
   deshalb, sie aus `cockpitKarte` herauszuziehen, nicht, sie ein zweites Mal zu schreiben.

4. **Die Widget-Konfiguration ist Zustand und liegt deshalb beim Server** (SWR-093), im
   Muster der Konfigurator-Einstellungen aus P7. Das ist **kein** Widerspruch zu Punkt 1:
   aufbereitet werden dort keine Projektdaten, gespeichert wird, was der Mensch sehen will.
   Die Trennlinie dieses ADRs verläuft zwischen *Daten über die Organisation* (nur eine
   Aufbereitung, serverseitig, `aggregation`) und *Vorlieben des Betrachters* (Zustand,
   serverseitig, eigener Speicher). **Verworfen:** Speichern im Browser. Die Einstellung
   soll einen Serverneustart überleben und auf dem Handy dieselbe sein wie am Rechner.

5. **Das Mail-Widget bekommt keinen eigenen Datenweg** (SWR-095). Es rendert zur Laufzeit
   hinter dem vorhandenen PIN-Lesegate (ADR-006), wie der Team-Reiter seit P7, und schreibt
   nichts — kein Cache, keine Datei, kein Log. Das ist die harte Auflage aus der Gründung
   von `team-dashboard` (Guardrail 2): landet je ein Digest-Inhalt in einem Repo, wird das
   Team `sensibel` und verliert seinen Remote. Ein Widget, das „nur zum Schnellermachen"
   zwischenspeichert, löst genau das aus.

6. **Die SWR-096-Tests lesen die Vertrags-YAML.** Sie schreiben die Feldliste nicht in
   Python ab. Die Auflage steht schon in `T-0003` und wird hier zur Architekturaussage: der
   Vertrag ist die einzige Feldliste, und ein Test, der sie kopiert, macht aus einem
   Wächter eine zweite Quelle.

## Konsequenzen

Das Dashboard ist eine **Ansicht** und keine Komponente mit eigener Datenhaltung: neue
Routen und Renderfunktionen in `app.js`, ein Speicher für die Widget-Konfiguration im
Backend, sonst nichts Neues auf der Serverseite. Das Architekturbild bekommt keine neue
Kante auf die Repos — und genau daran lässt sich später prüfen, ob dieser ADR gehalten hat.

**Der Preis, benannt.** Alle 16 Einträge in einem Payload heißt: das Dashboard ist so
schnell wie der langsamste Teil der Aggregation, und ein Fehler in **einem** Eintrag kann
die ganze Seite treffen. Beides ist heute schon so — das Cockpit hat dieselbe Eigenschaft,
und `cockpit_alle` hat bei B059 genau so ausgesehen. Der ADR verschiebt dieses Risiko nicht,
er erbt es. Wird es je zu groß, ist die Antwort ein belastbarer Einzelabruf **in der
Aggregation**, nicht eine eigene Sammlung im Dashboard.

**Bewusst nicht entschieden:** wie viele Kacheln bei 1920×1080 ohne Scrollen passen und was
bei Überlauf geschieht (gruppieren, nur Auffälliges, Favoriten). Das ist die offene
Entwurfsfrage aus dem Projektauftrag; sie hat mit `p11/T-0005` ein eigenes Ticket und eine
eigene Frist. Sie hier mitzuentscheiden hieße, eine Layoutfrage in einem Struktur-ADR zu
verstecken — B025.

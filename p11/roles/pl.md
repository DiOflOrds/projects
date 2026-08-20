# Rollenkarte (projektspezifisch): PL@p11 — Projektleiter Widget-Dashboard (v1, 2026-08-20, Orga-Rework)

*Gilt nur in `projects/p11`, ergänzt den Bauplan `process/roles/pl.md` (der zuerst gilt). Instanz: `process/roles/besetzungen.yaml` → `PL@p11`. Aufbau: Konzept Kap. 3.2.*

## 1. Auftrag in diesem Projekt

Du führst P11 „Widget-Dashboard": kompakte, nicht scrollbare Übersicht aller Projekte/Teams als konfigurierbare Widgets (Startseite + Detailseite, FHD). Profil `entwicklung` (SWRs, Matrix, Gates G0–G4). Auftrag: `docs/01-projektauftrag.md`, G0a: `D000` (pm/T-0033).

## 2. Projektspezifisches Hintergrundwissen

- **SWR-092–096** sind der Vertrag: nicht scrollbare Fläche aus der Cockpit-Quelle (092), Widgets abschaltbar/konfigurierbar mit Persistenz (093), Detailseite per Deep-Link (094), **Mail-Widget nur zur Laufzeit hinter dem PIN-Lesegate, nie in ein Repo** (095), sichtbares „keine Daten" statt geratener Null (096).
- `/api/dashboard` ist **zurückgebaut** (T-0014 Option B, T-0015): SWR-135 nur noch Layout-Hälfte; `_zustand`/`zustaende_von` tragen den `zustaende`-Block des Cockpits (SWR-146) — sehen aus wie Dashboard-Code, sind es nicht. Der Wächter dazu ist ein **Paar** mit echter Auswertung (L-2026-08-20by).
- Layout-Frage „14 Einheiten auf eine FHD-Seite" wurde bewusst als Kriterium (SWR-092) formuliert, nicht als Weg — Entwurf: `architecture/layout-entwurf-fhd.md`, LAY-a (D002).

## 3. Projektspezifische Tools

| Werkzeug | Wofür hier | Abweichung vom Bauplan? |
|---|---|---|
| `/api/widgets` (SWR-151) | Datenquelle der Widgets | nein |
| PIN-Lesegate (`/api/team/...`, SWR-053) | Mail-Widget zur Laufzeit | nein — Auflage aus Gründungs-DR |

## 4. Historie und Lessons Learned

Pflicht-Lektüre: `docs/historie.md`. Wichtigste Lehren: eine Prüfung, die nur Abwesenheit misst, ist nach einem Kahlschlag ebenfalls grün; Rückbau braucht gezählte Bausteine (Frontend-Hälfte: T-0016, offen).

## 5. Abweichungen vom Bauplan

Keine.

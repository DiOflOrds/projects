# Board (generiert von platform/scripts/board.py — nicht von Hand editieren)

Stand: 2026-08-20 · Tickets: 15


## open (1)

| ID | Titel | Typ | Takt | Rolle | Verantwortlich | Prio | Sprint | blockiert durch |
|---|---|---|---|---|---|---|---|---|
| [T-0003](tickets/T-0003.md) | Sprint 1: Widget-Dashboard bauen (ADR, Layout-Entwurf, Umsetzung, Tests, G4) | task | einmalig | pl | Team | hoch | 1 | — |

## in_progress (1)

| ID | Titel | Typ | Takt | Rolle | Verantwortlich | Prio | Sprint | blockiert durch |
|---|---|---|---|---|---|---|---|---|
| [T-0015](tickets/T-0015.md) | Rückbau: /api/dashboard, aggregation.dashboard und KACHEL_FELDER entfernen — SWR-135 auf die Layout-Hälfte zurückschneiden | task | einmalig | dev | Team | niedrig | 1 | — |

## done (13)

| ID | Titel | Typ | Takt | Rolle | Verantwortlich | Prio | Sprint | blockiert durch |
|---|---|---|---|---|---|---|---|---|
| [T-0001](tickets/T-0001.md) | SWE.1: STK-021 + SWR-092–096 (Widget-Dashboard) als Entwurf + Auftrag | task | einmalig | rm | Team | hoch | 0 | — |
| [T-0002](tickets/T-0002.md) | DR: G1 — Anforderungen „Widget-Dashboard\" (STK-021, SWR-092–096) freigeben | decision-request | einmalig | pl | Team | hoch | 0 | — |
| [T-0004](tickets/T-0004.md) | ADR: Wo die Widget-/Layout-Logik liegt — keine zweite Aufbereitung neben aggregation.cockpit | task | einmalig | arch | Team | hoch | 1 | — |
| [T-0005](tickets/T-0005.md) | Layout-Entwurf: passen 16 Kacheln ohne Scrollen auf FHD? (Entwurfsfrage vor dem Bau) | task | einmalig | pl | Team | hoch | 1 | — |
| [T-0006](tickets/T-0006.md) | DR: Das Dashboard verlässt den 62rem-Textkorridor — anders passen 16 Kacheln bei FHD nicht auf eine Seite | decision-request | einmalig | pl | Team | hoch | 1 | — |
| [T-0007](tickets/T-0007.md) | Sprint 1a: LAY-a als ADR festhalten — wo genau sitzt die Korridor-Ausnahme | task | einmalig | arch | Team | hoch | 1 | — |
| [T-0008](tickets/T-0008.md) | Sprint 1b: Backend-Endpunkt auf der Cockpit-Quelle + Widget-Konfiguration mit Persistenz | task | einmalig | dev | Team | hoch | 1 | T-0007 |
| [T-0010](tickets/T-0010.md) | Sprint 1b-a: Dashboard-Endpunkt und breite Kachelansicht (lesend) — die Korridor-Ausnahme einlösen | task | einmalig | dev | Team | hoch | 1 | — |
| [T-0009](tickets/T-0009.md) | Sprint 1c: Detailseiten per Deep-Link + Mail-Widget hinter dem PIN-Lesegate | task | einmalig | dev | Team | mittel | 1 | T-0008 |
| [T-0011](tickets/T-0011.md) | Sprint 1b-b: Widget-Konfiguration mit Persistenz (schreibend) | task | einmalig | dev | Team | mittel | 1 | T-0010 |
| [T-0012](tickets/T-0012.md) | Sprint 1c-a: Deep-Links vom Dashboard in die Detailseiten — auf <projekt>/T-xxxx, nie auf die Nummer allein | task | einmalig | dev | Team | mittel | 1 | — |
| [T-0013](tickets/T-0013.md) | Sprint 1c-b: Mail-Widget hinter dem PIN-Lesegate — eine Kachel mit EIGENER Zugriffsregel | task | einmalig | dev | Team | mittel | 1 | T-0012 |
| [T-0014](tickets/T-0014.md) | Entscheiden: bleibt /api/dashboard ohne Leser bestehen? (Anzeigehälfte von SWR-135 abgelöst) | problem | einmalig | pl | Team | niedrig | 1 | — |

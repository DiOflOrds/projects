# Organigramm: p11

*Generiert aus den Registries (`process/teams/registry.yaml`, `process/roles/besetzungen.yaml`) durch `platform/scripts/organigramm.py` — **nicht von Hand pflegen**, Änderungen gehören in die Registry (Konzept `process/docs/03-rollenmodell-v2-orga-rework.md` Kap. 8).*

**Auftrag:** Widget-Dashboard: kompakte, nicht scrollbare Übersicht aller Projekte/Teams als konfigurierbare Widgets, Startseite + Detailseite je Projekt/Team, Mail-Widget hinter dem PIN-Lesegate

```mermaid
graph TB
  MENSCH["Mensch<br/>Auftraggeber / Gates"]
  PM["PM-Team<br/>koordiniert alle PL"]
  MENSCH --> PM
  p11["p11<br/>entwicklung · aktiv"]
  PM --> p11
  p11_CORE["Core Team<br/>10 Rollen · Cowork/Session · sprint"]
  p11 --> p11_CORE
```

## Beteiligte

| Instanz | Rolle | Motor | Takt | Status | Quelle | Hinweis |
|---|---|---|---|---|---|---|
| ARCH@p11 | Architekt | Cowork/Session | sprint | aktiv | Core Team (implizit) | — |
| CHG@p11 | Change-Manager | Cowork/Session | sprint | aktiv | Core Team (implizit) | — |
| CM@p11 | Konfigurationsmanager | Cowork/Session | sprint | aktiv | Core Team (implizit) | — |
| COACH@p11 | Prozess-Coach | Cowork/Session | sprint | aktiv | Core Team (implizit) | — |
| DEV@p11 | Entwickler | Cowork/Session | sprint | aktiv | Core Team (implizit) | — |
| PL@p11 | Projektleiter | Cowork/Session | sprint | aktiv | Core Team (implizit) | — |
| PROB@p11 | Problemmanager | Cowork/Session | sprint | aktiv | Core Team (implizit) | — |
| QM@p11 | Qualitätsmanager | Cowork/Session | sprint | aktiv | Core Team (implizit) | — |
| RM@p11 | Requirements-Manager | Cowork/Session | sprint | aktiv | Core Team (implizit) | — |
| TEST@p11 | Verifikationsingenieur | Cowork/Session | sprint | aktiv | Core Team (implizit) | — |

Rollen-Bauplan: `process/roles/<rolle>.md` · projektspezifischer Teil: `roles/<rolle>.md` in diesem Repo · Historie: `docs/historie.md`

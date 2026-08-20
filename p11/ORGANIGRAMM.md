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
  DEV_p11["DEV@p11<br/>Cowork/Session"]
  p11 --> DEV_p11
  PL_p11["PL@p11<br/>Cowork/Session"]
  p11 --> PL_p11
  TEST_p11["TEST@p11<br/>Cowork/Session"]
  p11 --> TEST_p11
```

## Beteiligte

| Instanz | Rolle | Motor | Takt | Status | Hinweis |
|---|---|---|---|---|---|
| DEV@p11 | Entwickler | Cowork/Session | sprint | aktiv | — |
| PL@p11 | Projektleiter | Cowork/Session | sprint | aktiv | — |
| TEST@p11 | Verifikationsingenieur | Cowork/Session | sprint | aktiv | — |

Rollen-Bauplan: `process/roles/<rolle>.md` · projektspezifischer Teil: `roles/<rolle>.md` in diesem Repo · Historie: `docs/historie.md`

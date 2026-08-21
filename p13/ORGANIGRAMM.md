# Organigramm: p13

*Generiert aus den Registries (`process/teams/registry.yaml`, `process/roles/besetzungen.yaml`) durch `platform/scripts/organigramm.py` — **nicht von Hand pflegen**, Änderungen gehören in die Registry (Konzept `process/docs/03-rollenmodell-v2-orga-rework.md` Kap. 8).*

**Auftrag:** Produkt-Architekturbilder

```mermaid
graph TB
  MENSCH["Mensch<br/>Auftraggeber / Gates"]
  PM["PM-Team<br/>koordiniert alle PL"]
  MENSCH --> PM
  p13["p13<br/>entwicklung · ohne Status"]
  PM --> p13
```

## Beteiligte

| Instanz | Rolle | Motor | Takt | Status | Quelle | Hinweis |
|---|---|---|---|---|---|---|

Rollen-Bauplan: `process/roles/<rolle>.md` · projektspezifischer Teil: `roles/<rolle>.md` in diesem Repo · Historie: `docs/historie.md`

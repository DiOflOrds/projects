# Organigramm: p12

*Generiert aus den Registries (`process/teams/registry.yaml`, `process/roles/besetzungen.yaml`) durch `platform/scripts/organigramm.py` — **nicht von Hand pflegen**, Änderungen gehören in die Registry (Konzept `process/docs/03-rollenmodell-v2-orga-rework.md` Kap. 8).*

**Auftrag:** Markdown-Renderer auch für Briefe/Reports: Briefe und Sprint-Reports formatiert darstellen wie den Digest — über den vorhandenen Renderer, mit Ticket-Links im Inline-Pass statt eines zweiten Textwegs

```mermaid
graph TB
  MENSCH["Mensch<br/>Auftraggeber / Gates"]
  PM["PM-Team<br/>koordiniert alle PL"]
  MENSCH --> PM
  p12["p12<br/>entwicklung · aktiv"]
  PM --> p12
  DEV_p12["DEV@p12<br/>Cowork/Session"]
  p12 --> DEV_p12
  PL_p12["PL@p12<br/>Cowork/Session"]
  p12 --> PL_p12
```

## Beteiligte

| Instanz | Rolle | Motor | Takt | Status | Hinweis |
|---|---|---|---|---|---|
| DEV@p12 | Entwickler | Cowork/Session | sprint | aktiv | — |
| PL@p12 | Projektleiter | Cowork/Session | sprint | aktiv | — |

Rollen-Bauplan: `process/roles/<rolle>.md` · projektspezifischer Teil: `roles/<rolle>.md` in diesem Repo · Historie: `docs/historie.md`

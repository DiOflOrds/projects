# Konfigurationsmanagement-Plan: P11 (v1.0, Setup-Nachzieh T-0017, CM@p11)

*2026-08-21. Leichtgewichtig; Regeln der Organisation: `process/cm/cm-strategie.md` (gilt, wird nicht kopiert — B033). Hier steht nur, was P11-spezifisch ist.*

## Repos und Storage

Ein Ordner: `projects/p11` (im Sammel-Repo `projects`, Remote via abschluss.cmd). Datenklasse `intern` mit Auflage: **Mail-Inhalte nie committen** (SWR-095) — sonst sensibel + Remote-Verlust.

## Baselines

Anforderungs-Baseline seit G1 (D001); Änderungen daran nur per CR (SWR-135-Rückschnitt lief so: T-0014-Entscheid → T-0015). Release-Baseline mit G3.

## Work Products (SWR-181)

```yaml work-products
- pfad: docs/01-projektauftrag.md
  name: Projektauftrag
  eigentuemer: pl
  pruefstatus: G0 erteilt (D000)
- pfad: docs/projektplan.md
  name: Projektplan
  eigentuemer: pl
  pruefstatus: qm-review bestanden (2026-08-21, pm/T-0075)
- pfad: docs/cm-plan.md
  name: CM-Plan
  eigentuemer: cm
  pruefstatus: qm-review bestanden (2026-08-21, pm/T-0075)
- pfad: docs/qm-plan.md
  name: QM-Plan
  eigentuemer: qm
  pruefstatus: qm-review bestanden (2026-08-21, pm/T-0075)
- pfad: docs/historie.md
  name: Historie und Lessons Learned
  eigentuemer: pl
- pfad: verification/strategie.md
  name: Verifikationsstrategie
  eigentuemer: test
  pruefstatus: qm-review bestanden (2026-08-21, pm/T-0075)
- pfad: architecture/layout-entwurf-fhd.md
  name: Layout-Entwurf FHD
  eigentuemer: arch
  pruefstatus: LAY-a entschieden (D002)
- pfad: roles/pl.md
  name: Rollenkarte PL (projektspezifisch)
  eigentuemer: coach
```

Anforderungen liegen in `requirements/` (RM, eigene DoD) — sie sind über die SWR↔Test-Matrix geführt, nicht doppelt hier.

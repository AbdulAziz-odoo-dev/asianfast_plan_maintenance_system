# Module Roadmap & Conventions — Asianfast Plan Maintenance System

**Status:** Approved foundation, 2026-07-27.
**Purpose:** This module is the ground-up, production-track rebuild of Ship PMS,
replacing `asianfast_pms` (kept in the repo as a reference/archive only — not
installed alongside this module long-term). Rebuilding from scratch, sprint by
sprint, is a deliberate choice: it is meant to build a deeper understanding of
the system's flow while producing documented, production-ready code.

Every sprint spec under `.docs/specs/` inherits the conventions below unless a
spec explicitly overrides one for a stated reason.

## Naming

- **Module (technical name):** `asianfast_plan_maintenance_system`
- **File prefix:** every model, view, and wizard file is prefixed `afg_`
  (e.g. `afg_ship_vessel.py`, `afg_ship_vessel_views.xml`).
- **Technical model name prefix:** every Odoo model name is also prefixed
  `afg.` (e.g. `afg.ship.vessel`, `afg.ship.component`) — not just the file.
  This keeps model names, and therefore table names, distinct from the old
  `asianfast_pms` module's `ship.*` models if both are ever loaded in the same
  database during a transition period.

## Old module (`asianfast_pms`)

- Stays in `custom-addons/` as a reference implementation and learning
  artifact. Not deleted.
- Will be uninstalled from the `pms` database once this module reaches
  production readiness. No permanent side-by-side install in the same
  database is planned.

## Sprint order

Follows the original PRD's phase table (`Ship_PMS_PRD.md` section 15) as-is:

1. Vessel Master + Component Tree
2. Standard Job + Component Job model/forms
3. Running Hours (wizard, cascade, weighted average)
4. Due List + filter bar
5. Sign-out wizard, multi sign-out, postpone
6. Maintenance History, 30-day lock, cancel
7. Security Groups, Record Rules, seed data
8. Certificate Type + actual cert + groups
9. Certificate monitoring cron + alert system
10. Survey record + renewal wizard
11. Spare parts per component, stock integration
12. Material request integration, MR notification
13. PDF reports
14+. Later phases (circulating components, risk analysis/work permits,
     analytics dashboard, RH import) — revisited only once 1-13 are done.

**Security is deliberately sprint 7, not sprint 1** — same reasoning as the
old module: business logic gets built and understood first, without
permission checks getting in the way of hands-on testing as an admin.
Because this module is production-track, security still lands before any
certificate/spare-parts work, well ahead of go-live.

## Documentation standard

- Docstrings in **English** (stated hard requirement, not a style choice).
- Odoo-standard format: `""" Sentence(s), leading and trailing space. """` as
  the first statement inside the function body.
- **Required on:** every `@api.depends` (compute), `@api.constrains`,
  `@api.onchange`, `_cron_*` method, and any `action_*` method containing
  non-trivial logic (more than a single `write`/`return`).
- **Not required on:** trivial getters, simple `action_open_*` methods that
  only return an `ir.actions.act_window` dict, and other self-explanatory
  one-liners.

## Git

- This module is its own nested git repository under
  `custom-addons/asianfast_plan_maintenance_system/`, separate from the Odoo
  core repo and from `asianfast_pms`'s repo — same pattern as the old module.

## Process per sprint

Each sprint goes through: brainstorm (clarifying questions → approaches →
design) → write a dated spec under `.docs/specs/` → self-review → user
review → `writing-plans` skill for the implementation plan → implementation.

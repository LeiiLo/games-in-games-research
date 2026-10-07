# For coding agents and experts

A compact map of the repository. Everything here is plain Markdown; there is no code to run.

## Reading order for a planning task

1. `guides/01-choosing-a-route.md`: four questions that select an architecture. Do this first.
2. `guides/02-bridge-contracts.md`: ownership, units, transport and lifecycle. Write the contract before bridge code.
3. Route-specific guides, as needed: `03-frame-compositing`, `04-geometry-transfer`, `05-collision-and-combat`, `06-rebuilt-engine-in-a-host`, `07-reconstruction-and-fidelity`, `10-content-mods-and-assets`.
4. `guides/08-installers-and-diagnostics.md`: install, load order and loaded-code checks.
5. `guides/09-evidence-levels.md`: label every claim you report.

## Where the examples are

1. `case-studies/README.md` indexes one page per project. Each page gives versions, who owns the player, units, transport and the problems encountered.
2. `SOURCES.md` lists every named source with its pinned commit, tag or video.

## Rules for using this material

1. Versions, builds and measurements belong to the pinned version. Re-check the project's current release before relying on a detail.
2. Treat creator measurements as the creator's claims.
3. Do not assume a project used AI unless its own README, credits or a commit co-author line says so.
4. Licence: text CC BY 4.0, code MIT (`LICENSE`).

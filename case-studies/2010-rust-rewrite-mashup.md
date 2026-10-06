# 2010 Rust Rewrite Mashup: MW2, Skate and Minecraft in one runtime

**Repository:** [chasmlol/2010-rust-rewrite-mashup@f608f85](https://github.com/chasmlol/2010-rust-rewrite-mashup/tree/f608f85e407ff1b7689d54a9aafdd16e95711ac4) (v0.4.0)  
**Route:** engine recreation and fusion  
**What we read:** README, context and Skate docs, captured history, and the v0.2.0 to v0.4.0 materials. Nothing was run.  
**AI credit (as the project states it):** commits co-authored by Claude; the creator also describes weeks of manual tuning.

A single Rust/Bevy runtime combining [IW4L](iw4l.md), the rebuilt [Skate engine](skate-3-rust-engine.md) and a Minecraft reimplementation. The original MW2, Skate 3 and Minecraft don't run. MW2 assets come from the player's install; Skate data is converted from the player's extracted Xbox 360 files; Minecraft assets download on first run.

**What we learned**

- **Fusion is the work.** Skating runs as a worker simulation. The MW2 map's collision doubles as the skating world, rails are derived from walkable edges, Skate bones are retargeted to the soldier skeleton, and a key switches control. Writing everything in Rust didn't make the engines fit by itself.
- **Scripts are fidelity.** GSC compiles to an intermediate form and runs on authority frames; scheduling, event order, aliasing and seeds matter. Errors yield undefined values and carry on.
- **Foreign maps keep MW2 rules.** Maps from other titles contribute entities and assets; their scripts are dropped.
- **Date every bug report.** v0.2.0 added block damage, drops, inventory and mobs; v0.3.x moved matches to MW2's own scripts and fixed controller input for skating; v0.4.0 added grind rails on block edges and fixed reloads, grenade stalls and flicker. Bots got slower once scripts ran.
- **Benchmarks with scope.** Its performance doc compares five-run medians on one machine and one no-bot route (183 → 362 fps) and says the heavy-bot case wasn't measured.
- 36 host units per block.

**Still missing in the docs we read:** live bomb plant and defuse, full multiplayer, and persistent replay checks. Boards can be invisible on some maps.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

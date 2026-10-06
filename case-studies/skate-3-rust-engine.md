# SK8-ENGINE: a rebuilt Skate 3 runtime

**Repository:** [SK8-ENGINE/skate-3-rust-engine@4488651](https://github.com/SK8-ENGINE/skate-3-rust-engine/tree/4488651c35c44365faa1ba38eed5758b6ebde714) (default branch, 6 Oct 2026)  
**Route:** engine recreation (foundation for several mashups)  
**What we read:** three source archives and their docs. Nothing was run.

Converts the player's own Skate 3 Xbox 360 data for a Rust/Bevy runtime. It's the foundation under [BullySkate](bullyskate.md), [SkateGM](skategm.md), [World of Skatecraft](world-of-skatecraft.md) and the [2010 mashup](2010-rust-rewrite-mashup.md).

**What we learned**

- **Credit the human research.** The README credits years of earlier research and extraction tools and an earlier recompilation attempt before the Rust engine. AI use is acknowledged too; it doesn't replace that history.
- **SDK contracts.** Lua owns game policy; native code exposes generic primitives. Session authority changes are asynchronous and ratified by the host. Settled trick counters and event cursors stop fake achievements and repeated triggers. Overlap queries use real hulls but are sampled, not swept. A local command receipt isn't a remote acknowledgement.
- **Example mods show the contracts in use.** Broken Bones changes joint limits after hard impacts; Simon Says reads the settled clean-trick counter instead of trusting a client; a wipeout course separates first contact from repeated contact.
- **Vehicles and deformation.** A Skyline car mod cooks a compound collider in memory from the full mesh and uses raycast tyres. The deformation SDK uses an impact-driven lattice with convex pieces, capped at 64 pieces and 8,192 vertices, with an owner sending revisioned state. It's inspired by soft-body games, not a full soft-body simulation.
- **Keep original-game observations apart from runtime tests.** A grind crash was a missing native adapter, resolved against the original assembly; a quarter-pipe error was a wrong input action ID. One note says plainly that the native game wasn't launched for a staging change.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

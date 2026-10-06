# Halo / MW2 Director: three modes in one engine

**Repository:** [0xburn/halo-mw2-director@48da3f6](https://github.com/0xburn/halo-mw2-director/tree/48da3f656ed9f570e022a465254733c9d49e8fae) (default branch, 6 Oct 2026)  
**Route:** engine recreation plus map import plus an authored cinematic  
**What we read:** README, notice and six docs. Nothing was run.

Patches the [IW4L](iw4l.md) Rust MW2 runtime on macOS (Metal) and has three separate modes:

1. **An authored cinematic.** Halo CE characters retargeted to MW2 rigs act out a timed script; the Warthog follows a fixed route. It looks like play; nothing reacts.
2. **An imported map.** A local Halo CE map becomes geometry, textures, collision and spawns, but MW2's movement, weapons and rules run it. Scenery has no collision; teleporters, pickups and vehicles are missing.
3. **A bot match.** MW2 soldiers against Halo-style "Spartans" with changed movement and accuracy. No Halo shields. The movement change needs a new network protocol version, so older builds don't match.

**What we learned:** say which mode a clip shows. A cinematic, a map import and a reactive match are three different claims.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

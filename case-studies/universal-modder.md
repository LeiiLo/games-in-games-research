# Universal Modder: worked examples and field notes

**Repository:** [rehan-remade/universal-modder@0f5dcdf](https://github.com/rehan-remade/universal-modder/tree/0f5dcdfdcd8ed420f8413815bd6647586ab894a2) (main as of early October 2026)  
**Route:** agent tooling plus worked examples (frame compositing, native content mods, host-API recreations)  
**What we read:** the GTA V example and two content examples in full (text files), plus the knowledge notes. Nothing was run.  
**AI credit (as the project states it):** README credits Claude Code for the GTA V example; field notes name the agent used.

Universal Modder is a set of agent skills, engine playbooks and a knowledge base of field notes. It isn't a universal engine. Its value for this research is the worked examples, which are documented well enough to read end to end.

**Minecraft × GTA V (`examples/minecraft-gta5-passthrough`).** Minecraft Java and GTA V Legacy both run. Gameplay messages go over a localhost WebSocket; three Minecraft images (world colour, depth, and hand plus HUD) travel through named shared memory; a ReShade add-on composites them into GTA's frame. Its README credits Claude Code. The author's tests used build 3889 of GTA V Legacy (ScriptHookV build 3889.0, ReShade version 6.8.0).

- Ownership changes with mode: GTA's pose places the Minecraft player on foot; Minecraft owns movement in elytra flight while GTA supplies the camera.
- Units: one block per GTA metre; axes map as x → x, z → height (plus an offset), y → −z; yaw = 180 − heading.
- The transport is three GPU readbacks into three shared-memory slots, then host uploads. The shader reprojects with 24 log-spaced depth samples and three refinements, capped at 400 m.
- Lighting comes from a blurred host image plus screen-space contact shadows. It doesn't put guest blocks into GTA's shadow system.
- Collision is a sampled walking surface (160 probes per frame, radius 40) plus at most 400 frozen GTA boxes for placed blocks. Slabs and stairs are just "solid".
- Combat crosses as events through invisible proxies on both sides, with explosion de-duplication.
- The included `ws_test.cpp` passes if any message contains `explosion`. It doesn't check camera, depth or reconnects.
- The author says the demo cuts around a stale overlay on GTA's pause menu, and the fix wasn't built.
- Static reading found cleanup gaps worth testing: barrier tracking forgotten on detach, broad mob cleanup, `--remove` deleting a `ReShade.ini` the user may have had before.

**Native content examples.** Fal Arsenal (Terraria, tModLoader C#) and San Franciscans (an AoE2 DE civ) show the asset and data pipelines in [guide 10](../guides/10-content-mods-and-assets.md). Fal Arsenal's README says multiplayer is untested, and the nuke edits each machine's world with no tile sync.

**Agent bridge reference.** A Terraria reference bridge queues localhost JSON-lines requests onto the main thread and finds UI handlers instead of screen coordinates. Its "step" doesn't advance exactly one tick.

**Knowledge cases.** Field notes for Borderlands 3 features in Borderlands 2, Counter-Strike movement in Elden Ring, a Darkest Dungeon randomiser, Dragon's Dogma 2 Blink Behind, a Half Sword hammer mod, Halo content in Minecraft, a Mewgenics roster panel, a Minecraft dimension mod, a Stickman choices mod, a Victoria II total conversion and a WoW armour backport. Each records its own tests. Most are host-API recreations or data mods, which is a useful counterexample to "you need two decompiled games".

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

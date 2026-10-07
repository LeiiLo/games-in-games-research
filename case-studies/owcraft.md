# OWCraft: Minecraft inside Outer Wilds

**Repository:** [Yaekai/OWCraft@cd5f061](https://github.com/Yaekai/OWCraft/tree/cd5f0613e1de04c537e7db28a5813b0329cfbfa9) (default branch, 6 Oct 2026)  
**Route:** live state bridge (host draws guest meshes)  
**What we read:** READMEs, design notes and the change log for 0.1.0 and 0.1.1, and the commit pages for both. Nothing was run.  
**AI credit (as the project states it):** the README says AI wrote the mod under human direction and playtesting; commits credit Claude.

Outer Wilds stays the world and the renderer. A hidden real Minecraft handles movement, blocks and inventory, and its meshes are drawn inside Outer Wilds with the host's lighting; only the hand, HUD and menus arrive as separate images. It reuses SkyCraft's protocol, moved from a C++ host plugin to a C# one.

**What we learned**

- **Round planets need flat patches.** Each planet is split into tiles of roughly 40 m, and each tile gets its own flat Minecraft patch. Placed blocks are anchored to the planet, so they move with it. Minecraft's physics is still flat within a tile.
- **Changing patches is where it breaks.** Early versions teleported the player between patches, which zeroed velocity and froze elytra flight. Later versions shift the player relative to the new patch and keep momentum. Airborne tiles stay valid for longer than ground tiles, so flight changes patch less often.
- **Hand the native body back cleanly.** Entering the mod suspends the Outer Wilds body; leaving, dying, loading or losing the guest restores it with the planet's velocity.
- **Collision has a frame budget.** Host colliders are collected on the main thread and voxelised on workers, a few regions per frame. Sampling walls from the side fixed thin trunks but caused frame spikes the creator measured, so later builds skip side samples on static walls after the first pass.
- **Keep the release log honest.** The 0.1.1 log records real failures and retests, such as an old guest build paired with a new host, and a landing problem that stayed open. The 0.1.0 log says the released build hadn't yet been tested through the normal launcher.
- **Docs and code can disagree.** The design notes and the README describe some features differently from the code; record both.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

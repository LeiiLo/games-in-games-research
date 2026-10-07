# SubCraft: Minecraft inside Subnautica

**Repository:** [FumperForrest/SubCraft@26a936d](https://github.com/FumperForrest/SubCraft/tree/26a936d73e723e2e611666ef80b2d9a980cb1292) (default branch, 6 Oct 2026)  
**Route:** live state bridge (host draws guest meshes)  
**What we read:** README and commit pages. Nothing was run.  
**AI credit (as the project states it):** commits co-authored by Claude.

A hidden Minecraft (NeoForge 1.21.1) owns the player's physics, inventory, combat, blocks and mobs. Subnautica (BepInEx) keeps its terrain, creatures, light, sound and saves, and draws Minecraft's geometry with its own shaders. It follows SkyCraft's shared-memory approach.

**What we learned**

- **Build it in stages.** The README splits the work into a link test, a puppet with physics, and native look, each marked done by the creator, before exact collision, which was still in progress.
- **One layout file is the source of truth.** The shared-memory layout lives in one header with tests against a layout description, so both sides can't drift.
- **Fake-host tests are their own evidence level.** The commit history starts with a fake host before native rendering, oxygen, combat and audio. Results from the fake host don't count as Subnautica tests.
- **Other guest mods need a plan.** The history adds support for Minecraft mods that draw their own geometry, by capturing generic vertex buffers and turning off a competing renderer.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

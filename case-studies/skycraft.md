# SkyCraft: Minecraft inside Skyrim

**Repository:** [chasmlol/SkyCraft@bfcaf17](https://github.com/chasmlol/SkyCraft/tree/bfcaf178524b92c2cdeb88e4ce0f13ef9ded6f32) (v0.1.2)  
**Route:** live state bridge plus geometry transfer  
**What we read:** source of the protocol header, world exporter and collision code; README and release notes for 0.1.0 to 0.1.2. Nothing was run.  
**AI credit (as the project states it):** commits co-authored by Claude in the history we read.

Two real games run together. An SKSE C++ plugin in Skyrim and a Fabric mod in Minecraft share state through named shared memory. Minecraft supplies player physics, inventory, blocks, items and entities. Skyrim draws the exported Minecraft geometry in its own world with its own lights and shadows, and its collision is represented on the Minecraft side.

**What we learned**

- **Versioned shared memory with liveness.** The header carries a protocol version, process IDs, heartbeats, epochs and teleport sequences, so each side can tell a current peer from stale data. Camera and player state use a seqlock. Minecraft's previous and current tick are both kept so Skyrim can interpolate 20 Hz ticks into its own frames. One block is 70 Skyrim units.
- **Bounded channels for everything.** Input and events, actor records, a 32 MB collision stream and a 64 MB render stream each have a fixed budget. The Java and C++ layouts are mirrored by hand, which is a drift risk worth a layout test.
- **A geometry exporter with a time budget.** Minecraft's own block and fluid renderers produce triangles and atlas updates, limited to 12 sections and about 3 ms of scheduling per frame, with recent edits first.
- **Three collision representations.** Exact triangles for the local player, 8×8×8 sub-voxel shapes for other entities, and coarse occupancy elsewhere. It distinguishes "known empty" from "not received yet".
- **Digging goes both ways.** Minecraft records dug cells and supplies interior walls; Skyrim removes matching render and collision surfaces. Non-diggable triangles such as protected buildings are declined.
- **Hand-back is explicit.** Furniture, levers, mounts, kill-moves and scripted scenes return control to Skyrim. Inventory, magic, shouts and perks aren't available while Minecraft drives.
- **Versions differ a lot.** v0.1.0 set up the player bridge; v0.1.1 added digging into Skyrim and fixed connection failures; v0.1.2 fixed explosion lag (the commit cites a profile where the old probe took 86% of server-thread time) and added a destruction toggle.
- **A design doc isn't the shipped design.** An earlier draft proposed GPU colour/depth textures and separate interior slots. The shipped code exports meshes instead. Read the code for the version you use.

**Limits the project documents:** guest multiplayer shares Minecraft only; each player keeps their own Skyrim, so quests, NPCs and saves aren't shared. Interiors share coordinates. Some quest objects can't be hit. The opening cart scene can stall while Minecraft owns movement. Development targets the Skyrim AE 1.6/1.7 runtime family.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

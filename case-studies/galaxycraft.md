# GalaxyCraft: Minecraft inside Super Mario Galaxy 2 (Dolphin)

**Repository:** [M0uidev/GalaxyCraft@39269b3](https://github.com/M0uidev/GalaxyCraft/tree/39269b3b03afd10347e88dc26be4d728944632ae) (default branch, 6 Oct 2026)  
**Route:** geometry transfer into an emulated game  
**What we read:** README, protocol, header and layout test. Nothing was run.

Three programs share the work: Minecraft/Fabric for blocks, inventory and meshes; Dolphin running Super Mario Galaxy 2; and a native module inside the game for rendering, gravity, collision and Mario's adaptation.

**What we learned**

- Minecraft data reaches the game as GameCube display lists, textures and KCL collision, so the original renderer draws it.
- Two byte orders: the host protocol is little-endian (v10), the emulated mailbox big-endian (v5), and much model data is already big-endian and mustn't be swapped twice.
- A C file of compile-time assertions pins offsets, region lengths and struct sizes. Copy that idea.
- Two ownership modes: native Mario movement, or Minecraft movement with Mario hidden and Steve drawn.

We read the README, protocol, header and layout test. The Fabric side, host and native module weren't read. The README describes broad block and entity coverage, but the protocol has finite caps.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

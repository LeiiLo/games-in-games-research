# CS:Craft: Counter-Strike movement in a Minecraft survival mode

**Repository:** FrosttysBots/CS-Craft@9b0723a (default branch, 6 Oct 2026). Not linked: its release may contain Minecraft's game files.  
**Route:** engine recreation plus an added game mode  
**What we read:** docs and part of the captured commits. Nothing was run.  
**AI credit (as the project states it):** the README says it was written by Claude.

A Rust/Bevy runtime that reads CS:GO Legacy formats (BSP, VPK, VTF/VMT, MDL and friends), with a fixed 64 Hz simulation and interpolated rendering, plus a Minecraft-reimplementation survival mode with CS movement, weapons and economy.

**What we learned**

- Its parity doc says outright that it's an adaptation: dragon AI, portals, effects and CS-scaled combat differ, and enchanting, brewing, advancements and multiplayer were unfinished. Write a doc like that.
- Its completion test uses staged fixtures; it isn't a manual playthrough.
- A Java reflection exporter reads the installed dragon model and poses without decompiling the client.
- Stutter fixes: terrain pregeneration, shader warm-up, bounded uploads, stable mesh handles, avoiding change notifications, and lighting updates on workers.
- **Check what the binary package contains.** The source repo says no assets are copied; the Windows release bundles a Minecraft client JAR and assets, and its release README says so.
- A plan to share content IDs, weapon definitions, movement profiles and a trace interface with IW4L was still a design.
- Source (x, y, z) maps to Bevy (x, z, −y), with inches kept.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

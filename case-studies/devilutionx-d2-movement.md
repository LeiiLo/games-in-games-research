# Diablo II Movement for DevilutionX

**Repository:** [ITSTDMCC/DevilutionX-D2-Movement@c8a67ae](https://github.com/ITSTDMCC/DevilutionX-D2-Movement/tree/c8a67aead1abe43bd6dab241314d482c02b1a069) (default branch, 6 Oct 2026)  
**Route:** mechanic transplant inside a rebuilt engine  
**What we read:** source and harness docs. Nothing was run.  
**AI credit (as the project states it):** commits co-authored by Claude models.

Puts Diablo II-style continuous movement into [DevilutionX](devilutionx.md). Diablo I's tile occupancy, combat and saves stay underneath.

**What we learned**

- **Use the original as an oracle.** It compares its movement tables against the player's own Diablo II 1.12 `D2Common.dll` and an MIT-licensed reimplementation. The direction table is computed, not copied from game data.
- **Keep a way back.** If continuous movement stalls, it re-centres the hero and returns to stock tile walking.
- **Units:** 256/8 fine units per tick at 20 ticks per second in v0.2.
- **Date your reports.** v0.1 used Diablo II speeds everywhere; v0.2 keeps Diablo I's dungeon pace. An older report in the repo still describes v0.1.
- **Read check counts carefully.** Its report says 65/65 checks, but they measure different things (table agreement, travel time, unchanged level hashes, best-of-several frame rate), and the owned-file checks skip when files are missing.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

# DoomMaps: Doom on Hytale's world map

**Repository:** [ssquadteam/DoomMaps@2e782ad](https://github.com/ssquadteam/DoomMaps/tree/2e782ad476d9c5b8698d81b2f09ababdd1adc37d) (default branch, 6 Oct 2026)  
**Route:** guest frames shown on a host surface  
**What we read:** README and the commit history (January 2026). Nothing was run.

A Hytale plugin that runs a Java Doom engine (MochaDoom) and draws its frames on Hytale's world map, sending only the parts that changed. The player supplies doom1.wad, and sound isn't done.

**What we learned**

- **A frame bridge isn't a merged world.** Doom runs whole and its picture lands on one host surface; Doom's levels never become Hytale terrain. Putting a game "in" another can mean either, and the two need different work.
- **Host controls become guest input.** While the map is open, the player's movement and hotbar slots stand in for Doom's turn, fire, use, weapon and menu keys.
- **The frame rate is the creator's figure.** The README gives 35 FPS with delta compression; we didn't measure it.
- **Not every bridge comes from the autumn 2026 wave.** The whole history is from January 2026, and the project doesn't credit AI.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

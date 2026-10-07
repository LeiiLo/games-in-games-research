# Minebonk: Minecraft mechanics rebuilt inside Megabonk

**Repository:** [MGuibas/Minebonk@d66a56d](https://github.com/MGuibas/Minebonk/tree/d66a56d8470ca3181309ffc35748c3c462927399) (default branch, 6 Oct 2026)  
**Route:** guest rules recreated inside the host (one process)  
**What we read:** README (0.1.7) and commit pages. Nothing was run.  
**AI credit (as the project states it):** commits credit Claude.

A BepInEx (IL2CPP) and Harmony mod that rebuilds Minecraft's player, hotbar, combat, loot and mob look inside Megabonk. No Minecraft process runs; textures and sounds come from the player's own Minecraft files or a download the player agrees to.

**What we learned**

- **One process still has overhead.** The commits cut scene scans, cache collider lookups, give each mob one shadow caster, lower animation updates with distance and cull limbs. The creator's claim of no extra cost isn't a measurement.
- **Host events need adapting.** Mace landings need a check for the held weapon; explosion callbacks need a guess at the cause of a kill; pooled objects need resetting.
- **The host may recreate the player.** Megabonk builds a new player on each stage, so inventory, enchantments, XP and food have to be handed over explicitly. That handover is in memory, not a save.
- **Field of view affects imported viewmodels.** Changing the camera's field of view changed the first-person Minecraft hand, which needed a size and depth correction.
- **"1.21 behaviour" is a goal.** Parity with Minecraft 1.21 isn't measured.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

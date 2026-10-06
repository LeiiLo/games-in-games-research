# BullySkate: the rebuilt Skate engine inside Bully

**Repository:** [Faiqie/BullySkate@761c8de](https://github.com/Faiqie/BullySkate/tree/761c8def656b54481fa4bbbefba4156dd6300d34) (default branch, 6 Oct 2026)  
**Route:** rebuilt guest engine in worker processes  
**What we read:** source, read in full. Nothing was run.

A 32-bit Bully adapter talks to a 64-bit Skate physics worker and a separate sound worker. The player's own extracted Skate 3 data is the input. The original Skate 3 executable never runs.

**What we learned** (from source we read in full)

- **A fixed, pointer-free contract.** One 6,184-byte block, scoped by process ID: up to 24 actors, 8 vehicles and 8 input samples, rider and board pose, 64 sound values.
- **A separate channel for sound.** 296 bytes, odd/even sequence, three reader retries. The worker mutes after 250 ms without a new sequence instead of looping stale sound.
- **Axes once.** Bully is Z-up, Skate Y-up: (x, z, −y), and mount yaw is adjusted by π.
- **The guest keeps its own clock** from queued input durations, merges input on overflow and caps catch-up at 0.1 s. A long stall can drop a button press.
- **Bound lifetimes** with parent watching, health checks and a Windows job object.
- **Check the loaded code.** It launches Bully through Steam and requires all 203 code fingerprints and 19 data locations before installing hooks.
- **Version generated files.** Its grind-rail file moved from segments to polylines with a schema bump the launcher checks.
- **Physical collision, not navigation volumes.**

**Install issues we found by reading:** copies happen in steps with no staging, and the loader version check compares decimals, so 15.10 would read as 15.1. Its MIT licence covers the project's own parts, not the third-party code it includes.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

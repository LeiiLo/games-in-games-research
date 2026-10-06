# World of Skatecraft: the Skate engine inside a recreated WoW client

**Repository:** [Kimmo3223/world-of-skatecraft@73c0977](https://github.com/Kimmo3223/world-of-skatecraft/tree/73c09770e2da11c618d224e96c1ddcfa1dd87953) (default branch, 6 Oct 2026)  
**Route:** rebuilt guest engine inside a recreated host  
**What we read:** docs and captured history. Nothing was run.  
**AI credit (as the project states it):** README describes vibe coding; commits co-authored by Claude.

Adds the rebuilt Skate engine to [benilla](benilla.md), a recreated World of Warcraft 1.12.1 client, through an extension point that accepts extra Bevy plugins. No foreign process runs.

**What we learned**

- You inherit the recreated host's fidelity along with its convenience.
- A feature can depend on a server: the skateboarding profession needs a patched server on Linux, and the experimental Windows setup uses a stock server without it.
- The shared history records fixing tests whose names promised more than they checked, and screenshot sweeps that passed without checking anything, and later making a skipped test fail.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

# ValCraft: Minecraft inside Valheim

**Repository:** [LoAlCo/ValCraft@cf1b4fc](https://github.com/LoAlCo/ValCraft/tree/cf1b4fcfc34fb5231762296a99be936b1b065505) (default branch, 6 Oct 2026)  
**Route:** live state bridge  
**What we read:** README and early commit pages. Nothing was run.  
**AI credit (as the project states it):** README says it was built with Claude Code; commits co-authored by Claude.

Minecraft runs hidden and simulates the player through its Fabric mod, and Valheim's BepInEx plugin exchanges state with it over shared memory.

**What we learned:** collision streaming needs a policy for missing data. Early ValCraft prefetches host regions in the direction of travel, holds the guest at its last position (keeping momentum) while data is missing, and makes the host puppet kinematic so a gap can't fling it. A two-second timeout then lets movement continue, which reduces the problem rather than removing it.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

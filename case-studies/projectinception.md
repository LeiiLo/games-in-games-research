# Project Inception: Minecraft inside Minecraft

**Repository:** [Arc-blroth/ProjectInception@b782ef3](https://github.com/Arc-blroth/ProjectInception/tree/b782ef337282f95b07af050a034c43a012488f26) (default branch, 6 Oct 2026)  
**Route:** second game process shown inside the host  
**What we read:** README (1.2) and commit titles from 2020. Nothing was run.

A Fabric mod for Minecraft 1.16 that starts a second Minecraft and shows it inside the first. The two processes talk through memory-mapped files. It is client-only, and its releases date from September 2020.

**What we learned**

- **This route predates the AI wave.** The project shipped in 2020 and doesn't credit AI. A second game process with shared-memory messaging was already a working pattern then.
- **The commit history shows the hard parts.** Titles from August and September 2020 move texture reads off the render thread, add cleanup for several instances and for stopped servers, unlock input, and then move the inner game into a separate process. Titles say what was attempted, not what worked.
- **Shared memory has limits.** The README says the mod doesn't work from a network drive.
- **Accounts limit nesting.** Multiplayer inside the inner game is disabled; the author's explanation is that one account can't be signed in to two servers at once.
- **Crashes can show up in the operating system's kernel.** The README lists a kernel heap-corruption crash as believed fixed but not confirmed.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

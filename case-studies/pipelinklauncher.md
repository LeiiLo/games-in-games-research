# PipeLink: installer and converters around GTA San Andreas, Skate and MW2

**Repository:** Sm1jjj/PipeLinkLauncher@e0f3014 (default branch, 6 Oct 2026). Not linked: we couldn't see what its launcher downloads, which may include unauthorized executable patches (needs checking).  
**Route:** installer plus asset converters  
**What we read:** captured initial commit. Nothing was run.  
**AI credit (as the project states it):** commits co-authored by Claude.

**What we learned**

- **Load early enough.** A `dinput8.dll` proxy loaded too late for GTA San Andreas's mod loader and the project's own plugin. It installed the same loader under the name of a DLL the game imports at startup and kept the original under a new name.
- **List every loader before deciding.** Its early return when a loader already exists happens before obsolete copies are cleaned up.
- GTA is Z-up and the Skate engine Y-up, so the converters swap axes once, at the boundary.

We read the initial commit; the launcher body wasn't visible.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

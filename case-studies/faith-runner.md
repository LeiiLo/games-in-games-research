# Faith Runner: Mirror's Edge movement in Skyrim and Minecraft

**Repository:** [tnrjns/faith-runner@9470d75](https://github.com/tnrjns/faith-runner/tree/9470d755b6aa4e4fa9257fc1326abcfc3ad4b028) (default branch, 6 Oct 2026); [tnrjns/faith-runner-skyrim@f00ad21](https://github.com/tnrjns/faith-runner-skyrim/tree/f00ad21345091cae5b394913bd24b94f8ba5a5f9) (default branch, 6 Oct 2026); [tnrjns/faith-runner-minecraft@291bed6](https://github.com/tnrjns/faith-runner-minecraft/tree/291bed644507a3bf850980383bdc343f02fc9676) (default branch, 6 Oct 2026)  
**Route:** one mechanic library, several hosts  
**What we read:** partial: READMEs and host interface code. Nothing was run.  
**AI credit (as the project states it):** commits co-authored by Claude Opus 5.5 in all three repositories.

Faith Runner rebuilds selected Mirror's Edge movement in Rust. The library is statically linked into an SKSE plugin for Skyrim and loaded by Minecraft through Java's foreign-function API.

**What we learned**

- The host only has to supply box sweeps and overlap queries, so Skyrim's Havok, Minecraft's blocks or a Bevy greybox can all provide collision.
- Skyrim uses 70 units per metre in this port.
- Behaviour parameters come from extracted UnrealScript and native code read in Ghidra. Where something wasn't decoded, the project substitutes: an undecoded vertigo check is replaced, and auto-step is enabled although the original disabled it.
- The doubled gravity is inferred from a developer comment; where the doubling happens was unresolved. Matching numbers isn't matching behaviour.
- The Skyrim port guesses ladders and fixtures from collision shapes, and moving objects stay stale until reread.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

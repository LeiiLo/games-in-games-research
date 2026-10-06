# benilla: a recreated World of Warcraft 1.12.1 client

**Repository:** [samwhosung/benilla@2e82d34](https://github.com/samwhosung/benilla/tree/2e82d34c1b561e3bb033f7cd910036ba63ce1760) (default branch, 6 Oct 2026)  
**Route:** engine recreation  
**What we read:** docs and captured history. Nothing was run.

A Rust/Bevy client that reads WoW 1.12.1 (build 5875) data. [World of Skatecraft](world-of-skatecraft.md) builds on it.

**What we learned:** an extension entry point that accepts extra engine plugins is what made a second engine (Skate) easy to add. The history is also a good example of tightening tests: names that over-promised were corrected, and "green" screenshot sweeps that didn't check anything were fixed.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

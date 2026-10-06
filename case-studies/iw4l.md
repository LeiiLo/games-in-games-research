# IW4L: a Modern Warfare 2 runtime in Rust

**Repository:** [vladtrc/iw4L@d48a9f6](https://github.com/vladtrc/iw4L/tree/d48a9f650235298399709020764e63f8141353b1) (v0.1.0-demo.2)  
**Route:** engine recreation  
**What we read:** source snapshot, notice and release pages up to demo.2. Nothing was run.  
**AI credit (as the project states it):** a notice in the repository discloses LLM-assisted coding.

A Rust/Bevy/wgpu runtime that decodes MW2 assets from the player's install, normalises them, and translates D3D9 shader logic toward WGSL. It separates authoritative state, input, client prediction and snapshots.

**What we learned**

- **Credit prior work.** Its notice credits several earlier asset, protocol and engine projects, and discloses LLM-assisted coding and Ghidra-based reverse engineering.
- **Cross-title loading isn't three games.** demo.2 claims tested loading of an MW2, an MW3 and a Black Ops map with bots under MW2 rules. That's map and content support.
- **Protocol versions move.** Read the network docs for your exact revision.
- **Later builds.** This page covers the pinned demo build only. Anything added after it isn't covered here, so check the project's current README before relying on it.
- Other projects build on it, among them the [2010 mashup](2010-rust-rewrite-mashup.md) and [Halo / MW2 Director](halo-mw2-director.md).

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

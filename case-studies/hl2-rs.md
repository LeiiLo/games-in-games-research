# HL2-RS: Half-Life 2 rebuilt in Rust

**Repository:** [kvalls/hl2-rs@b4b1731](https://github.com/kvalls/hl2-rs/tree/b4b1731530a9ca146f9d45532f00ef7f9fc69e7d) (default branch, 6 Oct 2026)  
**Route:** engine recreation (standalone)  
**What we read:** all first-party Rust files. Nothing was run.  
**AI credit (as the project states it):** README credits Codex.

Rebuilds parts of Half-Life 2 in Rust (Macroquad, Rapier), loading Source assets. It isn't a mashup, but its fixes apply to any project that shares a renderer or drives input automatically.

**What we learned**

- Depth test and depth write are separate switches; transparent surfaces often need the first without the second.
- GPU state leaks between passes: a previous pipeline can stop a depth clear from working. Set what you need, then restore it.
- Synthetic Windows input lacks raw mouse motion, so it falls back to ordinary mouse deltas when raw input is absent.
- A loaded level isn't campaign parity: scene and NPC tests are narrow, and weapon spread and damage are approximations.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

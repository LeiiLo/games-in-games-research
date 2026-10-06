# chasm: AI-driven NPCs with a Fallout: New Vegas bridge

**Repository:** [chasmlol/chasm@d740b97](https://github.com/chasmlol/chasm/tree/d740b9742869e436576d93180f7b5886afdfa9db) (default branch, 6 Oct 2026)  
**Route:** runtime AI middleware (not a game merger)  
**What we read:** source snapshots, bridge and desktop docs. Nothing was run.  
**AI credit (as the project states it):** commits co-authored by Claude Opus.

A Rust backend with a React interface and a Tauri desktop wrapper. Unlike the bridges elsewhere in this repo, which run ordinary deterministic code, chasm calls language models during play for conversation, memories, intentions and actions.

**What we learned**

- **The bridge contract.** Stable actor identities, nearby actors, witnessed events, state, actions and save checkpoints. Streaming turns carry text, audio and requested actions, tied together by correlation IDs and chunk indexes.
- **Two delivery paths, one rule.** HTTP actions are executed once by the caller; file-bridge actions use a queue. Confusing them double-executes actions. Keep one inbox reader.
- **Memories follow saves.** Save IDs and game fingerprints checkpoint and roll back NPC memories so branches don't mix.
- **Where latency went.** Most of the improvement came from replacing a 750 ms poll with notifications plus a fallback. Speech and model calls dominate the rest.
- **Golden-file protocol replay**, and waiting for partial WAV files to settle, prevent duplicate processing.
- An action being available isn't proof it ran.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

# NewVegasCraft: Minecraft beside Fallout: New Vegas

**Repository:** [Davozh/new-vegascraft@ad00a38](https://github.com/Davozh/new-vegascraft/tree/ad00a384e81009f9bc877ee7e7f4dfa100db353b) (default branch, 6 Oct 2026)  
**Route:** frame compositing plus state  
**What we read:** about 20 commit pages and docs. Nothing was run.  
**AI credit (as the project states it):** commit pages credit Claude Opus 5.5.

An xNVSE plugin and a ReShade compositor bring Minecraft into Fallout: New Vegas (Steam 1.4.0.525), with a Proton setup. Its commit pages record a long diagnostic sequence, wrong turns included, which is why it's worth reading.

| Problem | Where it ended up |
|---|---|
| crash on the first native collision ray | a 4-byte-aligned stack met 16-byte SSE loads; aligned storage fixed it |
| host depth read empty | scene depth was a 4× MSAA surface; an earlier depth-format theory was wrong and removed; MSAA had to be off |
| shaders failed under Proton | a native 32-bit shader compiler DLL in that setup |
| the image shook | the host pose is now read at present time, not in the main loop |
| uploads stalled | dynamic textures; the creator reports 18.7 ms → about 5 ms per upload |
| the world seemed to drift | a field-of-view control was removed; the remaining "drift" was a real block intersecting a sign |

**What we learned**

- Build measurement tools first: a key that cycles composite, host depth, guest depth and difference; a crosshair marker pillar; a projection dump; capture bursts; a pose ring for lag 0/1/2.
- Remove failed fixes instead of keeping every theory.
- A fake host that compares each exported frame with the pose recorded for it catches misalignment early.
- Units: 70 host units per block; the exterior height offset was fixed at −34 after per-load re-levelling moved builds by about 0.9 m.
- Collision moved from a thin barrier skin to solid columns up to the highest surface, capped near 24 blocks, which also fills arches.
- Early versions sent WebSocket messages synchronously from the game loop; use a sender thread with a bounded queue.
- Host actors walk through guest blocks at the version we read; the reverse collision path wasn't built.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

# ER Mario: Mario 64 movement inside Elden Ring

**Repository:** [deltarooo/er-mario@83d1397](https://github.com/deltarooo/er-mario/tree/83d1397b5377c88a0fcdb00717e84ce068798ceb) (v0.3.8)  
**Route:** embedded rebuilt mechanic (in-process)  
**What we read:** source README, binary README and the collision test notes. Nothing was run.

A Rust DLL loaded through me3 with the C library [libsm64](libsm64.md) compiled in. Elden Ring's live Havok collision is adapted for Mario's movement and animation. It's a reusable component inside the host, not two games running side by side.

**What we learned**

- **Keep the host's systems alive.** A hidden native Tarnished follows Mario, so Elden Ring still handles doors, menus, quests, deaths and saves. Mario uses a separate offline save.
- **Collision notes worth reading.** Mirrored coordinate systems and triangle winding, convex hull orientation, moving-platform displacement, sequential wall correction, anchoring ceiling queries, and removing stale invisible guard walls.
- **Synthetic tests have limits.** Its tests cover translations, rotations, stream changes and rebasing. They don't cover every Havok situation.
- **Read asset claims precisely.** The player supplies a US Mario 64 ROM for textures, audio, animations and icons. The source build derives some model geometry from decompilation files, and the release contains that mesh.
- **Known limits in v0.3.8:** elevators, cutscene appearance, some mounts and springs, throw and ragdoll aborts, shadows and controller reconnects. Update checks notify; they don't install.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

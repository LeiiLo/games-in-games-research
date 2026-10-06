# CrossOver bridges: Minecraft in Elden Ring and Monster Hunter: World on macOS

**Repository:** [justbustin/minecraft-crossover-bridge@d172387](https://github.com/justbustin/minecraft-crossover-bridge/tree/d1723873ec8f370389cf19f74fac455b9e581321) (default branch, 6 Oct 2026)  
**Route:** frame compositing plus state  
**What we read:** parts of the source (the native adapters were not read). Nothing was run.

One creator built two bridges for Minecraft 1.21.1 (Fabric). Minecraft runs natively on macOS, the host runs in CrossOver, and a file-backed shared-memory mapping is visible to both.

| | Monster Hunter: World | Elden Ring |
|---|---|---|
| Host version | 15.23.00 (build 421810) | app 1.17.1 (exe 2.7.1.0) |
| Graphics path | a Direct3D-on-Metal layer, D3D11 | D3DMetal, D3D12 |
| Layers sent | world, depth, hand and HUD together | world, depth, HUD, separate hand |
| Lighting | world multiplied by blurred host brightness | ambient and haze from two downsampled passes, plus a tint |
| "Damage" means | native HP | Minecraft damage scaled by the target's max HP |

**What we learned**

- **Same magic, different meaning.** The two protocols share magic and version but differ in header size, regions, units and damage semantics. They live in separate folders for that reason.
- **Pose matching with fallbacks you can count.** The host keeps eight poses, prefers the exact frame, then an older slot, then the last upload, and logs the counts every 300 presents.
- **A size ceiling.** Above 1920×1200 they fall back to a transparent overlay window with no depth occlusion.
- **Read your sync code against its comments.** In one host the frame writer's counter goes even to even, so it never marks a write in progress; the other host does it correctly. Nobody showed a visible glitch from it.
- **Validate, then upload.** One host uploads before its final sequence check; the other validates the CPU copy first.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

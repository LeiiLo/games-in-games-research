# WiiCompiled: Mario Kart Wii by static recompilation

**Repository:** [patchzyy/Wiicompiled@82991ec](https://github.com/patchzyy/Wiicompiled/tree/82991ec33745950273f64afa7fb52c7cd4e7555f) (v0.2.33)  
**Route:** static recompilation  
**What we read:** main and v0.2.33 source archives and docs. Nothing was run.

A .NET translator decodes PowerPC DOL and REL code into an intermediate form and emits C++, which is compiled with a compatibility runtime. Graphics go through Aurora/GX and WebGPU/Dawn. There's no runtime PowerPC interpreter or JIT. A clean PAL Mario Kart Wii disc is the supported input.

**What we learned**

- Translating another game's executable doesn't give you its graphics, audio or input. Those are runtime work.
- A popular mod pack gets its own static profile: its payload code is translated alongside the base game.
- 120/144 Hz interpolation changes presentation, not the physics tick, and can add artefacts.
- Ghost-input replay is a good parity test for the routes it covers; it doesn't prove "100% physics".
- Translator tests default to synthetic inputs; asset and host-compiler tests are off by default. Manifests require exact binary hashes. Unsupported instructions fail unless you opt into traps.
- The release history lists concrete fixes (memory corruption, underflow, black frames, resize, timers, input). The runtime assets README lists bundled DSP coefficients and network bootstrap material, and the project's third-party notices say the DSP file is a free replacement ROM written by the Dolphin team, not Nintendo data.
- The Mac renderer shares IOSurfaces through Dawn. Avoiding CPU copies still leaves GPU blit and scheduling cost, and a skipped test isn't a pass.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

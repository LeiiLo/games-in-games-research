# MKX Character Studio: conversion that the game still rejects

**Repository:** [Giraffebutt/MKX-Character-Studio@ec69899](https://github.com/Giraffebutt/MKX-Character-Studio/tree/ec69899711d1f2c803434b7b3a51160c9223d8c6) (default branch, 6 Oct 2026)  
**Route:** asset tool (adjacent: not a game merger)  
**What we read:** README and commit titles. Nothing was run.  
**AI credit (as the project states it):** the README says it was built with AI from start to finish under the author's direction; later commits are co-authored by Claude.

A Python tool that exports Mortal Kombat X character packages to GLB and converts edited GLB files back. It keeps the original skeleton and allows four bone weights per vertex, the first level of detail only, up to 65,535 vertices and embedded textures only.

**What we learned:** The author's checks read the converted packages back correctly across 232 packages, but the stock game rejects modified packages. A file that converts and reads back isn't a mod that loads; test in the game before calling it working.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

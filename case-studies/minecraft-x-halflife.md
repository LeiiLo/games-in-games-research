# Minecraft × Half-Life (GoldSrc)

**Repository:** [SawyerTheNerd/Minecraft-X-HalfLife@fecf40c](https://github.com/SawyerTheNerd/Minecraft-X-HalfLife/tree/fecf40cfbbdc22b30e7a4f31d8d221923d546532) (default branch, 6 Oct 2026)  
**Route:** geometry transfer  
**What we read:** partial: README, task list, several source files. Nothing was run.  
**AI credit (as the project states it):** a commit co-authored by Claude Opus 5.5.

A hidden real Minecraft supplies movement, physics and meshes. Modified Half-Life client and server DLLs draw the Minecraft geometry through Half-Life's own OpenGL pipeline; only the HUD is a pasted image.

**What we learned**

- **Units:** 40 Half-Life units per block, so 72 units is about 1.8 m. Health maps 20 → 100, a factor of 5.
- **Collision:** BSP faces and the player-clip hull become Minecraft collision, with the hull expansion removed first. Moving brushes send updates; old and new regions are both marked dirty and stale worker jobs are cancelled by epoch.
- **Hand-back:** ladders, `use`, noclip and death return movement to Half-Life. Crouch heights don't line up exactly.
- **Two things to avoid:** the shared mapping overwrites an existing layout on magic or version mismatch instead of rejecting it, and native entities are referred to by index without a generation tag, so a late event could reach a recycled entity.
- **Pin your downloads.** Its tools fetch Java from a "latest" endpoint.

Five large files (renderer, client, BSP converter and two patches) weren't visible in what we read. The project's own task list keeps arms, hazards, lighting, mob paths and shared death as not yet accepted in game.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

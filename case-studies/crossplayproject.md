# CrossplayProject: Minecraft and Roblox linked

**Repository:** [Atmerek/CrossplayProject@1f843f0](https://github.com/Atmerek/CrossplayProject/tree/1f843f0117f41934b6a54adae3264dd46e615e17) (default branch, 6 Oct 2026; archived)  
**Route:** server-to-game state bridge  
**What we read:** READMEs for releases 1.0 to 1.3 and the main branch. Nothing was run.

A Minecraft server plugin exposes blocks, players, mobs, weather and chat over an HTTP API. A Roblox experience reads it, clones block models, and sends building, breaking, chat and later NPC actions back. Neither game's client runs inside the other.

**What we learned**

- **It predates the current wave.** Its releases date from 2024 and the repository was archived in 2025. Don't assume a game bridge is recent or AI-made because it looks like recent ones.
- **The API changed shape between releases.** 1.3 shortened block fields and switched to bounded areas, so clients and servers have to match versions. Version the API, not only the plugin.
- **Two-way presence came late.** Only 1.3 sends Roblox players back into Minecraft as NPC stand-ins. Check which direction a "crossplay" claim covers.
- **README examples aren't schemas.** Some example payloads in the docs aren't valid JSON. Treat examples as illustrations and check the code.
- **Read the height range.** The block API covered a fixed vertical range; check it against the world you're using.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

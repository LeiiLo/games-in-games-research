# hytale2mc: one minigame framework for Minecraft and Hytale

**Repository:** [alskea/hytale2mc@db8b444](https://github.com/alskea/hytale2mc/tree/db8b44415245244fedf97e2de044a2307d2522f1) (default branch, 6 Oct 2026)  
**Route:** shared simulation (adjacent)  
**What we read:** README (February 2026). Nothing was run.

A Kotlin entity-component framework that runs the same minigame on a Minestom (Minecraft) server and a Hytale server. NATS keeps the two in sync, and JetStream stores replays. Its examples are an aim trainer and Quake-inspired games.

**What we learned:** This route writes new game logic once and gives each platform its own spawn and render code; it doesn't translate an existing game. Both platforms need a compatible way to show the same state. The README says the framework is incomplete, and a "Quake" showcase is a game in that style, not a port.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

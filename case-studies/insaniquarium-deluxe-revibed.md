# Insaniquarium Deluxe Revibed: a Rust rebuild with layers of evidence

**Repository:** ITSTDMCC/Insaniquarium-Deluxe-Revibed@276648b (default branch, 6 Oct 2026). Not linked: it may contain the original game's code.  
**Route:** engine recreation (adjacent: one game, no host)  
**What we read:** READMEs for 1.0, 1.1 and the default branch, and commit titles. Nothing was run.  
**AI credit (as the project states it):** the README says Claude Opus 5.5 wrote most of it; commits are co-authored by Claude.

A standalone Rust and Bevy port of Insaniquarium Deluxe that rebuilds the game's PopCap framework and reads art and sound from the player's own Steam copy. Tools generate class tables and a coverage manifest from a reference SQLite database.

**What we learned**

- **Scripted frames aren't play.** Fixed-step frame scripts, headless screenshots and a setup cheat let the agent test levels quickly. They show that a scripted route runs, not that every rule matches the original.
- **Separate the AI uses.** Optional Real-ESRGAN upscaled art is a different use of AI from writing the code, and nicer images say nothing about logic fidelity.
- **Timing comes from the original.** One commit changes the logic clock from 10 ms to the original's 28 ms frame time.
- **Writes into the owned game.** Saves and generated HD art go into the game's own folders, so the README suggests copying the folder first or turning saving off.
- **Agents can build without launching.** One commit title says the agent builds the game but doesn't run it; someone still has to play it.
- **Hardware figures are claims.** The memory and disk numbers are the creator's, not benchmarks.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

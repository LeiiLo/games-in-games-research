# ccboy: a Game Boy on ComputerCraft monitors

**Repository:** [amatheo/ccboy@1e0fe92](https://github.com/amatheo/ccboy/tree/1e0fe92c174b75acfd096298f87b37e4d3a8e660) (default branch, 6 Oct 2026)  
**Route:** external emulator streamed to an in-game display  
**What we read:** README (September 2025). Nothing was run.

A Python server runs the PyBoy emulator headless, reduces each frame to ComputerCraft's colour palette, compresses it with zlib and sends it over a WebSocket to a Lua program on a CC:Tweaked monitor in Minecraft. A second monitor works as a touch controller and sends button presses back. The player supplies the ROM.

**What we learned**

- **Simulation rate and display rate differ.** The defaults emulate at 60 Hz and update the screen at 15 Hz, so the game runs at full speed but doesn't show 60 frames a second. Say which rate a claim means.
- **The host's scripting mod is the transport.** ComputerCraft's HTTP and WebSocket support carries frames in and buttons out; nothing else in Minecraft changes.
- **Check toggles against the code.** A compression setting exists, but the current build always compresses.
- **Setup docs can drift.** The Docker setup refers to a requirements file that isn't in the repository.
- **No audio.** The project doesn't claim sound.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

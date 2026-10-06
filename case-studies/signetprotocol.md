# Signet: a shared match instead of a bridge

**Repository:** [kian-cx/signetprotocol@2ddb136](https://github.com/kian-cx/signetprotocol/tree/2ddb136ee941705d3e1c020eaddad93be65026f2) (default branch, 6 Oct 2026)  
**Route:** shared neutral simulation  
**What we read:** SDK core, C interface, server and bindings. Nothing was run.

Signet runs one neutral simulation on a server, and each game acts as a viewer. Its Doom and OpenArena viewers are reimplementations; Minecraft joins through a gateway.

**What we learned**

- The SDK predicts movement at a fixed 20 Hz, replays unacknowledged commands when the server corrects it, and logs corrections over 5 cm.
- The shared rules are simple on purpose: movement is 2.5D, so stacked floors collapse into one column, and shooting is horizontal. Doom-looking walls don't bring Doom physics.
- Hazards need explicit mapping: the Doom and OpenArena importers set the neutral water flag false even for water, lava and toxic materials.
- The C interface consumed events during a null-buffer sizing call, so "ask the size, then copy" lost them.
- The client writes TCP while holding a mutex, and the server broadcasts while holding shared state, although the docs say the client never blocks.
- The planned AI translator ("Forge") is described in the project as not implemented.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

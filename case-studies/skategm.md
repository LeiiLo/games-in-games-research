# SkateGM: the rebuilt Skate engine inside Garry's Mod

**Repository:** [the-schwilliam/SkateGM@d392028](https://github.com/the-schwilliam/SkateGM/tree/d392028e8581c74e003d5966f6826c144791f0bf) (default branch, 6 Oct 2026)  
**Route:** rebuilt guest engine inside a host scripting runtime  
**What we read:** captured commit pages, some bodies hidden. Nothing was run.  
**AI credit (as the project states it):** the README says AI coding tools helped write the code.

Runs the rebuilt Skate engine inside Garry's Mod through a native module and Lua, with the player's own Skate 3 data.

**What we learned**

- **Normalise input at the boundary.** SDL controllers (PlayStation, Switch, generic) are converted to the Xbox-shaped input structure the engine already used; button labels are picked separately. The engine's input code didn't change. A connected XInput pad takes priority over SDL.
- **Moving props need their own collision layer.**
- **Skip what isn't ported, visibly.** An unported air-dismount producer is skipped rather than failing the tick.
- **Pin downloads with hashes.** Its fetch script pins SDL2 2.32.10 and a mapping-database commit and checks SHA-256.
- **Make checks fail loudly.** Its disc-image fixture and several Lua checkers print failures without a failing exit code.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

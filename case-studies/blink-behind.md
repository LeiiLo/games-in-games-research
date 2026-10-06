# Blink Behind for Dragon's Dogma 2

**Repository:** [ntgoten/blink-behind@1a4825c](https://github.com/ntgoten/blink-behind/tree/1a4825ccbb941e539d67df5da855a2df6a7cc093) (default branch, 6 Oct 2026)  
**Route:** same-game ability mod (REFramework Lua)  
**What we read:** README and field note. Nothing was run.  
**AI credit (as the project states it):** commits co-authored by Claude.

Replaces a Fighter skill with a short-delay teleport behind the target, borrowing effects and sounds from another skill in the same game.

**What we learned** (from the project and its Universal Modder field note): retain managed arguments, avoid pointer truncation, use terrain spherecasts to find a safe spot, restore shared parameters, and finish effect instances explicitly. Working tests on two vocations don't cover every target, controller or build.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

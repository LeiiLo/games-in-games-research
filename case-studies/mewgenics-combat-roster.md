# Mewgenics combat roster panel

**Repository:** [TotSamiyMorzh/mewgenics-combat-roster@4cf4233](https://github.com/TotSamiyMorzh/mewgenics-combat-roster/tree/4cf423376df555ebb0a1c556e8aa06d57fe3fe97) (default branch, 6 Oct 2026)  
**Route:** native utility mod (proxy DLL, SDL/OpenGL hooks, ImGui)  
**What we read:** README and field note. Nothing was run.  
**AI credit (as the project states it):** commits co-authored by Claude.

Adds a combat roster with health, shields, mana, statuses and tooltips.

**What we learned:** recognise the real engine stack (here SWF, GON and SDL) before choosing tools; stray strings suggested the wrong engine. Hook the actual SDL dispatch slot, handle the GL context being replaced, and avoid a game function that looks like "highlight" but changes game state.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

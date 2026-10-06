# Touhou HFR: high frame rate and timing patch

**Repository:** [vittorioromeo/th12_hfr@895dbe9](https://github.com/vittorioromeo/th12_hfr/tree/895dbe9dd1415ca1d81bc15efa831a16117901eb) (v0.11)  
**Route:** native timing and graphics patch (adjacent: not a mashup)  
**What we read:** docs for selected release roots. Nothing was run.  
**AI credit (as the project states it):** README says ChatGPT helped with the original patch and Claude from the scaler onward; commits name the co-author.

Proxy DLLs that change timing and rendering for several Touhou games.

**What we learned**

- **Changed rules need a separate leaderboard.** Smaller simulation steps can change hits and scores, so the project's own docs say runs aren't comparable with the stock game. Full-run replay parity was unverified.
- **Check the loaded code.** Steam versions must be launched through Steam, and the patch checks the instructions it expects.
- **Agree responsibilities with other mods.** With a popular rotation wrapper, the wrapper owns render targets, rotation and presentation; HFR owns timing, input and replays and turns its own scaling off. HFR re-takes its graphics imports after a translation patch loads, then chains them.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.

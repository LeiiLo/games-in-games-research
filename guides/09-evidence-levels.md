# 09. Evidence levels

"X works" can rest on a creator's video, a README, code someone read, a test that passed, or a real play session. Keep them apart in your notes and in what you tell people. This applies to your own mod as much as to someone else's.

## Label each claim

| Level | Means | How to write it |
|---|---|---|
| **Creator report** | someone says it works (README, post, video, commit message) | "the creator reports…" |
| **Design or proposal** | planned, drafted or described as future work | "planned", "proposed" |
| **Source reading** | code or docs were read; nothing ran | "the code does…" |
| **Comparison** | a diff between versions or forks | "compared with upstream X…" |
| **Synthetic test** | a test, fake host or fake guest ran | "passes against a fake host" |
| **Real run** | the real game ran this build and someone looked | "verified in game on <version, OS, GPU>" |

Only the last row supports "working" for your own work, and only for the scenarios you played.

## Things that look like proof and aren't

- **A design document.** [Garry's Redemption](../case-studies/garrys-redemption.md)'s first commit plans in-frame Vulkan/DX12 compositing. Its later release ships a separate overlay window and calls in-frame drawing unbuilt.
- **A test that checks little.** The GTA V example's `ws_test.cpp` passes if any received message contains the word `explosion`. It doesn't check camera accuracy, depth alignment or reconnecting. Elsewhere, a save-verification check once passed with no game data present at all.
- **A synthetic peer.** [LibertyCraft](../case-studies/libertycraft.md) ships stand-ins for both ends of its bridge. Passing one still needs both real games.
- **A percentage badge.** [AnyPS5](../case-studies/anyps5.md) reports progress against the system functions it knows about; the denominator grows as more are found. That isn't game compatibility. Decompilation directories list matching, linking and other percentages that mean different things.
- **A clean install on the author's PC.** One machine isn't two. Several projects report a single tested PC.
- **A test suite that can't fail.** [benilla](../case-studies/benilla.md) and [World of Skatecraft](../case-studies/world-of-skatecraft.md)'s history records fixing tests whose names promised more than they checked, and screenshot sweeps that were "green" without checking anything. It later made a skipped test fail instead of pass.
- **A count of checks.** The [Diablo II movement mod](../case-studies/devilutionx-d2-movement.md) reports 65/65 checks, but they measure different things (table agreement, travel time, level hashes, best-of-several frame rate), and the owned-file checks skip when files are missing.
- **Changed rules, same leaderboard.** [Touhou HFR](../case-studies/th12-hfr.md)'s smaller simulation steps can change hits and scores, so its own docs say runs aren't comparable with the stock game.
- **Docs stronger than code.** [Signet](../case-studies/signetprotocol.md)'s docs say the client never blocks and that authority prevents cheating. The code we read writes TCP under a lock, and its roadmap lists command-rate checks as pending. Believe the roadmap.
- **A main-menu result.** One investigation found the host's depth buffer flat in the main menu and stopped. That says nothing about depth during gameplay. Record negative results with their exact scope; they're useful.
- **An edited video.** Cuts can hide stale frames. Sampled frames can miss short events. Automatic captions misname games and people.

## Lineage and credit

- **Record the exact upstream commit a fork started from.** LibertyCraft's first commit says it forks [SkyCraft](../case-studies/skycraft.md)'s Fabric mod and protocol header, but the commit has no parent, so the exact base can't be recovered.
- **A new file name isn't new code.** Relocated upstream helpers are still upstream work.
- **A fork's history carries the upstream's credits.** [FalloutCraft](../case-studies/falloutcraft.md)'s history includes SkyCraft commits co-authored with an AI model. Those don't describe the later Fallout-specific changes.
- **AI credit belongs to the work its author credits.** Many projects credit a model in commits or READMEs. Long-running human research underneath (decompilation projects, extraction tools, prior engines) stays human work. Missing credit isn't evidence either way.

## In your own write-up

- Your status line describes your own verified state. Quote other people's claims as theirs.
- Say what ran, on which versions, OS and GPU, and what didn't.
- Keep corrected diagnoses. LibertyCraft's history first blames one cause for a login loop, then removes a different add-on as the real cause. The correction is the useful part.
- Count sources, not copies. Several reposts of one creator's video aren't independent.
- Hashing, unzipping or listing archive members isn't reading the code.
- Keep creator measurements attributed ("the creator reports 18.7 → 5 ms") even when it's convenient to drop the attribution.

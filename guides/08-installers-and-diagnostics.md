# 08. Installers and diagnostics

Mashups ship several moving parts: a host plugin, a loader, a guest mod, sometimes a worker process or a converted asset set. The install script is where users lose settings or end up with half an install, and "it doesn't load" is where most support time goes.

## Install patterns that worked

1. **Check the code that's loaded, not the file on disk.** Steam-wrapped executables are decrypted in memory, so the file doesn't show the real code. [BullySkate](../case-studies/bullyskate.md) launches Bully through Steam, and its native plugin requires all 203 code fingerprints and 19 data locations to match before it installs any hooks. [Touhou HFR](../case-studies/th12-hfr.md) requires Steam versions to be launched through Steam and checks the instructions it expects.
2. **Load early enough.** [PipeLink](../case-studies/pipelinklauncher.md) found that a `dinput8.dll` proxy loaded too late for GTA San Andreas's mod loader and its own plugin. It installed the same loader under the name of a DLL the game imports at startup and kept the original under a new name.
3. **Pin and verify downloads.** [SkateGM](../case-studies/skategm.md)'s fetch script pins SDL2 2.32.10 and a commit of a controller-mapping database, and checks SHA-256. Fetching Java from a "latest" endpoint, pinning versions without hashes, or pulling shader headers from a moving branch all leave you with an install you can't reproduce. A download "stamp" isn't a content digest.
4. **Record what you created and what you replaced.** BullySkate validates prepared asset sets by hash, keeps timestamped backups and refuses an unknown existing loader. The GTA V example's `--remove` deletes `ReShade.ini` even if the user had one before installing. Keep a manifest so uninstall can be exact.
5. **Replace loaded DLLs safely.** [NewVegasCraft](../case-studies/new-vegascraft.md) switched to copy-then-rename so a DLL that's already mapped isn't rewritten in place.
6. **Agree responsibilities with other mods.** Touhou HFR and a popular rotation wrapper split the work: the wrapper owns render targets, rotation and presentation, HFR owns timing, input and replays and turns its own scaling off. HFR re-takes its graphics imports after a translation patch loads, and chains them, after an older version had them silently removed. Document the exact combinations you tested.
7. **Treat "installed", "loaded", "connected" and "plays" as four checks.** A clean install isn't gameplay.

## Install mistakes

1. **Name checks pass on the wrong build.** [ReSkate Trainer](../case-studies/reskate-trainer.md)'s batch installers check file or process names only. Hash or fingerprint the target.
2. **Half an install.** BullySkate, ReSkate Trainer and SkateGM all copy in steps. Stage to a temporary folder, verify, then swap, and detect a partial earlier run on start.
3. **A backup marker that lies.** ReSkate Trainer uses the backup DLL as its only "backup done" marker and copies the DLL and launcher in separate steps. Write the marker last, after every file is verified.
4. **Version strings compared as decimals.** BullySkate's loader check compares versions as decimals, so 15.10 reads as 15.1. Compare components as integers.
5. **Duplicate loaders.** PipeLink returns early when a loader exists, before it cleans up obsolete copies. List every loader before deciding.
6. **Checks that print instead of fail.** SkateGM's disc-image fixture and several Lua checkers print failures with a success exit code. Make every check affect the exit code.
7. **A default build that skips the meaningful tests.** Asset-dependent smoke tests skip silently when owned files are missing. Report skipped checks as skipped.
8. **Installers that clobber shared config.** The GTA V example's save helper copies its chosen saves into every discovered profile. A mod that changes and saves the player's options, or resets inventory on join, needs its own launcher profile.

Aim for a launcher that records hashes and backups and never touches the guest game's saves. Some two-game launchers we looked at did exactly that while their bridges still had rough edges, and the careful installer made those edges much easier to report.

## Diagnostics worth building early

- **A fake host and a fake guest.** The GTA V example's Python host and synthetic D3D11 fixture, and [LibertyCraft](../case-studies/libertycraft.md)'s stand-ins for both ends of its bridge, catch transport and alignment bugs before a multi-gigabyte game is involved. Passing them still needs both real games afterwards.
- **Debug views on keys.** See [guide 03](03-frame-compositing.md) for NewVegasCraft's set.
- **Logs on both sides with a shared frame counter.**
- **Liveness checks for capture.** Identical capture hashes during an animation can reveal a stale capture. A scene that's legitimately still needs some other liveness signal.
- **A continuous raw recording next to the edited showcase.** The GTA V example's author says the demo cuts around stale-overlay frames. Edited footage doesn't show uninterrupted play.

## Agent-driven testing

[Universal Modder](../case-studies/universal-modder.md)'s Terraria reference bridge queues localhost JSON-lines requests onto the game's main thread after each update, exposes menus, player state, inventory and nearby enemies, and finds UI click handlers instead of using screen coordinates. Two cautions from it: its "step" sets controls for upcoming frames and observes the frame that already finished, so it doesn't advance exactly one tick, and held controls need a disconnect watchdog. Its recorder runs ffmpeg window capture outside the game because reading the backbuffer inside the game leaked memory.

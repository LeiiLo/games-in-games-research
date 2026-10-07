# Case studies

One page per project we learned from. Each page links the repository at the commit or tag we looked at, names the route, says how much we read, and lists what we took away. Nothing on these pages was run, and nothing is copied from the projects.

## Two real games running together

- [SkyCraft: Minecraft inside Skyrim](skycraft.md)
- [LibertyCraft: SkyCraft ported to GTA IV](libertycraft.md)
- [ValCraft: Minecraft inside Valheim](valcraft.md)
- [Killcraft: Minecraft inside ULTRAKILL](killcraft.md)
- [FalloutCraft: Minecraft inside Fallout 4](falloutcraft.md)
- [Universal Modder: worked examples and field notes](universal-modder.md)
- [Wither Storm × GTA V](wither-storm-gta5-passthrough.md)
- [CrossOver bridges: Minecraft in Elden Ring and Monster Hunter: World on macOS](minecraft-crossover-bridge.md)
- [NewVegasCraft: Minecraft beside Fallout: New Vegas](new-vegascraft.md)
- [Minecraft × Half-Life (GoldSrc)](minecraft-x-halflife.md)
- [GalaxyCraft: Minecraft inside Super Mario Galaxy 2 (Dolphin)](galaxycraft.md)
- [Garry's Redemption: Garry's Mod beside Red Dead Redemption 2](garrys-redemption.md)
- [OWCraft: Minecraft inside Outer Wilds](owcraft.md)
- [SubCraft: Minecraft inside Subnautica](subcraft.md)
- [CrossplayProject: Minecraft and Roblox linked](crossplayproject.md)
- [Project Inception: Minecraft inside Minecraft](projectinception.md)

## Shared simulation

- [Signet: a shared match instead of a bridge](signetprotocol.md)
- [hytale2mc: one minigame framework for Minecraft and Hytale](hytale2mc.md)

## Rebuilt engines and mechanics inside a host

- [ER Mario: Mario 64 movement inside Elden Ring](er-mario.md)
- [libsm64: Mario 64 movement as a library](libsm64.md)
- [BullySkate: the rebuilt Skate engine inside Bully](bullyskate.md)
- [SkateGM: the rebuilt Skate engine inside Garry's Mod](skategm.md)
- [World of Skatecraft: the Skate engine inside a recreated WoW client](world-of-skatecraft.md)
- [Faith Runner: Mirror's Edge movement in Skyrim and Minecraft](faith-runner.md)
- [AC1 Movement Rewritten](ac1-movement-rewritten.md)
- [Diablo II Movement for DevilutionX](devilutionx-d2-movement.md)
- [DevilutionX: a rebuilt Diablo engine](devilutionx.md)
- [Minebonk: Minecraft mechanics rebuilt inside Megabonk](minebonk.md)

## Engine recreations

- [SK8-ENGINE: a rebuilt Skate 3 runtime](skate-3-rust-engine.md)
- [IW4L: a Modern Warfare 2 runtime in Rust](iw4l.md)
- [2010 Rust Rewrite Mashup: MW2, Skate and Minecraft in one runtime](2010-rust-rewrite-mashup.md)
- [CS:Craft: Counter-Strike movement in a Minecraft survival mode](cs-craft.md)
- [benilla: a recreated World of Warcraft 1.12.1 client](benilla.md)
- [HL2-RS: Half-Life 2 rebuilt in Rust](hl2-rs.md)
- [Halo / MW2 Director: three modes in one engine](halo-mw2-director.md)
- [Insaniquarium Deluxe Revibed: a Rust rebuild, and what its checks prove](insaniquarium-deluxe-revibed.md)
- [gang-beasts-rust: a Rust rebuild that keeps game files out](gang-beasts-rust.md)

## Recompilation and decompilation

- [WiiCompiled: Mario Kart Wii by static recompilation](wiicompiled.md)
- [WheelWizard: a mod manager for Mario Kart Wii](wheelwizard.md)
- [Mario Kart: Double Dash matching decompilation](mkdd.md)
- [AnyPS5: porting console executables by relinking](anyps5.md)
- [gamedb: a SQLite index of decompiled source](gamedb.md)

## Installers, patches and same-game mods

- [PipeLink: installer and converters around GTA San Andreas, Skate and MW2](pipelinklauncher.md)
- [Touhou HFR: high frame rate and timing patch](th12-hfr.md)
- [ReSkate Trainer](reskate-trainer.md)
- [Blink Behind for Dragon's Dogma 2](blink-behind.md)
- [Mewgenics combat roster panel](mewgenics-combat-roster.md)

## Adjacent: not a game merger

- [chasm: AI-driven NPCs with a Fallout: New Vegas bridge](chasm.md)
- [HyCraft: Minecraft clients on a Hytale server](hycraft.md)
- [wasmcraft2: programs compiled into Minecraft commands](wasmcraft2.md)
- [DoomMaps: Doom on Hytale's world map](doommaps.md)
- [ccboy: a Game Boy on ComputerCraft monitors](ccboy.md)
- [MKX Character Studio: conversion that the game still rejects](mkx-character-studio.md)

## Lessons without a named source

Some lessons came from projects or posts we can't name, because their authors didn't give an open licence or say the work was AI-assisted. We kept the lessons, in our own words and without details that would identify where they came from. We didn't discover these; other people did.

- **Check the install README before guessing the architecture.** One two-game mod looked like a rebuilt guest engine inside the host in clips. Its install instructions required a separate copy of the guest game, which made it a live bridge. Guesses from footage are often wrong.
- **Installers can be careful even when the bridge is rough.** Some two-game launchers record hashes and backups, never touch the guest's saves, and provide a recovery key. Users still reported floating characters, missing overlays and version mismatches, but the careful installer made those reports useful.
- **A rebuilt engine running as a library inside a host process** keeps the last valid transform when the guest produces invalid values, stops after repeated errors in a short window, and can recover by reloading the guest. Its default build skips asset-dependent smoke tests and reuses an existing library without checking its revision.
- **Budget collision rebuilds for vehicles.** Some projects watch nearby cars inside a radius, rebuild only after a set distance of movement and at most once a second, and skip identical triangle batches by hash. Identical order is caught; geometric equivalence isn't.
- **Map conversion between engines** turns one game's levels into another's: geometry, materials, collision and spawns. A converted level can load and still be impossible to finish (a hidden door that doesn't open is a classic). Play it through.
- **A test that passes with no data.** One project's changelog records a save-verification check that passed with no game data present. Make missing inputs fail.
- **Footage misleads.** A widely shared "every game in one" video was a Garry's Mod addon montage, and some viewers took it for an engine rebuild. Several recent videos describe two-game bridges as AI translating frames during play; no project we read works that way.
- **Comments and posts aren't implementation evidence.** Claims like "two full games merged in 12 hours" had no implementation behind them. Keep a partial experiment and someone else's bigger claim apart.
- **Old decompilation projects without a licence** remain important foundations for later work. If you build on one, record that, and know that its legal status is its own question.

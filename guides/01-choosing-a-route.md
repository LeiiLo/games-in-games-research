# 01. Choosing a route

"Put X in Y" names at least eight different architectures. They can look identical in a 30-second clip. Pick one on purpose, before the first line of bridge code, and pick again if your plan drifts (a "passthrough" that has turned into an overlay is the usual drift).

## The routes

| Route | What runs | What crosses between them | What each player needs | Case studies |
|---|---|---|---|---|
| **Live state bridge** ("passthrough") | both real games | positions, collision, hits, events | both games and both mod loaders | [SkyCraft](../case-studies/skycraft.md), [LibertyCraft](../case-studies/libertycraft.md), [ValCraft](../case-studies/valcraft.md), [Killcraft](../case-studies/killcraft.md) |
| **Frame compositing** | both real games | the guest's colour, depth and HUD images plus the camera pose they were rendered with | both games | [Minecraft × GTA V](../case-studies/universal-modder.md), [CrossOver bridges](../case-studies/minecraft-crossover-bridge.md), [NewVegasCraft](../case-studies/new-vegascraft.md), [Wither Storm × GTA V](../case-studies/wither-storm-gta5-passthrough.md) |
| **Geometry transfer** | both real games; the host draws guest meshes | meshes, texture atlases, collision | both games | [SkyCraft](../case-studies/skycraft.md), [Minecraft × Half-Life](../case-studies/minecraft-x-halflife.md), [GalaxyCraft](../case-studies/galaxycraft.md) |
| **Shared neutral simulation** | a separate server; each game is a viewer | neutral state and input intents | a viewer for their game | [Signet](../case-studies/signetprotocol.md) |
| **Engine recreation** | one rebuilt engine reading the player's own game data | nothing; it's one process | the original game's files | [IW4L](../case-studies/iw4l.md), [2010 Rust Rewrite Mashup](../case-studies/2010-rust-rewrite-mashup.md), [CS:Craft](../case-studies/cs-craft.md), [HL2-RS](../case-studies/hl2-rs.md), [benilla](../case-studies/benilla.md) |
| **Rebuilt guest engine or mechanic inside a real host** | the real host plus a rebuilt engine or mechanic | the rebuilt part's state, through a DLL, a C API or a worker process | the host and their own copy of the guest's data | [BullySkate](../case-studies/bullyskate.md), [SkateGM](../case-studies/skategm.md), [World of Skatecraft](../case-studies/world-of-skatecraft.md), [Faith Runner](../case-studies/faith-runner.md), [ER Mario](../case-studies/er-mario.md) |
| **Asset or map conversion** | the host only | files converted offline | the host, and their own copy of the source game | [PipeLink](../case-studies/pipelinklauncher.md) converters, the map import in [Halo / MW2 Director](../case-studies/halo-mw2-director.md) |
| **Host-API recreation** | the host only | nothing; the guest's look or rules are rebuilt with the host's own modding API | the host | [Universal Modder's cases](../case-studies/universal-modder.md) (Borderlands 3 guns in Borderlands 2, Counter-Strike movement in Elden Ring) |

Also called mashups, and out of scope here: cross-game progression links such as multiworld randomisers, protocol translators ([HyCraft](../case-studies/hycraft.md) lets Minecraft clients join a Hytale server), programs compiled into Minecraft commands ([wasmcraft2](../case-studies/wasmcraft2.md)), games drawn onto another game's map screen ([DoomMaps](../case-studies/doommaps.md) renders Doom on Hytale's world map), and emulators running inside games.

Most shipped projects mix routes. The common pairing is frame compositing for the picture plus a state bridge for collision and combat.

## Four questions, in order

1. **Do you need the guest's real behaviour, or only its look or one rule?** Real behaviour needs the guest running (state bridge, compositing or geometry transfer) or a faithful rebuild. Look or rules alone means conversion or host-API recreation, which is far smaller. Original-looking assets don't bring original physics: Signet draws Doom-looking walls on top of its own simplified 2.5D movement.
2. **Who owns the player, and how does control come back?** Write it down now ([guide 02](02-bridge-contracts.md)). Real choices differ. Minecraft owns movement in SkyCraft, and Skyrim takes over for furniture, mounts and kill-moves. In the GTA V example GTA owns movement on foot and Minecraft owns it in elytra flight. In the Half-Life bridge, ladders, `use`, noclip and death hand movement back to Half-Life.
3. **Can the host draw guest geometry itself?** If yes, the guest gets the host's lighting and shadows, and you work inside the host renderer's timing and state. If no, you paste a picture, estimate lighting from the image, and fight stale frames ([guide 03](03-frame-compositing.md)).
4. **What must each player own and run, and on which OS?** Record exact builds from day one. NewVegasCraft targets Fallout: New Vegas Steam 1.4.0.525. The GTA V example was tested on GTA V Legacy build 3889. Windows hosts can run on Linux or macOS through a translation layer (LibertyCraft under Wine, NewVegasCraft under Proton, the CrossOver bridges on macOS with native Minecraft), and each of those needed platform-specific fixes. Name the layer and its version.

## The first slice for each route

| Route | Smallest slice that proves the route | Check it in the real game before adding features |
|---|---|---|
| State bridge | the host plugin logs one line; one float (player x) crosses and is logged on both sides with a frame counter | positions agree within a frame while walking; a teleport or level load doesn't desync |
| Frame compositing | a fake host renders known geometry against one exported guest frame and the pose recorded for it | a debug view of host depth alone; a marker at the host crosshair lines up with guest pixels |
| Geometry transfer | one guest cube drawn by the host renderer at a known position | a host wall hides it, host light lights it, and host render state is restored afterwards |
| Shared simulation | two viewers show the same server state | prediction corrections are logged and bounded |
| Engine recreation | one asset from the owned install loads and renders | a round trip or byte-match check for that format |
| Conversion | one map or model converts and loads | the converted level can be finished, not only loaded |
| Rebuilt engine in a host | the engine steps from a fixed contract (inputs in, pose out) against a fake host | mount, ride, dismount, and a crash or stall of the guest each behave as the contract says |
| Host-API recreation | the mechanic runs on a greybox with no game data | its numbers match a recorded reference scenario |

Don't promise a universal adapter. Every bridge we studied needed per-host reverse engineering: the camera, a safe entry point on the game thread, collision queries, actors, health, display and input. That held even when two bridges shared a creator, a magic number and a byte layout.

## Mistakes that keep recurring

1. **An overlay sold as a passthrough.** Blocks float over host walls or ignore host light because there's no depth test, or because the guest is a separate window. [Garry's Redemption](../case-studies/garrys-redemption.md) planned in-frame compositing in its design document and shipped an overlay window. Decide compositing or geometry transfer explicitly, and check release notes, not the design doc, before listing a feature.
2. **One word for three routes.** "Passthrough" gets used for state bridges, compositing and geometry transfer. An agent then ports a technique that can't work in your route, such as native shadows on a pasted image. Name the route in your notes and keep using that name.
3. **Running a whole second game for one mechanic.** If you want parkour or skating, a transplanted mechanic or a rebuilt engine in the host is smaller and sturdier ([guide 06](06-rebuilt-engine-in-a-host.md)).
4. **Treating shared rules as the original games.** A neutral simulation runs its own physics. List per viewer what is native (how it looks) and what is shared (movement, hits).
5. **Assuming one bridge fits a second host.** The two CrossOver bridges share a magic and version but differ in header size, memory regions, units and what "damage" means. Version the protocol per host and keep each host's adapter in its own folder.
6. **"Same language" read as "composable".** Two Rust/Bevy engines still disagree on coordinates, entity IDs, physics, input, animation and saves. CS:Craft's plan to share content IDs and a trace interface with IW4L was still a design when we looked. List each seam before you promise a merge.

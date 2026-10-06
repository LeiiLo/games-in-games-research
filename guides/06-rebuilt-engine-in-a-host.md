# 06. A rebuilt engine inside a host

Many "game B inside game A" projects never run game B's executable. They rebuild game B's engine, or one of its mechanics, and plug that into the real host. The rebuilt Skate 3 engine has been put into Bully, Garry's Mod and a recreated World of Warcraft client this way; Mirror's Edge movement into Skyrim and Minecraft; Mario 64's movement into Elden Ring.

Use this route when the thing you want is a mechanic set the host lacks (skating, parkour, a movement model), a rebuild exists or can be built, and running the original alongside would be heavier or impossible.

## Where the rebuilt part runs

| Placement | Example | What it means for you |
|---|---|---|
| **In the host's process, behind a C API** | [ER Mario](../case-studies/er-mario.md) compiles the C library [libsm64](../case-studies/libsm64.md) into a Rust DLL loaded into Elden Ring | lowest latency; must match the host's bitness and address budget; a guest fault is a host fault, so the guest has to recover by itself |
| **Worker processes** | [BullySkate](../case-studies/bullyskate.md): a 32-bit Bully adapter, a 64-bit Skate physics worker and a separate sound worker | the guest can be 64-bit next to a 32-bit host and keeps big state off the host's threads; you need a transport contract and bound lifetimes |
| **Inside a host scripting runtime** | [SkateGM](../case-studies/skategm.md): the rebuilt Skate engine in Garry's Mod through a native module and Lua | the host's scripting decides what's easy; moving props need their own collision layer |
| **Inside a recreated host** | [World of Skatecraft](../case-studies/world-of-skatecraft.md) adds the Skate engine to [benilla](../case-studies/benilla.md), a recreated WoW 1.12.1 client, through an extension point that accepts extra Bevy plugins | no foreign process at all, but you inherit the recreated host's own fidelity |
| **A mechanic inside a rebuilt host engine** | [Diablo II Movement](../case-studies/devilutionx-d2-movement.md) inside [DevilutionX](../case-studies/devilutionx.md): continuous movement above Diablo I's tile occupancy, combat and saves | smallest scope; the host's systems stay in charge |
| **One mechanic library, several hosts** | [Faith Runner](../case-studies/faith-runner.md): Mirror's Edge movement in Rust, linked into an SKSE plugin and loaded by Minecraft through Java's foreign-function API | one mechanic, many hosts; each host supplies only box sweeps and overlap queries |

Some projects embed the rebuilt engine as a 32-bit DLL directly inside an older 32-bit host. That works, but it inherits every constraint in the first row.

## Write a small fixed contract

BullySkate's contracts are the best model we found:

- **No pointers, fixed size.** Bully and the physics worker share one 6,184-byte C-compatible block, scoped by process ID, with command and response events. It holds up to 24 actors, 8 vehicles and 8 input samples, the rider and board pose, and 64 sound values.
- **A separate channel per consumer.** Sound gets its own 296-byte block of the latest values, protected by an odd/even sequence with three reader retries. The host never waits for the mixer.
- **Axes stated once.** Bully is Z-up and the Skate engine is Y-up, so positions and view vectors cross as (x, z, −y), and mount yaw is adjusted by π.
- **The guest keeps its own clock.** The physics worker steps a fixed period built from queued input durations, not once per host frame. On overflow it merges the last input and caps catch-up at 0.1 s.
- **Bound lifetimes.** The worker watches the parent exit, the host watches worker health, and a Windows job object ties them together.
- **Hand back for host content.** You step off the board for doors, shops and missions, then get back on.

## Normalise at the boundary

SkateGM converts SDL controllers (PlayStation, Switch, generic) into the Xbox-shaped input structure the rebuilt engine already used, and chooses button labels separately. The engine's input code never changed. Note its precedence rule: a connected XInput pad wins over SDL.

ER Mario keeps a hidden native player character following Mario, so Elden Ring still handles doors, menus, quests, deaths and saves. Mario uses a separate offline save.

## Check against the original where you can

[Diablo II Movement for DevilutionX](../case-studies/devilutionx-d2-movement.md) compares its movement tables against the player's own copy of a Diablo II 1.12 DLL and against an MIT-licensed reimplementation, keeps a probe-and-report harness, and falls back to stock tile walking when continuous movement stalls. Faith Runner reads behaviour parameters from extracted UnrealScript and from native code inspected in Ghidra. When owned files are missing these checks skip, so record when a run skipped them.

## Problems and fixes

1. **Invalid numbers from the guest.** Keep the last finite transform, reset heading, restore the last good pose, and stop if another error arrives within a few seconds.
2. **Lost button presses during stalls.** Merged or capped input catch-up (BullySkate) can drop a press. Log merges, and keep presses as edge events rather than levels.
3. **Silence or looping sound.** BullySkate's sound worker mutes after 250 ms without a new sequence rather than repeating stale state. Its mixer callback still takes an engine mutex, so "non-blocking host" doesn't mean "lock-free audio".
4. **Host collision for the guest.** BullySkate uses Bully's physical collision volumes, not its navigation volumes, and versions its grind-rail file format with a schema check. SkateGM needs an explicit collision layer for moving props. ER Mario's notes cover mirrored coordinate systems, triangle winding, convex hull orientation, moving platforms, sequential wall correction and stale invisible guard walls.
5. **Unported original behaviour.** SkateGM skips an unported air-dismount producer instead of failing the tick. Faith Runner substitutes an undecoded vertigo check and enables auto-step that the original disabled. List substitutions next to features.
6. **"Original feel" from extracted numbers.** Faith Runner's doubled gravity is inferred from a developer comment; where the doubling actually happens was unresolved. Matching numbers isn't matching behaviour.
7. **A feature that needs a server.** World of Skatecraft's skateboarding profession needs its patched server on Linux; the experimental Windows setup uses a stock server without it.
8. **Reports from an older version.** The Diablo II movement mod's v0.1 used Diablo II speeds everywhere and v0.2 kept Diablo I's dungeon pace, but an older report in the repo still described v0.1. Date every report.

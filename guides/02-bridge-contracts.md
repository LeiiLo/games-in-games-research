# 02. Bridge contracts

Most hard bugs in two-program mods come from something nobody wrote down: who owns the player, which unit a number is in, which frame a pose belongs to, or what happens when one side pauses or restarts. Write a contract file (`docs/CONTRACT.md` works) before the bridge code, and check every change against it.

Use this for any route where two programs share state: a live state bridge, frame compositing, geometry transfer, a mechanic behind a C API, or a worker process.

## Fields to fill in

1. **Route and exact versions** of both programs, their loaders, and any translation layer (Wine, Proton, CrossOver).
2. **Ownership table.** One row each for player position and physics, camera, world geometry, collision in each direction, NPCs, damage and health, inventory, saves, menus and pause, loading screens, and scripted events (cutscenes, vehicles, furniture). Each row names the owner, the trigger that hands control over, and how it comes back.
3. **Units and axes**, written as one function both sides share.
4. **Channels.** For each: kind (snapshot, queue, byte ring, image), direction, rate, and what happens when it's full or late.
5. **Protocol identity.** Name, magic number, version, byte order, and how each side notices the other restarted.
6. **Lifecycle.** Start order, pause, level change, death, disconnect, crash, uninstall.
7. **Not covered.** Systems the bridge leaves alone, and what the player will see because of it.

## Ownership choices other projects made

| Project | Owns player movement | Hands back when |
|---|---|---|
| [SkyCraft](../case-studies/skycraft.md) v0.1.2 | Minecraft | furniture, mounts, kill-moves and native camera scenes return control to Skyrim for their duration |
| [LibertyCraft](../case-studies/libertycraft.md) | Minecraft | GTA IV takes over for missions, cutscenes, minigames and cars; the guest follows a non-physical mount at the seat and re-syncs with a teleport handshake |
| [Minecraft × GTA V](../case-studies/universal-modder.md) | GTA on foot; Minecraft in elytra flight | GTA supplies look direction and the chase camera during flight |
| [Minecraft × Half-Life](../case-studies/minecraft-x-halflife.md) | Minecraft | ladders, `use`, noclip and death return movement to Half-Life |
| [Garry's Redemption](../case-studies/garrys-redemption.md) (design doc) | a hidden Garry's Mod | native actors are mirrored as invisible proxies; the guest drives grabbed or struck proxies, and ownership returns after release and settling |

## Unit mappings other projects used

| Project | Mapping |
|---|---|
| Minecraft × GTA V | one block per GTA metre; x → x, z → height plus an offset, y → −z; yaw = 180 − heading |
| LibertyCraft (GTA IV) | 1 metre per block |
| NewVegasCraft | 70 host units per block; exterior yOffset fixed at −34 after per-load re-levelling moved builds by about 0.9 m |
| Minecraft × Half-Life | 40 units per block (72 units is about 1.8 m); health 20 → 100, a factor of 5 |
| SkyCraft | 70 host units per block |
| Faith Runner in Skyrim | 70 units per metre |
| Killcraft (ULTRAKILL) | 2 host units per block |
| Garry's Redemption design | 0.01905 m per Source unit, with a planned floating origin |
| 2010 Rust Rewrite Mashup | 36 host units per block |

These are the projects' own constants. Put the unit inside every message that carries a quantity, damage included.

## Channel types that worked

LibertyCraft inherited these from SkyCraft's Windows transport:

- **Seqlock snapshots** for state that's replaced continuously (pose, player state). The writer makes the counter odd while writing and even when done. The reader checks it before and after copying and retries if it changed or was odd.
- **Bounded queues** for discrete events, with a stated drop rule.
- **Padded byte rings** for variable-size data such as meshes.
- **Latest-frame triple buffering** for images.
- **Restart generations.** When the generation changes, each side resends the state the other lost.

SkyCraft adds process IDs, heartbeats and epochs so a reader can tell a live peer from stale data, and keeps the previous and current Minecraft tick so the host can interpolate 20 Hz ticks into its own frame rate. Its Java and C++ layouts are mirrored by hand, which makes layout drift a real risk.

## Byte order and layout

[GalaxyCraft](../case-studies/galaxycraft.md)'s host protocol is little-endian while the emulated PowerPC side is big-endian, and much of its model data is already big-endian and mustn't be swapped twice. It ships a C file of compile-time assertions that pins offsets, region sizes and struct sizes. Do the same on both sides of your bridge. It costs minutes.

## Mistakes worth avoiding

1. **Same magic, different meaning.** A bridge built for one host half-works on another. Version per host and put the unit in the message.
2. **A seqlock that isn't one.** In one CrossOver host the frame writer moved the counter from one even value straight to the next, so it never marked a write in progress; the other host's writer did it right. Nobody showed a visible glitch from it. Test the writer: assert the counter is odd during the write.
3. **Layout reuse taken as compatibility.** LibertyCraft kept SkyCraft's v11 byte layout but changed the magic and added GTA-specific events. Change the magic or version whenever meaning changes, so an old peer gets rejected instead of misread.
4. **Overwriting on mismatch.** The Half-Life bridge overwrites an existing shared mapping when magic or version differ. Reject and log instead, so two builds can't corrupt each other.
5. **Recycled entity IDs.** Referring to native entities by index alone lets a late event hit a recycled entity. Send an (index, generation) pair.
6. **Blocking calls on the game thread.** NewVegasCraft's first WebSocket client sent synchronously from the game loop with no deadline. Signet's client wrote TCP while holding a mutex. Use a sender thread with a bounded queue, and time-outs.
7. **Events eaten by a size query.** Signet's C interface consumed events during a null-buffer sizing call, so the usual "ask the size, then copy" pattern lost them. Make sizing calls side-effect free.
8. **A setting both sides touch.** If your plugin flips a host flag (NewVegasCraft disables fighting), record the original value and who set it.
9. **Generated-file drift.** [BullySkate](../case-studies/bullyskate.md) moved its rail file from segments to polylines and bumped a schema version that its launcher checks. Version every generated file and refuse a mismatch.
10. **Scripted openings with the guest in control.** SkyCraft's docs warn that Skyrim's opening cart scene can stall while Minecraft owns movement, and suggest starting from a later save. Hand control back for scripted sequences, or document the workaround.

## Template

```markdown
# Bridge contract: <guest> inside <host>

Route: <state bridge | compositing | geometry transfer | rebuilt engine | ...>
Host: <game, exact build, loader + version, OS / translation layer + version>
Guest: <game or engine, exact version, loader + version>

## Ownership
| System | Owner | Hand-over trigger | How it comes back |
|---|---|---|---|
| Player movement | | | |
| Camera | | | |
| Collision host → guest | | | |
| Collision guest → host | | | |
| NPCs | | | |
| Damage / health | | | |
| Inventory | | | |
| Saves | | | |
| Menus / pause / loading | | | |
| Cutscenes / vehicles / furniture | | | |

## Units and axes
host_to_guest(x, y, z) = ...

## Channels
| Name | Kind | Direction | Rate | When full or late |
|---|---|---|---|---|

## Protocol
Magic: ... Version: ... Byte order: ... Restart detection: ...

## Lifecycle
Start order / pause / level change / death / disconnect / crash / uninstall

## Not covered
...
```

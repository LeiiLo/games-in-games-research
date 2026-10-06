# 05. Collision and combat

"Collision supported" hides four separate jobs: getting the host's geometry, turning it into something the guest understands, sending the guest's builds back so host actors respect them, and keeping both fresh as things move. Combat adds units, attribution and double counting.

## 1. Get the host's geometry

| Host | How the project got it |
|---|---|
| Skyrim ([SkyCraft](../case-studies/skycraft.md)) | walks Havok collision shapes under the engine's own locks; a worker streams nearby regions |
| GTA IV ([LibertyCraft](../case-studies/libertycraft.md)) | incremental native line probes plus sampling nearby objects |
| GTA V ([example](../case-studies/universal-modder.md)) | ground probes: 160 columns per frame within radius 40, plus a budget of 120 around mobs and police |
| Fallout: New Vegas ([NewVegasCraft](../case-studies/new-vegascraft.md)) | a distance-sorted disk of radius 32, at most 48 columns per host tick, with a terrain-height fallback |
| Half-Life ([Minecraft × Half-Life](../case-studies/minecraft-x-halflife.md)) | BSP faces and the player-clip hull, with the hull's expansion removed first; moving brushes send updates |
| Super Mario Galaxy 2 ([GalaxyCraft](../case-studies/galaxycraft.md)) | the other way round: guest geometry becomes native KCL collision |
| Elden Ring ([ER Mario](../case-studies/er-mario.md)) | live Havok queries adapted for an embedded Mario 64 movement library |

LibertyCraft kept SkyCraft's guest-side representation (triangles plus eighth-block voxels) and replaced only the host-side sampling. Keeping the representation is what made the port cheap.

## 2. Pick a representation for each consumer

SkyCraft keeps exact triangles for the local player's smooth movement, voxel shapes (8×8×8 per block) for general entity queries, and coarse occupancy for everything else. Capsules and spheres become boxes; unsupported convex shapes become bounding boxes. LibertyCraft uses triangles for the local player, voxels for other guest entities, full-cell masks for host NPCs, and column heights for water. When you write "exact collision", say which representation and for whom.

SkyCraft also tells apart a region known to be empty from a region it hasn't received yet. Your guest should too, or it'll drop players through unloaded ground.

## 3. Send guest builds back to the host

| Project | Guest → host |
|---|---|
| GTA V example | a block change becomes a solid/not-solid event and then a frozen GTA box (at most 400, up to 30 new per frame); slabs and stairs are just "solid" |
| [FalloutCraft](../case-studies/falloutcraft.md) | solid guest blocks become merged native Havok boxes; NPCs collide with them but don't path around them |
| NewVegasCraft | not built at the version we read; host actors walk through guest blocks |
| SkyCraft | native mesh cutting for dug holes, extra support and wall planes in native collision, and an intercept that stops NPCs being lifted out of holes |

Physical blocking, AI navigation and visual removal are three outcomes. Check each one.

## 4. Keep it fresh without stalling

- **Prefetch along velocity and hold the guest.** Early [ValCraft](../case-studies/valcraft.md) fetches host regions ahead of the player, holds the guest at its last position (keeping momentum) while data is missing, and makes the host puppet kinematic so a collision gap can't fling it. A two-second timeout then lets movement continue, so this reduces the problem rather than removing it.
- **Budget rebuilds for moving objects.** Some projects rebuild nearby vehicle collision only after it moves a set distance and at most once a second, and skip unchanged triangle batches by hash. A hash of an ordered batch catches identical data, not geometric equivalence.
- **Mark moving regions dirty.** The Half-Life bridge marks both the old and new region of a moving brush, and cancels stale worker jobs with an epoch.

## 5. Combat

- **Invisible proxies** are the common pattern. Host NPCs become invisible guest entities (villagers in the GTA V example, living-entity proxies in SkyCraft) so guest weapons can hit them. Guest fighters become frozen, invisible host doubles so host AI can target them. SkyCraft moves its client proxies every frame because server-tick updates arrived too late for hits to line up visually.
- **Put the unit in the damage message.** Minecraft 20 → Half-Life 100 (×5) in the Half-Life bridge; native HP in the Monster Hunter bridge; Minecraft damage scaled by the target's maximum HP in the Elden Ring bridge. SkyCraft scales damage by NPC level, so fights don't feel like vanilla Minecraft.
- **Deduplicate.** The GTA V example remembers eight recent explosion positions for half a second, and stops player projectiles hitting ped proxies.
- **Keep native attribution.** LibertyCraft adds explicit crime attribution, ragdoll before death, safe death in cars, and a large native health buffer that feeds guest-side damage.
- **Watch side effects of invulnerability.** Making the host player explosion-proof to avoid double damage also blocks legitimate host explosion damage. Write that trade-off down.

## Problems and fixes

1. **Host NPCs walk through guest blocks.** Collision only went host → guest. Add the reverse path, and treat navigation as a separate job.
2. **Barriers stay behind after a door or car moves.** Cached columns were never invalidated. Use dirty regions for anything that moves.
3. **Blocks placed inside rocks and signs.** There was only a thin barrier skin. NewVegasCraft switched to solid columns from terrain up to the highest surface (capped near 24 blocks), which also fills arches. Record that trade-off.
4. **Dug holes refill.** New collision samples overwrite earlier edits. Keep edit masks separate from sampled geometry.
5. **Vehicles and cutscenes.** A collision-free puppet can't trigger native doors or seats. LibertyCraft hands control to GTA in cars and has the guest follow a non-physical mount. It pushes doors with a remembered contact side and tries several native door paths.
6. **The guest produces invalid numbers.** A rebuilt physics engine can go non-finite. Keep the last finite transform, reset heading, restore the last good pose, and stop if errors repeat within a few seconds. Some projects reload the guest engine off the main thread without restarting the host.
7. **Water by name only.** [Signet](../case-studies/signetprotocol.md)'s Doom and OpenArena importers set the neutral water flag to false even for water, lava and toxic materials. Map hazards explicitly. SkyCraft exports a surface-column grid that only guest physics sees; LibertyCraft exports hazard tags that trigger native damage and wading.
8. **Very short-lived bullets miss.** In a Counter-Strike-in-Elden-Ring conversion, bullets could expire before the next collision frame, so a minimum lifetime mattered.

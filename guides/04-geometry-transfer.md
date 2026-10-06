# 04. Geometry transfer

Here both games run, but the host draws the guest's world itself: the guest exports meshes, texture atlases and collision, and the host renders them as if they were native. You get the host's lights and shadows. In exchange you work inside the host renderer's timing and state, and you have to keep a stream of geometry fresh.

## What the guest sends

[SkyCraft](../case-studies/skycraft.md) is the most complete example we studied. Its Minecraft exporter uses Minecraft's own block and fluid renderers and models to produce triangles, texture-atlas updates, held items and arrows, block outlines, lights, a bitset of solid cells and a bitset of dug cells. Skyrim then draws them with its own lighting, replacing some of Minecraft's baked directional brightness.

The exporter limits itself to 12 changed sections and a scheduling deadline of about 3 ms per frame, sends recent edits first, and retries sections when the ring buffer is full. That's a work budget, not a measured end-to-end latency.

Its shared-memory header also has room for up to three full-screen RGBA overlays, used for things like the HUD. An overlay channel existing doesn't mean the world is a flat picture.

## Other ways to do it

- [Minecraft × Half-Life](../case-studies/minecraft-x-halflife.md) modifies Half-Life's client and server DLLs so Minecraft geometry goes through Half-Life's own OpenGL pipeline. Only the HUD is a pasted image.
- [GalaxyCraft](../case-studies/galaxycraft.md) goes into an emulated game: Minecraft data becomes GameCube display lists, textures and KCL collision, and Super Mario Galaxy 2 running in Dolphin draws them.
- [LibertyCraft](../case-studies/libertycraft.md) ported SkyCraft's guest side to GTA IV and rewrote only the host renderer and collision sampling.

## Checks before you add features

1. One guest cube, drawn by the host at a known position.
2. A host wall hides it. A host light lights it.
3. The host's render state is the same after your draw as before it. State leaks between passes are the common bug: a previous pipeline can stop a depth clear from working, and depth test and depth write are separate switches (transparent surfaces often need test without write). [HL2-RS](../case-studies/hl2-rs.md) documents both.
4. An edit in the guest (dig a block) shows up in the host within a stated number of frames.

## Digging and destruction go both ways

When a player digs in SkyCraft, Minecraft records the mined cells and supplies replacement interior walls, and Skyrim removes the matching render and collision surfaces. Its v0.1.1 code declines triangles marked non-diggable, such as protected buildings, so "mine anything" isn't literal. Its v0.1.2 change keeps nearby triangles and prunes distant ones when classifying blast material; the commit cites a profile in which the old probe took 86% of server-thread time.

SkyCraft only destroys host terrain where the host knows native geometry, so explosions far from the host player leave Skyrim alone. Document limits like that next to the feature.

## Interiors and multiplayer

SkyCraft shares one Minecraft world between players while each player runs their own Skyrim. Host quests, NPCs and saves aren't shared. Interiors share coordinates, so blocks placed in one interior can show up in another. A player count in a README isn't a stress test.

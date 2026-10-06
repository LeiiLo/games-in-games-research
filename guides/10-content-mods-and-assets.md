# 10. Content mods and generated assets

Plenty of "game X in game Y" ideas don't need a second game at all. If the goal is new items, units, enemies, a civilisation, or a map, a supported mod loader or data route is enough. These notes come from [Universal Modder](../case-studies/universal-modder.md)'s worked examples (Fal Arsenal for Terraria and the San Franciscans civ for Age of Empires II DE) and its field notes.

## Start from the host's supported boundary

- Fal Arsenal is a tModLoader C# mod: five weapons, three enemies and a boss, built from framework-discovered item, NPC, projectile and system classes plus generated sprites. No other game runs inside Terraria.
- San Franciscans replaces an existing civilisation slot in AoE2 DE, clones a base civ's structure, and adds units, a wonder, bonuses and technologies. The builder checks the slot's name before changing it. The author reports that the game's civ picker and icon tables prevent adding a visible extra slot.

## Art pipeline

- **Sprites (Terraria):** concept images on white, background removal, rotations, nearest-neighbour fitting, padding and small transforms. Items point right, NPCs face left, and frames stack vertically. Bobbing, squash and rocking come from transforming one drawing. Only two rebuilt sprites matched the original pipeline byte for byte; regenerating art without a fixed seed changes it.
- **Units (AoE2):** concept image → textured 3D model → consistent Blender renders → a custom SLD encoder. Sixteen headings start east and turn clockwise, with an orthographic camera 30° above the ground. A saturated blue trim becomes the player-colour mask. One model keeps all directions consistent. A painted wonder needs one view and skips 3D.
- **Format details matter more than the source image.** SLD uses BC1 for colour and BC4 for shadow and player masks in 4×4 tiles with skip/draw runs. The example writer emits independent frames and no damage layers, and the source notes header variants it doesn't understand, so it isn't a general lossless round-tripper. Palettes, camera angles, frame order, pivots and team colours decide whether art looks native.
- **Regeneration isn't reproduction.** The shipped AoE2 models came from one model generator; the script now calls a newer one, and its motion parameters go beyond what the simple CLI flag can express. Re-running the printed command doesn't reproduce every shipped asset.

## Data pipeline

- Copy stock units, techs and effects, then change selected fields. The AoE2 drone inherits crossbowman mechanics; its hovering art isn't a flight simulation.
- Change IDs within their category. Restricting replacements to `Units` fixed an earlier collision between two unrelated IDs.
- Don't assume string IDs are free. Scan for collisions.
- Hard-coded UI slots may force you to reuse native icons.

## Code does the spectacle

Generated art supplies textures. Code supplies targeting, movement, damage, destruction and effect timing: Fal Arsenal's Tesla chain hits up to five enemies and draws jagged arcs, the singularity pulls enemies and items then implodes, and the nuke authors its crater, flash, sound layers and a mushroom cloud built from a small pixel disc and timed puffs.

Different enemies need different choices. One borrows vanilla slime AI. Another needed custom chase-and-hop logic because vanilla daytime behaviour didn't suit it.

## Multiplayer is its own job

Fal Arsenal's README says multiplayer is untested. Reading the code shows why it matters: small explosions skip containers and send removed-tile messages, but the nuke edits every machine's local world directly with no tile sync, and some pulling logic runs without a single-owner guard. Plan sync for every world edit.

## More cases of the same kind

Universal Modder's knowledge base has field notes for host-API recreations: Borderlands 3 guns and movement in Borderlands 2 (Python on the game's own SDK), Counter-Strike movement and economy in Elden Ring (a Rust DLL and host parameters), [Blink Behind](../case-studies/blink-behind.md) for Dragon's Dogma 2, a Darkest Dungeon seeded randomiser, Halo content converted into a Minecraft mod, a Victoria II total conversion, newer World of Warcraft armour backported to an older client, and the [Mewgenics combat roster](../case-studies/mewgenics-combat-roster.md). Lessons that repeat across them:

- Keep transient runtime definitions out of saves; give per-item choices stable IDs.
- Collect targets before killing members of a linked list; defer landing work to a later tick; guard re-entrant damage calls.
- When a hook can't replace the original arguments, reapply the effect in a post-hook with a recursion guard, and say so.
- Retain managed arguments, avoid pointer truncation, and finish effect instances explicitly.
- Recognise the real engine stack before choosing tools; stray strings can suggest the wrong engine.
- Start a data mod with a one-item canary. Same-path replacements and new-file merges behave differently.
- Preserve multi-texture passes and UV conventions instead of round-tripping through an editor that drops them; cap textures to the old client's limits.
- A staged showcase inventory isn't evidence of naturally earned progression, and a long soak without a crash shows stability, not balance.

# Start here (plain-language version)

This page is for anyone who wants the idea first and the detail later. You don't need to know how to program to follow it.

## What this is about

People sometimes build mods that put one game inside another: Minecraft inside Skyrim, a skateboarding game inside a city game, and so on. In a short video these all look alike. Underneath, there are only a handful of different ways to do it, and picking the wrong one is the most common reason these projects stall.

This collection explains those ways, what each one is good and bad at, and what real projects ran into.

## The short version

1. **Decide what you actually want.** The real game's behaviour, or only its look, or only one mechanic from it? That one answer rules out most approaches.
2. **Decide who is in control.** In a mashup, one game usually owns the player and the other follows. Write down which, before anything else.
3. **Expect the unglamorous problems.** Pictures that slide, depth that doesn't line up, one game pausing while the other runs, installers that load things in the wrong order. These cause far more trouble than the headline idea.
4. **Be honest about what was tested.** "The code says it works" and "I played it and it worked" are different claims. Say which one you mean.

## Words you'll see

1. **Host:** the game that stays in charge of the window and the player's computer.
2. **Guest:** the game, engine or mechanic being brought in.
3. **Bridge:** the code that passes information between the two.
4. **Contract:** a short written list of who owns what and which units each side uses.
5. **Frame compositing:** drawing one game's picture on top of the other.

## Where to go next

1. Read [choosing a route](guides/01-choosing-a-route.md). It's four questions and a table.
2. Skim [case studies](case-studies/README.md) for the project closest to your idea.
3. If something goes wrong, [installers and diagnostics](guides/08-installers-and-diagnostics.md) and [evidence levels](guides/09-evidence-levels.md) are the practical pages.

The full detail is in the [guides](guides/README.md). Those pages are written to be read by people and by coding agents; this page is the gentle entrance.

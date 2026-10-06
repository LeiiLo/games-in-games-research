# 07. Reconstruction and fidelity

Engine recreations ([IW4L](../case-studies/iw4l.md), [SK8-ENGINE](../case-studies/skate-3-rust-engine.md), [CS:Craft](../case-studies/cs-craft.md), [HL2-RS](../case-studies/hl2-rs.md)), static recompilation ([WiiCompiled](../case-studies/wiicompiled.md)) and matching decompilation ([mkdd](../case-studies/mkdd.md)) all rebuild a game's behaviour from its binary and data. They're related, and they're four different things:

| Term | What it produces | How you measure it |
|---|---|---|
| **Matching decompilation** | source code that compiles back to the same bytes as the original | per-function byte match with the original compiler and settings |
| **Static recompilation** | the original machine code translated ahead of time into another language, plus a platform runtime | instruction coverage and behaviour parity on real inputs |
| **Engine recreation** | a new engine that reads the original data and reimplements its rules | trace comparisons against the original, per system |
| **Emulation** | the original binary running on an emulated machine | timing and device accuracy |

A byte-matching metric, a linked build, a deterministic export and behaviour parity are four different numbers. Don't add them up.

## Start from what the binary contains

Identify the engine and runtime before you choose tools. Native executables need Ghidra, IDA or Binary Ninja. .NET games expose readable IL through ILSpy or dnSpy. Java has CFR, Vineflower and JADX. Lua, Python, IL2CPP and Delphi each have their own bytecode and layout tools. Asset extraction, disassembly, decompilation, behavioural reconstruction and runtime integration are separate outputs; plan for each.

An index such as [gamedb](../case-studies/gamedb.md) (decompiled source loaded into SQLite with functions, symbols, strings, callers and modules) speeds up lookups. It doesn't decompile anything itself, and its lightweight parsing can miss constructs. A cached size-and-timestamp check can miss changed content; force a re-index with a hash manifest when coverage matters. Each search result should point back to the full source and its version.

## The fidelity loop

The most useful workflow we saw:

1. Capture original inputs and state.
2. Build a narrow reconstructed slice.
3. Replay the same inputs into it.
4. Compare traces against a golden recording and find the first divergence.
5. Fix the constant, timing, random number, numeric order or hidden state that caused it.
6. Keep both the failing and the repaired evidence.

You either have an executable oracle (you can run the original and record it) or you rely on binary and decompiler observations. Say which.

WiiCompiled's ghost-input replay is a good parity test for the routes it covers. It doesn't prove "100% physics" on routes nobody replayed. Its translator tests default to synthetic inputs and skip asset and host-compiler tests by default, and its manifests require exact binary hashes. Unsupported instructions fail, or use traps you opt into explicitly.

## Lessons from recreations

- **Credit prior work.** IW4L's notice credits several earlier asset, protocol and engine projects as references. SK8-ENGINE credits years of human research and extraction tools before its Rust engine. Reconstruction rarely starts from zero.
- **Keep versions apart.** IW4L's demo.2 release claims tested loading of maps from MW2, MW3 and Black Ops with bots under MW2 rules. That's cross-title map support, not three games. Its network protocol changed across revisions, so read the docs that match your build.
- **Scripts are part of fidelity.** In the 2010 runtime, GSC scripts compile to an intermediate form and run only on authority frames. Their scheduling, event order, copy and alias semantics and random seed all matter. Runtime errors yield undefined values and carry on without rolling back earlier writes, and runaway loops are capped.
- **Foreign content keeps host rules.** When the 2010 runtime loads maps from other titles, it takes their entities and assets and drops their gameplay and UI scripts. MW2 modes and rules stay in charge.
- **Same language isn't composable.** Rust doesn't make engines fit together. World IDs, coordinate systems, physics, input, entity lifecycle, animation, saves, assets and networking still need reconciling.
- **Build determinism isn't game determinism.** Identical generated source doesn't guarantee identical random numbers, floating-point order, thread order or host inputs.
- **Render optimisations need render checks.** SK8-ENGINE's renderer changes (bindless texture-slot reuse, conservative occlusion depth, cache keys tied to buffer identity and revision) have to preserve motion and visibility updates. Assertions about data structures don't show that.
- **Measure narrowly and say so.** The 2010 mashup's performance doc compares alternating five-run medians on one machine and one no-bot route (183 → 362 fps). It states that the heavy-bot case wasn't measured. That's the right way to report a benchmark.

## Ways to organise the work

Stable address-to-symbol mappings, compact query views, narrow row patches, schema checks, hash-gated regeneration, clear module ownership, isolated worktrees, append-only experiment journals and short handoff notes all cut repeated discovery. None of them shows correctness without a meaningful comparison.

Data-driven designs (schemas and tables with provenance, generating code at build time, with hand-written kernels where needed) are useful. A complete table isn't proof that all runtime behaviour was recovered. Build scripts that generate code run on the build machine, so they're not a sandbox.

## Reinforcement learning is a special case

One Terraria project rebuilt the Eye of Cthulhu fight as a fast Python simulator, trained a policy on it, and exported the weights into a mod. Its 99.7% win rate is a simulator number. Action timing, trace alignment and a hidden movement buff from a sunflower item explained why a good simulator policy did worse in the real game. Validate transfer in the real game.

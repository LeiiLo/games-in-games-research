# Games-in-Games Research

Methods for putting one game, engine or mechanic inside another, and what real projects did when they tried.

"Minecraft inside Skyrim" and "Skate inside GTA" look alike in a clip. Underneath they can be two live games swapping state, a picture pasted into another game's frame, guest meshes drawn by the host's renderer, a rebuilt engine loaded as a DLL, or a converted map. Each route has different costs, different failure modes and different things every player has to own. This repository sorts them out.

We wrote it by studying several existing games-within-games projects in detail to find their methods, plus examples from a community of what people are building and how.

New here? Read [START-HERE.md](START-HERE.md). Agents and expert readers: [FOR-AGENTS.md](FOR-AGENTS.md).

## What's here

| Folder | What you get |
|---|---|
| [guides/](guides/README.md) | Method guides in our own words: picking a route, writing a bridge contract, frame compositing, collision and combat, rebuilt engines inside a host, reconstruction and fidelity checks, content mods, installers and diagnostics, and how to label evidence. |
| [case-studies/](case-studies/README.md) | One page per project we learned from. Each links the project's repository at the commit or tag we looked at and says what we took away. No code or text from those projects is copied here. |
| [SOURCES.md](SOURCES.md) | Every named source, pinned to a commit, tag or video. |

## How to use it

1. If you have an idea ("put X in Y"), start with [choosing a route](guides/01-choosing-a-route.md). It asks four questions that decide most of the architecture.
2. Find the closest case study and open that project's own repository. Read its current README; ours describes the version we looked at.
3. Before writing bridge code, fill in a contract using [bridge contracts](guides/02-bridge-contracts.md).
4. When you report progress, label each claim with the levels in [evidence levels](guides/09-evidence-levels.md).

## The same lessons in other projects

We've offered these lessons as pull requests to two projects that people already use, rewritten in each project's own format:

1. **[ai-game-modding-guides](https://github.com/trevaintdead/ai-game-modding-guides)** ([pull request](https://github.com/trevaintdead/ai-game-modding-guides/pull/3)). Its pull request adds a guide on choosing a route, a case-study guide, a table of ownership, sync and rendering symptoms, and templates for a bridge contract, a playtest report and attribution.
2. **[universal-modder](https://github.com/rehan-remade/universal-modder)** ([pull request](https://github.com/rehan-remade/universal-modder/pull/135)). Its pull request adds nine technique notes for its knowledge base (choosing a route, bridge contracts, frame compositing, geometry transfer, collision and combat, a rebuilt engine inside a host, engine recreation, installers and load order, and evidence levels), a table of mashup projects, and a checklist its mashup skill runs before picking a pattern.

Each pull request stands on its own. This repository keeps the fuller version, with a page for every case study.

## What this is not

- **Not a test report.** We read source, docs, commit pages and release notes. We didn't run the games, mods, installers or tests for this edition. A sentence like "the code does X" means someone read the code.
- **Not a tool.** There's no installer or engine here. The guides are written for people and for coding agents planning a mod.
- **Not a ranking.** Where a project has a limit or a bug, we say so because the lesson is useful, not to score it.

Version numbers, build numbers and measurements belong to the version we looked at. Creator measurements stay attributed to the creator. Check each project's current release before you rely on a detail.

## AI credit

We say a project used AI only where the project itself says so: its README, credits, or a commit co-authored by a model. We don't infer it from how a project looks. Missing credit isn't evidence either way.

## Licence

The template in guide 02 is under the MIT License. Everything else is under Creative Commons Attribution 4.0. See [LICENSE](LICENSE). Project names, trademarks and links to other projects belong to their owners.

Maintained by LeiiLo. Version history: [CHANGELOG.md](CHANGELOG.md).

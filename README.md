# kurodakoo | Roblox scripting samples

Three connected Luau samples: combat, persistent progression, and grid placement. The source is the portfolio. A small Studio place is included so the systems can also be exercised together.

**Discord:** kurodakoo · **GitHub:** [kurei8182-cell](https://github.com/kurei8182-cell)

[Browse every script](docs/SCRIPT-INDEX.md) · [Portfolio overview](PORTFOLIO.md) · [Tests and verification](docs/VALIDATION.md)

## Read the code

| Sample | Start here | What to inspect |
| --- | --- | --- |
| Combat and abilities | [CombatService.luau](src/server/CombatService.luau) | Four-hit combos, timed parries, stun cancellation, overlap queries, line-of-sight checks, dash collision checks |
| Inventory and saving | [DataService.luau](src/server/DataService.luau) | Session ownership, ordered saves, retries, disconnect handling, refusing invalid data |
| Grid placement | [BuildService.luau](src/server/BuildService.luau) | Per-player plots, rectangular rotation, server placement checks, inventory consumption and pickup |

The [portfolio overview](PORTFOLIO.md) is written for someone reviewing the samples. [Code notes](docs/CODE-NOTES.md) explain the decisions and extension points. [Validation](docs/VALIDATION.md) distinguishes automated checks from pending Studio checks.

## Open the demo

1. Open `SystemsLab.rbxlx` in Roblox Studio using **File > Open from File**.
2. Press **Play**. The scripts create the test scene at runtime; the place looks empty before Play.
3. Fight the sentinels in the red arena, visit the blue supply station, or open the inventory and place furniture on your plot.

No plugin, paid asset, external package, HTTP access, or animation upload is required for the basic demo. Animation IDs are optional slots in `src/shared/Settings.luau`; they are zero by default. Attacks have timing and visual flashes, but no custom punching animation until you provide assets.

In Studio, profiles are **session-only by default**. They reset when the server stops. Published servers use a separate DataStore named `KurodakooSystemsLab_v1`. For a persistence test, use a separate test experience. Set `STUDIO_SAVING` in `DataService.luau` to `true` and enable Studio API access there, or test in a published private experience. Don't point this sample at an existing game's data.

Use a small test server, up to eight players. This is a demonstration scene, not a finished game or a measured concurrency benchmark.

## Controls

| Action | Keyboard / mouse | Gamepad | Touch |
| --- | --- | --- | --- |
| Light attack / place | Left click | R2 | Attack button |
| Heavy / rotate while building | E / R | X | Heavy / Rotate button |
| Parry / pick up while building | F / X | L2 | Parry / Pick up button |
| Dash / leave building mode | Q | B | Dash / Combat mode button |
| Inventory | Tab | Y | Bag button |
| Medkit | H | D-pad up | Heal button |

Furniture selection is in the inventory. On touch and gamepad, aim with the center of the screen. On desktop, aim with the mouse. Buy buttons require standing near the supply station. Claim the one-time contract reward after defeating three sentinels.

PvP is disabled by default. `Settings.Combat.pvp` enables player targets, but this sample has no duel consent flow, safe-zone rules, or competitive latency compensation.

## Source layout

```text
src/
  shared/           Catalog, settings, and grid math
  server/
    core/           Engine-independent rules and data validation
    CombatService   Roblox hit queries and ability execution
    DataService     DataStore adapter and session lifecycle
    BuildService    Plot instances and placement requests
    SentinelService Simple open-arena training behavior
    World           Demo environment
    Main.server     Lifecycle, remote routing, and integration
  client/           Input, UI, preview, feedback, optional animations
tests/              Rule tests and yielding DataStore simulation
docs/               Reading guide and verification checklist
```

Only the shared modules and client scripts replicate to players. Damage decisions, rewards, inventory changes, and save credentials remain on the server.

## Build and check

Tools used for this package: [Rojo 7.7.1](https://github.com/rojo-rbx/rojo/releases/tag/v7.7.1) and [Luau 0.742](https://github.com/luau-lang/luau/releases/tag/0.742). The lifecycle-test runner also needs Python 3. These tools are not bundled in the archive.

From this folder, with the executables on your PATH:

```powershell
luau tests/run.luau
python tests/run_data_service.py
rojo build default.project.json -o SystemsLab.rbxlx
```

Or run `tools/verify.ps1` to compile the scripts, type-check the pure modules, run both suites, and rebuild the place. Supply executable paths if the tools are not on PATH. Rebuild after editing source files; the included place does not automatically sync changes.

## Scope

These are demo systems with source and automated tests. They are not examples of shipped client work, revenue, visits, or production scale. The current test report does not claim that Studio playtesting or live DataStore testing has been completed.

Roblox references used when designing the adapters: [client/server validation](https://create.roblox.com/docs/scripting/security/client-server-boundary), [player data and session locking](https://create.roblox.com/docs/cloud-services/data-stores/player-data-purchasing), and [world queries](https://create.roblox.com/docs/reference/engine/classes/WorldRoot).

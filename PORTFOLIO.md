# kurodakoo

Roblox scripting samples | Luau

Three systems with source code, a test place, and automated checks. Start with the sample closest to the job you need done.

## Combat and abilities

A configurable melee system with a four-hit light combo, a charged strike, a short directional parry, a dash, and training enemies.

The client sends an action name. The server owns recovery times, cooldowns, damage, target selection, and rewards. Attacks query a box after their windup, deduplicate targets, and check line of sight. Getting stunned invalidates a pending strike. A finisher applies knockback; a successful parry interrupts the attacker.

Read [CombatService](src/server/CombatService.luau), [CombatState](src/server/core/CombatState.luau), and the [configuration](src/shared/Settings.luau).

**Useful for:** combat prototypes, configurable abilities, RPG systems, and integration with supplied animations.

## Inventory and persistent progression

A credits wallet, item shop, consumables, enemy rewards, and a one-time contract reward. Buying an item checks the server's price, available balance, ownership limit, and distance from the shop. Placement and pickup share the same inventory.

The save adapter holds a session lease, makes periodic snapshots, retries failed writes, and waits for an in-flight save before releasing a session. A load failure never becomes a writable default profile. Invalid or newer schemas are refused instead of silently replaced.

Read [DataService](src/server/DataService.luau), [Economy](src/server/core/Economy.luau), [Profile](src/server/core/Profile.luau), and [Lease](src/server/core/Lease.luau).

**Useful for:** simulator progression, shops, inventory UI integration, and saved player settings.

## Grid placement

An eight-by-eight plot with snapping, quarter-turn rotation, preview feedback, overlap checks, and pickup. Rectangular furniture changes its footprint when rotated. The server derives the final transform from an item ID and grid coordinates; the client cannot submit an arbitrary position or someone else's plot.

The saved layout contains plain data rather than Instances or CFrames. Removing a piece refunds it once. Items on the plot count toward purchase limits.

Read [BuildService](src/server/BuildService.luau), [Grid](src/shared/Grid.luau), and the [client controller](src/client/Main.client.luau).

**Useful for:** tycoons, shop simulators, housing, and base customization.

## Review notes

32 automated tests cover the rules and a simulated save lifecycle. All 17 source files compile; the eight engine-independent modules pass Luau's type checker. The Studio integration checklist is still pending. See the [validation report](docs/VALIDATION.md) for the exact boundary.

The scene uses generated parts and basic effects. Custom animation slots are provided, but no animation assets are included. Movement anti-cheat, rollback networking, trading, and real-money purchases are outside these samples.

Contact: **kurodakoo**

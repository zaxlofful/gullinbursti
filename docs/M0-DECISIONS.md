# M0 answers and open claims

These are proposals, not verified game facts, until the live assembly is available.

## Sustained damage

Unverified. Inspect burning, AoE, and ballista implementations for their tick scheduler and authoritative hit path. Proposed lance model: owner-only fixed-rate ticks, hard rate cap, one hit-resolution path, pressure/heat charged per accepted tick.

## Demister multiplayer

Unverified. Determine whether Wisplight's effect is a local camera/fog operation or replicated world state. If local, the lamp must run the same client-side effect for every nearby client; owner-only execution is insufficient.

## Intent seam

Yes as a design seam: `HumanDriver`, `Governor`, and `Autopilot` each produce `DriveIntent`; `Motor` consumes only the selected intent. `Motor.cs` need not change when Autopilot arrives. Exact input, physics, and ownership APIs remain unverified.

## Lore channels

Unverified: inspect `RuneStone` registration, tutorial keys/`Player.ShowTutorial`, Munin presence, and `DreamTexts` mutability. Fallbacks: one-time `MessageHud` text for raven lines; omit dreams if additions are unsupported. Lore never gates function.

## Kitbash inventory proposal

Pending prefab-list verification. Candidates: volumetric iron/wood wall and roof pieces for cab occlusion; beams for frame; boiler/smelter-like body; cart or ship wheel assemblies for drivetrain; turret/ballista ring and emitter; Wisplight/wisp-lamp housing; windmill-driven rotating mesh or gear for gauge needle; furnace/eitr-lit component for Tier IV cogitator; vanilla smoke, steam, glow, and metallic audio sources.

## Brief corrections or underspecification

- Exact game version, Unity editor, BepInEx, Jötunn, item names, and prefab names are unverified.
- `ZRoutedRpc` payload and ownership-transfer semantics need explicit tests.
- “Three clients see same gauge readings” needs a defined replication/update cadence.
- “No player build damage” needs a verified target-component traversal, not only a root-component check.
- Camera patch point and demister replication are release-blocking unknowns.
- Offline reconciliation needs an atomic, tested cargo/depot transaction model after exact `ZDO` APIs are known.


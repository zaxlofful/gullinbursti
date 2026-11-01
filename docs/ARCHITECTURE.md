# Gullinbursti architecture — M0 proposal

This is a pre-code architecture, pending API and prefab verification.

## State and authority

ZDO is authoritative. `MachineController` reads/writes persistent state only when `ZNetView.IsValid()` and `IsOwner()`. Other peers render replicated state. Vehicle owner resolves damage, drops, cargo, fuel, and mission reconciliation.

Control flags remain separate from machine state: driver, gunner, passengers, governor, and mission suspension. No `BeingDriven` state.

## Intent seam

`IIntentProducer` produces a validated `DriveIntent`; `HumanDriver`, `Governor`, and later `Autopilot` implement it. `Motor` consumes the selected intent and knows nothing about its producer. Selection priority and stale-intent timeout belong to the controller, not the motor.

## Cognition gate

One gate checks `tier >= CognitionTier` before target selection, route planning, autopilot, mission transitions, or expedition reconciliation. Failed checks produce idle/reflex behavior and a diagnostic log. Tier data supplies the cognition unlock; no subsystem bypasses the gate.

## Lance path

Input intent originates at gunner. Owner receives/validates aim and fire intent, checks wards, `Piece`, crew, tame, policy, rate limits, tool tier, pressure, and heat, then resolves hit and drops. Clients never apply authoritative damage.

## Persistence schema

Use namespaced ZDO keys for tier, control flags, machine state, course/target, fuel, pressure, heat, hull, home depot, mission id/timing/seed/route, resolution, and stuck strikes. Cargo uses vanilla `Container`/`Inventory` only after their serialization behavior is verified.

## Tier data

Tier records contain recipe, station, lance/tool stats, hull, hold, mist radius, resistances, cab variant, and `UnlocksCognition`. Adding Tier V must require data/config changes only.

## Camera and expedition

Camera remains local-only behind `Compat`. Embodied mode is the only mode below Tier IV and the only mode with crew. Abstract expedition mode is Tier IV-only, deterministic from world time, world seed, machine ZDOID, and mission index; reconciliation sets `resolved` before item creation.


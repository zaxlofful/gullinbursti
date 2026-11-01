# API notes — M0 reconnaissance

Status: **blocked for signature verification**.

This checkout contains no Valheim installation, `assembly_valheim.dll`, publicized game DLL, Jötunn DLL, or `VALHEIM_INSTALL`. Therefore no method signature below is asserted as verified. Supplying signatures from memory would violate the build brief. Run recon against the exact game build before M1.

## Required inspection set

`Pathfinding`, `HitData`, `IDestructible`, `TreeBase`, `TreeLog`, `MineRock5`, `MineRock`, `Destructible`, `Pickable`, `ItemDrop`, `Inventory`, `Container`, `PrivateArea`, `ZNetView`, `ZDO`, `ZRoutedRpc`, `ZoneSystem`, `Smelter`, `WearNTear`, `StaticTarget`, `Turret`, `Ship`, `ShipControlls`, `Chair`, `Player.AttachStart`, `Player.AttachStop`, `ZInput`, `GameCamera`, and the component implementing Wisplight mist dispelling.

## Verification procedure

1. Set `VALHEIM_INSTALL` to the live install.
2. Copy no DLLs into this repository.
3. Decompile with ILSpy/dnSpy or inspect with a small read-only reflection tool.
4. Record declaring type, visibility, parameters, return type, fields/properties, and version in this file.
5. Confirm multiplayer ownership and RPC behavior in-game.

## Current findings

- No local symbols can be verified.
- `Demister` is only a hypothesis from the brief; not verified.
- `ShipControlls` spelling is a hypothesis; verify exact type name.
- API compatibility target `0.221.13` is unverified locally.
- Any implementation depending on these symbols is deferred.

## External prior art to inspect

- [ImmersiveFirstPerson](https://github.com/Gerominoes/ImmersiveFirstPerson)
- [ValheimRAFT](https://github.com/zolantris/ValheimMods/tree/main/src/ValheimRAFT)


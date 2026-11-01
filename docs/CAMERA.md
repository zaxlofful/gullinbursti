# Camera — M0 verdict

## Verdict

**Feasible in principle; confidence: low until exact assemblies and a live client are available.** The brief requires an enclosed cab, so geometry can provide occlusion. The fragile part is intercepting and restoring vanilla `GameCamera` state without leaving a bad camera after death, wreck, disconnect, or exception.

## Not yet verified

No local `assembly_valheim.dll` exists. `GameCamera` transform flow, full-zoom first-person path, head hiding implementation, and patch-safe interception point remain unknown.

## Proposed approach

- Inspect vanilla `GameCamera` and identify the narrowest late camera-update hook.
- Keep all camera calls behind `Compat/CameraCompat.cs`.
- Save vanilla position, rotation, FOV, near clip, and player visibility state on attach.
- Apply cockpit/ring/free transforms only on the local client; never execute on a dedicated server.
- Restore in `try/finally` plus explicit dismount, death, wreck, disconnect, and destroy paths.
- Use third-person as fallback if required camera symbols are absent.

## Prior art

Inspect [ImmersiveFirstPerson](https://github.com/Gerominoes/ImmersiveFirstPerson) and [ValheimRAFT](https://github.com/zolantris/ValheimMods/tree/main/src/ValheimRAFT) after obtaining source/dependencies. Search results confirm active first-person mods exist, but do not prove compatibility with this game build.


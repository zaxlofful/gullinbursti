# Build Prompt — **Gullinbursti**: a crewed Dvergr-steam harvester for Valheim

> **How to use this document.** Paste it whole into Claude Code, Codex, or a fresh Claude conversation as the task brief. It is written *to the coding agent*, not to you. It is deliberately opinionated: it makes the hard architectural calls up front so the agent doesn't invent worse ones. Section 16 lists the decisions you may want to reverse. Names marked `<>` are placeholders.

> **The one-line pitch:** *For three tiers, you drive it. On the fourth, it drives itself.*

---

## 0. Read this before you write a single line

You are building a BepInEx mod for Valheim. You almost certainly have stale or hallucinated knowledge of Valheim's internal API and of its item and prefab names. **Assume every method signature and every prefab name you "remember" is wrong until you have read it out of the decompiled game assembly or the live prefab list.**

Your first deliverable is **not code**. It is reconnaissance. See §15.

If you cannot find a symbol, **say so and propose an alternative.** Do not invent an API and do not write code that "should work." A mod that compiles against a hallucinated method is worse than no mod.

Along the way, think about what should be in the AGENTS.MD files, and update it as you go. SO that you don't forget what we have done here.

---

## 1. Mission

Build a mod that adds a **buildable, configurable, crewed land vehicle** to Valheim: a steam-driven harvesting machine that players **drive, gun, and ride** — a mining beam on a rotating ring, an armoured cab, a boiler, a cargo hold.

For the first three tiers, **it has no mind.** It is a tool. It cuts what you aim it at, hauls what you load it with, and goes where you steer it. A Gullinbursti parked and unmanned is furniture.

At the **fourth and final tier**, you take it to the Galdr Table and give it an eitr-brain. Then — and only then — it begins to work without you: choosing its own targets, planning its own routes, and eventually leaving on multi-day expeditions and coming home with a full hold.

**Every crew combination is legal**: unmanned, driven, gunned, ridden, or fully crewed. Below Tier IV, at least one of those seats must have a human in it for anything to happen.

Think **Gnomish Shredder / Ulduar siege engine** in silhouette and attitude — cleated wheels or treads, a riveted boiler, a smokestack, exposed gears, pressure gauges, a rotating gun ring — but rendered entirely in Valheim's own material language so it reads as *salvaged Dvergr machinery that a Viking has bolted iron plate onto*, not as a foreign asset from another game.

---

## 2. Design intent & progression

**Read this before you make a single balance decision. It resolves most of them.**

### What this mod is for

> **The Gullinbursti is not an entry ticket. It is an exploitation engine.**

It never lets a player skip a biome they have not beaten. It lets them **industrialize a biome they have.** You break into the Mistlands the hard way — on foot, in the fog, terrified. You claw enough Dvergr machinery out of an Infested Mine to build one. *Then* the world opens up.

Whenever you are unsure whether a feature is too strong, ask: **does this remove danger, or does it remove tedium?** Remove tedium. Never remove danger.

### The three grades of machine behavior — the spine of this design

This taxonomy governs the entire tier structure. Learn it; you will use it to settle a dozen arguments.

| Grade | What it is | Available from |
|---|---|---|
| **Reflex** | Stimulus → response. No goal, no plan, no choice. The boiler burns fuel. The hopper vacuums drops in radius. An unmanned lance fires at whatever is closest and hostile — *the same tripwire circuit as the vanilla Ballista, which is itself a mindless Mistlands-tier device.* | **Tier I** |
| **Governance** | The machine executes a **player-supplied** goal. Hold this heading. Drive to this waypoint. Stop here. **The player supplies the goal; the machine supplies the driving.** | **Tier I** |
| **Cognition** | The machine supplies **its own** goal. It picks targets. It plans routes. It decides the hold is full and it is time to go home. It leaves on missions unsupervised. | **Tier IV only** |

> **The Governor drives. It does not decide.**
> Below Tier IV, a human always chooses *where* and *what*. The machine only ever executes.

This line is the whole design. Guard it.

### The gate: Black Cores, not metal

There is **no new smeltable metal ore in the Mistlands.** Verify in recon, but it is almost certainly true: the Mistlands' tier-defining resources are **Black Cores** (looted from Infested Mines — functionally, *Dvergr machine cores*, and every Mistlands crafting station is built from them), plus **Refined Eitr**, **Black Marble**, **Yggdrasil Wood**, **Soft Tissue**, **Sap**, and **Carapace**.

This is *better* than a metal gate. The unlock narrative becomes: **you crawl into an Infested Mine, loot the Dvergr's own machine cores, and reverse-engineer them into a machine of your own.** Already canon. Better than "I mined some ore." Explains the whole steampunk-Dvergr aesthetic without a word of invented lore.

### The lore — the real myth, recast onto Valheim's own cast

Vanilla Valheim already names real mythic individuals in its flavor text — **Skoll and Hati** is a craftable knife — so this mod does the same: take the *actual* Norse account of Gullinbursti and retell it nearly one-to-one, with every living role recast onto something that already walks, flies, or swarms in Valheim.

**The source myth (Skáldskaparmál):** to settle a wager a liar made with his own head as the stake, the smith brothers **Brokkr** and **Eitri** forged three treasures. Brokkr worked the bellows; Eitri laid a pigskin in the coals and drew out **Gullinbursti** — a living boar, tireless over any ground by day or dark, its golden bristles glowing so its rider never lost the light. A stinging **fly** (the liar in disguise) bit the bellows-worker to spoil the work. The boar was counted the least of the three treasures. Another of them was a hammer.

**The recast:**

| In the myth | In Valheim |
|---|---|
| Brokkr & Eitri at the forge | Named in lore text only (the Skoll-and-Hati precedent). Their living heirs are **the Dvergr** of the Mistlands expeditions. *Eitri is the manuscripts' variant of Sindri — chosen here for the eitr resonance.* |
| The liar and his wager | *"a liar's wager"* — never named. Vanilla never names him either. |
| **The stinging fly that sabotaged the forging** | **The Seekers.** The swarm that overran the Dvergr mines — and this time *the fly has a Queen.* This substitution is the keystone of the whole recast: the myth's insect saboteur, scaled up into the biome's boss. |
| Gullinbursti's glowing bristles | **The Tier II mist lamp.** *"Night is never dark for the rider"* is practically a feature spec. |
| The tireless boar itself | The machine. The Dvergr rebuilt the old pattern in iron and eitr to haul what their expeditions dug. |
| "The least of the three works" | The tablet winks at it — *"one of the others rides the storms above you still."* **Thor's rare storm-sky sighting is vanilla.** |
| Dwarven craft granting it life | **The Galdr Table.** Tier IV. The Dvergr built the body and withheld the spark; you don't. |
| Odin's ravens | **Hugin** announces; **Munin** remembers. Hugin is the vanilla tutorial raven; **verify Munin's in-game presence in recon** — if absent, both lines are Hugin's. |

**And the canon upgrade this buys the gate:** Black Cores are no longer generic Dvergr machine parts. **They are the eitr-hearts of the dead boars**, still lying where the swarm caught them. Every core in your recipe is the heart of an original. You are not inventing a machine; you are looting a grave and finishing the Dvergr's work.

**Canonical lore text** — ship these; tune freely, but keep the voice: terse, allusive, second-person, never explaining the joke.

*The lore tablet at the Depot* (split across two tablets if long):

> "Two brothers kept a forge, and a liar wagered his head their craft could be beaten. Brokkr worked the bellows. Eitri laid a pigskin in the coals and drew out a boar — gold-bristled, never tiring, swift over any ground by day or dark, its burning back a lamp no night could smother. It was counted the least of the three works forged on that wager. One of the others rides the storms above you still."

> "The Dvergr remembered the pattern. They came to this world to dig, and they forged boars of iron and eitr to carry what they tore from the deep. But a fly spoiled the first forging, and flies ended the last — the swarm found the mines, and the swarm had a Queen. The forges went dark. The boars went still. Their hearts are down there yet."

*Per-tier descriptions:*
- **I — Salvaged:** "Hammered together after the old Dvergr pattern. It does not tire. It does not think. Both of those are your job."
- **II — Eitr-Lance:** "Its bristles burn again. Night is never dark for the rider."
- **III — Flametal:** "Shod and plated for the burning lands. The boar walks where you cannot."
- **IV — Awakened:** "The Dvergr built the body and withheld the spark. You did not. It knows the way home now. It knew before you did."
- **Depot:** "A berth, a crane, and a trough of coal. Even an iron boar comes home to eat."

*Hugin, on first Tier I placement:* "A boar of iron! The Dvergr forged these to haul what they dug — before the mist took the mines. Mind whose pattern you're borrowing, hm?"

*Munin, on first Tier IV awakening:* "The old smiths gave the first boar its own spark. The Dvergr never dared. You just did. I will remember this, whichever way it goes."

*Dream, first sleep after awakening one:* "You dream of a bellows breathing on its own, and of something in the dark of the hold turning over, getting comfortable."

**Delivery channels** — all additive flavor; none may gate function; `Lore.Enabled` config, default true:
1. **Item & piece descriptions** — Jötunn `LocalizationManager`. Zero risk. Ships with M5.
2. **The Depot lore tablet** — the vanilla readable-runestone mechanism (`RuneStone`; verify the component and how custom text registers — recon).
3. **Hugin/Munin lines** — verify the vanilla tutorial-raven trigger path (`Player.ShowTutorial` / `Tutorial` keys) and whether custom entries register (recon). Fallback: a one-time `MessageHud` center-screen line, which is vanilla idiom.
4. **The dream** — verify whether the sleep `DreamTexts` list accepts additions (recon). If hostile, cut it silently; it is a garnish.

### The four tiers — **force → sight → shelter → mind**

Tiers I–III are forged at the **Black Forge** (Mistlands metalwork station; Black Marble + Yggdrasil Wood + Black Cores; confirm it services Ashlands gear too).

**Tier IV is not forged. It is *conjured*, at the Galdr Table** — the Mistlands *magic* station. The machine's body is metalwork. Its mind is not. That structural split is the point, and both stations already exist in the game.

| Tier | Station | Gate | Lance cuts | The gift |
|---|---|---|---|---|
| **I — Salvaged Gullinbursti** | Black Forge | Black Cores + Black Forge. *You have a foothold in the Mistlands.* Body: Black Cores, Iron, Black Metal, Yggdrasil Wood, Sap, Fine Wood. | Wood, stone, copper, tin, iron, silver | **FORCE.** A sustained beam that fells a tree in seconds with no stamina cost. A hopper that vacuums every drop in radius. A hold that carries ten times what you can. **You still do the work — you just do it with a machine instead of a hand tool.** That is the entire Industrial Revolution in one build, and it is worth every Black Core. |
| **II — Eitr-Lance Gullinbursti** | Black Forge | Refined Eitr (⇒ a working Eitr Refinery) + Carapace + more Black Cores | + Black Marble, Soft Tissue, Yggdrasil Wood | **SIGHT.** Mounts the mist lamp (§8.9). Now it works *in* the Mistlands — and makes the Mistlands **navigable**. Green eitr-arc beam. |
| **III — Flametal Gullinbursti** | Black Forge | An Ashlands foothold: Flametal + Ashlands materials. *(Verify the real names in recon — do not guess.)* | + Flametal veins | **SHELTER.** Heat-shielded hull; crew shielded from the environment. A mobile bunker you *ride* into the Ashlands instead of walking into it. |
| **IV — Awakened Gullinbursti** | **Galdr Table** | Ashlands endgame: heavy **Refined Eitr**, more **Black Cores**, Flametal, and an endgame trophy. *(When Deep North lands, re-gate this to Deep North materials via config, or add a Tier V.)* | *(no change)* | **MIND.** Cognition. The mission state machine. Target selection. Route planning. Dispatch. And eventually the offline expedition (§5). **This is where the machine stops being a tool and starts being an agent.** |

### Why Tier IV is last, and why that matters

Automation is the **reward for having done it by hand.** For three tiers, you and your friends *are* the machine's mind — you spot, you steer, you aim, you decide when to go home. Tier IV is the moment you're no longer needed.

That earns it emotionally. It also earns it mechanically: **§5 (the hardest engineering problem in this document) is now a Tier IV concern, which means it ships last.** The mod is complete, playable, and good *without ever solving it.* Do not undo this by building the autopilot early.

### The guardrail

The machine must never make a biome **safe**. Seekers, Gjalls, Morgens, Fallen Valkyries and Charred must remain a genuine threat *to the tank*. It changes tedium and mobility, not danger. **If a player can park a Tier IV in the Ashlands and go AFK, you have failed the brief.**

Your throttles: **fuel burn, boiler pressure, hull integrity, lance tool-tier, and the driver's restricted field of view** (§8.10). Use them. Do not add new ones.

---

## 3. Environment — verify, do not assume

| Thing | Current value (verify at build time) |
|---|---|
| Game version | `0.221.13` line (post–*Call to Arms*). Read the real version from the running game. |
| Engine | **Unity 6000.0.61f1** (bumped Feb 2026). Any AssetBundle must be built with the *exact* editor version. |
| Loader | **BepInExPack Valheim** (`denikson-BepInExPack_Valheim`), BepInEx 5.4.x line, HarmonyX bundled. Pin the current Thunderstore version; do not guess. |
| Framework | `netstandard2.0` / .NET Framework 4.6.2-era Mono. Constrained C# — no records, no top-level statements, careful with newer LINQ. |
| Modding lib | **Jötunn (JVL)** — NuGet `JotunnLib`, currently ~`2.29.x`. Use it. Do not hand-roll prefab registration, config sync, key hints, or localization. |
| Asset system | Valheim uses **Soft Referenceable Assets** (since 0.217.40). Jötunn abstracts this. Do not fight it. |
| Publicizer | Use the Jötunn mod stub's `DoPrebuild.props`. Never commit publicized DLLs or any game DLL. |

> ⚠️ **Timing hazard.** Valheim 1.0 / *Deep North* ships **September 9, 2026** and will very likely break assembly references, possibly bump the engine again, and add a biome. Keep all game-API touchpoints behind thin adapters (§8.15), keep Harmony patches minimal, and do not hard-code prefab or material lists a new biome would invalidate. **Assume a Tier V will be wanted in September** — the tier system must be data-driven, not hard-coded.

> 🐧 **Linux/Proton note.** The game data directory was recently renamed `valheim_Data` → `Valheim_Data`; case-sensitive filesystems care. Build scripts must not assume Windows paths. Drive the install location from `VALHEIM_INSTALL` / `Environment.props`, and make the post-build deploy step a no-op if it isn't set.

---

## 4. Prime directives (non-negotiable)

1. **The ZDO is the single source of truth.** All persistent state — tier, mission state, crew, governor course, target, cargo, fuel, boiler pressure, lance heat, hull, home depot, mission ledger — lives in the `ZDO`. The `MonoBehaviour` is a *stateless executor* that reads the ZDO, acts, and writes back. Ownership must transfer mid-operation with another peer resuming exactly where the last left off. **This is required from day one for multiplayer crew, long before any automation exists.**
2. **Only the owner simulates.** Guard every mutation with `nview.IsValid() && nview.IsOwner()`. Other peers render and interpolate.
3. **The vehicle's ZDO owner is the sole damage authority — always.** Including when a *different* player mans the lance. The gunner sends input; the owner raycasts, resolves, applies damage, inserts drops. Client-side damage application is how you get item duplication and cheating. No exceptions.
4. **There is exactly one drivetrain and exactly one lance. Everything is an *intent producer*.** The human driver produces `DriveIntent` from `ZInput`. The Governor produces `DriveIntent` from a player-set course. The Tier IV autopilot produces `DriveIntent` from a plan. **Build this seam in M2, before any automation exists.** If you build the manual drivetrain without it, you will rewrite the entire locomotion system at M7. This directive is now *more* important than it was when automation shipped first, not less.
5. **Headless-safe.** No code path may assume `Player.m_localPlayer` exists, that there is a camera, or that anything renders. **Applies with particular force to §8.10 and §8.11** — camera and instrument code must never run on a dedicated server.
6. **Determinism across peers.** Anything resolved probabilistically (offline loot, hazard rolls) must be seeded from stable inputs (world seed + machine ZDOID + mission index), never from ambient `UnityEngine.Random` state.
7. **Kitbash first, custom art last.** You cannot author 3D meshes. Ship a complete, good-looking machine assembled from vanilla prefab parts (§8.1). Leave a seam for a custom AssetBundle later. **Do not block delivery on art you cannot make.**
8. **Tiers are data, not code.** Tool tier, hull, hold size, lance stats, mist radius, resistances, recipe, cab variant, **and whether cognition is unlocked** — all per-tier config records. Adding Tier V in September is a config edit, not a refactor.
9. **Cognition is gated at the tier boundary, in one place, and it is checked.** No autopilot, target selector, route planner, or mission state may execute unless `tier >= CognitionTier`. Assert it. A Tier I machine that starts thinking is a bug, not a feature.
10. **State is shown, not printed.** If machine state can be a physical gauge, needle, glow, or sound, it must be — not a floating HUD element. See §8.11.
11. **Never grief.** Never damage player builds, never harvest inside a warded area without access, never destroy anything with a `Piece` component. A *player-aimed* mining beam makes this ten times more important. See §8.16.
12. **Composition over patching.** Every Harmony patch needs a comment justifying *why no non-patch approach exists*. **§8.10 is the one place a substantial patch is pre-authorized** — and it is the riskiest thing in this document.

---

## 5. The hard problem — and why you are not solving it yet

Valheim **does not simulate objects in unloaded zones.** No `Update()` runs. "Send the tank away and it gathers while you do other things" naïvely produces a tank that freezes solid the moment the player walks away.

**This is a Tier IV problem. You will solve it at M9, not M2.** The tier restructure bought you that deferral — take it. Design the architecture so it *can* land (Directives 1, 2, 6), and then build everything else first.

When you get there:

**Mode A — Embodied** (zone loaded, a peer owns the ZDO). Real physics, real pathfinding, real hit resolution. **This is the only mode that exists below Tier IV, and the only mode that can ever run with crew aboard.**

**Mode B — Abstract / Expedition** (Tier IV only; zone unloaded; no crew). The machine is not simulated. A **mission ledger** persists in the ZDO:

```
missionId, departTime(worldSeconds), routePlan, targetProfile,
plannedDuration, fuelLoaded, expectedYieldTable, hazardProfile, resolved(bool)
```

**Reconciliation** runs when a peer loads the zone and takes ownership — or, for a machine dispatched *from* a depot, the moment the **depot's** zone loads (the depot is at the player's base, so it is the reliable anchor):

- Compute `elapsed = ZNet.GetTimeSeconds() - departTime` using **world time, not wall-clock time.** World time only advances while the world is running. This is the correct, non-exploitable choice: a wall-clock timer would let anyone farm by editing their system clock, and would mean the machine "works" in a world nobody is playing. Say so in the README; players will ask.
- Advance deterministically: fuel burn, distance along the route plan, yield rolled from the seeded table, hazard rolls (ambush → hull damage, cargo loss, possible total loss).
- On completion, the machine **arrives home**: re-instantiate at the depot with its cargo, set `resolved = true` atomically (owner-only, same ZDO write), deposit or hold per config.
- If incomplete, re-materialize mid-route at its interpolated position and hand back to Mode A.

**Crew ⇒ always Mode A.** Any player aboard — including a passenger — collapses the abstract mission into concrete state at the machine's interpolated position. Dispatching with crew aboard is an error, surfaced clearly.

**A passenger anchors Mode A.** Their presence keeps the zone loaded, so the machine genuinely simulates. Riding along *is* how you watch it work.

---

## 6. Design brief — the machine

**Silhouette:** low, broad, heavy. Wider than tall. Front-heavy. Rear boiler with a smokestack. A rotating gun ring on top. It *shoulders through* terrain; it does not dart.

**Materials (all vanilla-sourced):** riveted bronze and black-iron plate, copper piping, exposed brass gearing, tarred wood decking, leather strapping. **Green eitr glow** from the furnace vents, the lamp housings, and the lance emitter — Valheim's canonical "advanced technology" light. Dvergr, not sci-fi.

### The three stations must be legible from outside — and *built*, not implied

- **Driver's cab — an actual enclosed volume.** Not a seat on a deck. **Walls, a roof, an instrument panel, and a narrow forward vision slit.** Build it from solid volumetric pieces (iron plate, wood beams), never flat planes, because **that geometry is what does the occlusion in §8.10.** You are not going to *program* "the driver can't see out." You are going to *build a box with a slit and put the camera inside it*, and let the mesh do the work.
- **Gunner's ring** — rear, elevated, an open rotating cupola with the lance emitter. Exposed. The gunner sees everything and can be *hit* by everything. That trade is the point.
- **Passenger bench** — an open riding deck with grab-rails under the lip of the armour. Somewhere a player will *want* to sit. This is the mod's screenshot.

### Tiers must be visually distinct — and Tier IV must be an *event*

Players want to *see* their upgrade.

- **I:** scavenged iron and wood. Honest, ugly, wonderful.
- **II:** eitr-green conduit, glowing seams, the mist-lamp housing.
- **III:** heat-shield plating, a sullen red glow.
- **IV:** **the cogitator.** A slowly-turning eitr-lit core visible through a grille in the hull, and a dial on the panel that has no mechanical function — it is watching something. When the machine runs unsupervised, it should *look purposeful* in a way it never did before. Add a new gauge for it (§8.11) that does nothing except tell you the thing is thinking.

**Once it has a mind, an unmanned Gullinbursti is no longer furniture.** Sell that.

### It must feel alive on idle

- Smokestack particles — thin idle wisp, thick black plume under load, sputtering when fuel is low.
- Rotating gear meshes and a reciprocating piston keyed to speed.
- Steam hiss on stop. Metallic groan on turn. Chugging that pitches with throttle.
- Furnace-vent glow that pulses with load and dies when the fire does.
- The lance emitter glows hotter as heat accumulates and force-vents in a plume on overheat.
- **Working instruments.** See §8.11 — a subsystem, not decoration.

**Scale:** ~1.5× a cart; 2.5–3m at the stack, taller at the gun ring. An event when it rolls up. Small enough to path through a Black Forest.

---

## 7. Behavior

### Two orthogonal axes. Do not conflate them.

**Axis 1 — Control mode** (flags in the ZDO, *not* mission states):

```
driverManned : bool    gunnerManned : bool    passengers : long[]
governorActive : bool  missionSuspended : bool
```

**Axis 2 — Machine state** (the enum below).

Crew occupancy **does not add states.** It *suspends*. Mission state, target, route and ledger are preserved verbatim while a driver is aboard, and resumed on dismount (§8.7). If you add a `BeingDriven` value to the enum, you have modelled it wrong.

### Machine states

One enum in the ZDO. **The cognitive states are unreachable unless `tier >= CognitionTier` — assert this** (Directive 9).

| State | Tier | Enters when | Exits to |
|---|---|---|---|
| `Idle` | I+ | Placed, or operation complete. Parked. **Reflex only** — boiler burns, hopper vacuums, lance auto-defends. It does not move and it does not choose. | `Governed` (player sets a course) |
| `Governed` | I+ | Player sets a heading or waypoint course (§8.2). The machine drives it. **The player chose the destination.** | `Idle` (arrived / cancelled), `Stranded` |
| `Provisioning` | **IV** | Dispatched. Draws fuel; validates; **rejects if crew aboard**; writes the mission ledger. | `Seeking`, or `Idle` (on failure — surface *why*) |
| `Seeking` | **IV** | Has capacity & fuel. **Chooses** the highest-priority valid target; paths to it. | `Harvesting`, `Wandering`, `Returning` |
| `Wandering` | **IV** | No target in scan radius. Executes a self-planned route to find new ground. | `Seeking`, `Returning` |
| `Harvesting` | **IV** | Arrived at a self-chosen target. Aims the lance and cuts (§8.8). | `Collecting`, `Seeking` |
| `Collecting` | **IV** | Node yielded drops. *(Note: the hopper vacuum is reflex and runs at all tiers — this state is only about the machine* choosing *to wait for drops.)* | `Seeking`, `Returning` |
| `Returning` | **IV** | **Decides** the hold is full ∥ fuel low ∥ hull low ∥ recalled ∥ mission expired. Paths to depot. | `Depositing`, `Stranded` |
| `Depositing` | **IV** | At depot. Transfers cargo; optionally auto-refuels. | `Idle`, `Provisioning` (auto-redeploy) |
| `Stranded` | I+ | Out of fuel ∥ unreachable ∥ stuck past recovery budget. Distress beacon. Minimap pin. **Can always be crewed and driven home manually.** | `Idle` (rescued) |
| `Wrecked` | I+ | Hull ≤ 0. Lootable wreck with a configurable fraction of cargo + salvage. **Ejects all crew cleanly first.** | *(terminal)* |

**Below Tier IV the whole machine is four states: `Idle`, `Governed`, `Stranded`, `Wrecked`.** That is a small, testable, shippable system, and it is a *complete game loop*. Build it, prove it, and only then teach it to think.

**Stuck recovery is a first-class feature** at every tier. Progress watchdog → reverse-and-reangle → alternate path → widen agent radius → escalate → `Stranded`. Log every escalation. And note: **a human driver is the ultimate unstick** (§8.7).

---

## 8. Subsystems

### 8.1 Prefab & visuals — the kitbash

Use Jötunn's `KitbashManager` / `PrefabManager`. Do **not** ship a custom AssetBundle in the first release.

Recon: dump the vanilla prefab list and *find the real names*. Candidates:

- **Dvergr props** (`dvergrprops_*`) — pipes, lanterns, crates, barrels, metal frames. Your primary steampunk vocabulary.
- **Blast furnace** — riveted plate, copper venting, industrial mass.
- **Windmill** — gearing and rotating assemblies. **Also your gauge needles** — find out how its blades are driven.
- **Cart (`Vagon`)** — wheels, axles, the wooden chassis idiom.
- **Charcoal kiln / smelter** — boiler forms, ember glow, smoke emitters (steal the particle systems wholesale).
- **Ballista (`Turret`)** — the gun ring, the rotating mount, and the reflex-defence circuit (§8.8).
- **Iron/wood building pieces** — **the cab must be built from solid volumetric pieces** so it occludes from the inside (§6, §8.10). If a candidate piece is single-sided, discard it.
- **Eitr refinery / sap extractor / wisp torch / Galdr table** — glowing tubing, the green light, the lamp housing (§8.9), **and the Tier IV cogitator core.**

Shaders: reuse the game's own (`Custom/Piece`, `Custom/Creature`, …) via Jötunn's shader-replacement system. Bundled shaders render magenta.

Leave a documented seam — one prefab-construction entry point. **Tier visual variants** and **instrument transforms** (§8.11) hang off it.

### 8.2 Locomotion, drive intent & **the Governor**

#### The intent seam — build this in M2 or regret it in M7

```csharp
struct DriveIntent { float Throttle; /* -1..1 */ float Steer; /* -1..1 */ bool Brake; }

interface IIntentProducer { DriveIntent Produce(MachineContext ctx); }
```

Exactly one producer is active at a time:

| Producer | Tier | Source of the goal |
|---|---|---|
| `HumanDriver` | I | The player's hands, via `ZInput` |
| `Governor` | I | A course the **player set** |
| `Autopilot` | **IV** | A plan the **machine made** |

**The motor consumes `DriveIntent` and nothing else. It never knows who produced it.** This is Directive 4 and it is the single most important structural decision in the codebase: it is what lets the Tier IV mind slot in at M7 without touching a line of locomotion code.

#### The Governor (Tier I) — *the thing that makes this mod work solo*

A mechanical speed-and-course regulator. Period-correct — a flyball governor is a Victorian steam-engine part — and mechanically essential:

**Without it, a solo player has a two-person vehicle and no second person.** They can drive *or* gun, never both. The Governor is what lets one player set a course, climb into the ring, and cut.

- **What it does:** hold a heading at a set throttle; or follow a short list of player-placed waypoints; or hold position. Use `Pathfinding.instance` to *get to* a waypoint — navigating to a point a human chose is not cognition.
- **What it does not do:** choose a destination, choose a target, avoid anything cleverly, or decide to come home. It is **dumb.** Set it going and climb into the gun ring, and it will happily drive into a tree and get stuck, and you will have to climb back down.
- **That friction is the point.** It is what makes Tier IV feel like liberation rather than like a bigger number.
- **"Come home" works below Tier IV** — but only because *you* set the depot as a waypoint. The machine deciding *by itself* that the hold is full and it is time to go home is Tier IV. Guard that line.
- Set the course from the driver's seat, via a physical lever/dial in the cab (§8.11), not a menu.

#### The motor

- Rigidbody-driven, never `transform.position` teleporting. Study `Vagon` (cart physics) and `BaseAI`/`MonsterAI` (steering, ground handling).
- **`Pathfinding.instance`** — Valheim's own A* with an `AgentType` enum (`Humanoid`, `TrollSize`, `HugeSize`, …). Verify the enum and the `GetPath`/`FindPath` signature in the decompiled source. Pick the largest plausible agent type; config it.
- Physical refusals — slope limit, water depth limit (it is not a boat), forward raycast fan. **The human driver gets the same refusals.** Those are machine limits, not AI limits.
- Query ground via `ZoneSystem`/`Heightmap` helpers, not raw raycasts alone.
- Top speed scales with boiler pressure (§8.5).
- **Expose chassis pitch and roll as a clean signal** — §8.10's camera and §8.11's instruments both need it.

### 8.3 Harvesting

Where naïve implementations fail. **At Tiers I–III a human aims the lance at a target a human chose.** The *hopper* (the vacuum sweep) is reflex and runs at every tier. The *target selection* is cognition and is Tier IV only (§8.12).

- **Damage-based nodes** (trees, stumps, ore, rocks): construct a `HitData` and call `IDestructible.Damage(...)`. It must carry:
  - the right channel — `m_damage.m_chop` for wood, `m_damage.m_pickaxe` for stone/ore;
  - **`m_toolTier` ≥ the target's `m_minToolTier`**, or the hit *silently does nothing*. This is the mechanism the entire tier system rides on.
  - sane `m_point`, `m_dir`, `m_attacker`, `m_hitType`. Be deliberate — some hit types feed player skill XP. You probably do not want the lance levelling the gunner's Woodcutting. Config, default off.
- **Trees are two-stage.** Killing a `TreeBase` spawns a `TreeLog` — a physics trunk that must be chopped *again* to yield wood, and which rolls downhill. **A human gunner will discover this in the first thirty seconds and it will feel awful if the lance can't cut logs.** Handle `TreeLog` as a first-class target.
- **Ore deposits (`MineRock5`)** are segmented — damage lands on *sub-areas* via specific colliders. **The lance's raycast must resolve to the correct sub-area collider**, or beaming a copper vein does nothing and you won't know why. Older `MineRock`/`Destructible` nodes (tin) differ. Handle both.
- **`Pickable`** (berries, mushrooms, flint, thistle) are *picked*, not damaged. Invoke the `Pick` RPC via its `ZNetView` rather than `Interact()` (which wants a `Humanoid` you may not have).
- **The hopper (reflex, all tiers):** sweep for `ItemDrop`s in radius — `Physics.OverlapSphere` on the item layer → `ItemDrop` → `nview.IsValid()` → **`ClaimOwnership()`** → `Inventory.AddItem(...)` → `ZNetScene.instance.Destroy(...)`. Failing to claim ownership before destroying is how you get duplication and ghost drops. **Runs on the owner regardless of who is aiming the lance.**

**The hopper alone justifies Tier I.** Every Valheim player has spent hours chasing scattered wood across a hillside. Make it hoover.

### 8.4 Cargo & the Depot

- A `Container` + `Inventory` on the chassis. Slots per tier; stack behavior from config.
- **"Full"** = no empty slot, OR weight ≥ capacity, OR fill ratio ≥ threshold. **Below Tier IV, "full" is just a readout — the machine does nothing about it.** *You* notice and *you* drive home. At Tier IV it becomes a trigger.
- **Crew can open the hold in transit** (`CrewCanAccessHoldInMotion`, default true).
- **The hold fills visibly** (§8.11) — the best instrument in the machine.
- **The Depot** — a buildable docking bay / crane gantry. It holds the linked output container(s), the fuel supply, and (at Tier IV) the **mission ledger anchor** (§5). Below Tier IV it is a garage: refuel, unload, repair. At Tier IV it becomes a dispatch bay.
- Store the depot link in the machine's ZDO as a `ZDOID`. Handle depot destruction mid-operation.
- Many machines per depot; many depots per world. **No singletons.**

### 8.5 Fuel & the boiler

Study `Smelter` for the vanilla fuel loop.

- Fuel items configurable; default **Coal**, with **Surtling Core** as a high-efficiency option.
- Burning fuel produces **boiler pressure** — the machine's real currency, powering the drivetrain *and* the lance (§8.8) from one shared pool (`SharedBoilerPressure`, default on).
- Burn rate scales with load: idle < driving < cutting. Pressure regenerates as fuel burns and drains as spent.
- Low pressure → reduced top speed and slower lance ticks. Zero fuel → `Stranded`.
- **Below Tier IV, nothing warns you but the gauge.** Run it dry in the Swamp and you walk home. That's the deal, and the rescue trip is *good* gameplay. At Tier IV the machine gets a reserve calculation and comes home on its own — and **that calculation must account for distance home**, or it will strand itself constantly.

### 8.6 Survivability & threat

- A hull/HP pool (`WearNTear`-style); a legitimate aggro target for mobs (`StaticTarget` — verify). A machine trolls politely ignore is not interesting, and it is precisely why the gunner's seat exists.
- Config per tier: hull HP, damage resistances, `Invulnerable` (off by default).
- `Wrecked` **ejects crew cleanly** (`AttachStop` first, *then* swap in the wreck). Never trap or kill a player inside a destroyed vehicle. Wrecks return a configurable fraction of build materials — losing one should hurt, not be ruinous.
- **Crew exposure differs by station, and that is a designed trade.** The driver is enclosed and hard to hit. The gunner is in an open ring and *can* be hit. Passengers are behind armour but exposed from above. Verify the colliders actually make this true.
- **Below Tier IV, an unmanned machine has only reflex defence.** Park it in the Mistlands and come back to a wreck. Park it inside your walls and it's fine. That tension is intentional.

### 8.7 Crew & stations

**Three station types.** All `Interactable` attach points. All independently occupiable. **Every combination is legal.**

#### Driver's station
On interact: attach **and claim ZDO ownership of the vehicle.** Read `ShipControlls` (yes — misspelled with two Ls in the game source; that is the real class name, do not "fix" it) to see how Valheim's only vanilla vehicle does this. The driver becomes the simulation and damage authority. Camera: **cockpit** (§8.10). Also operates the **Governor** controls (§8.2).

#### Gunner's station
On interact: attach. Does **not** claim ownership. Sends throttled aim/fire input RPCs to the owner (§8.8). Camera: **over the ring**, free 360° (§8.10).

#### Passenger station — *the one that changes nothing*
`PassengerSeats` (config, default 2). Attach. **No ownership claim. No automation suspended. Zero behavioral effect.** Camera: **free** (§8.10). Can open the hold, read the instruments, hop stations.

**Three reasons it is load-bearing, not decoration:**
1. **It is the crew's eyes.** Because of §8.10, the driver is half-blind. The passenger is the lookout. That is a *job*, not a hammock.
2. **It is the delivery mechanism for Tier III.** A heat-shielded tank with a passenger seat is a **mobile bunker** — you *ride* into the Ashlands.
3. **It anchors Mode A** (§5). A passenger keeps the zone loaded, so a Tier IV machine genuinely simulates while you watch.

#### The control matrix

**Tiers I–III:**

| Driver | Gunner | Drivetrain | Lance |
|---|---|---|---|
| — | — | **Parked**, or executing a player-set Governor course | **Reflex defence only** |
| **Player** | — | **Manual** | Reflex defence only |
| — | **Player** | Parked, or Governor course | **Manual** |
| **Player** | **Player** | **Manual** | **Manual** |

**Tier IV** adds a row of behavior to every cell: an unoccupied station's automation takes over.

| Driver | Gunner | Drivetrain | Lance | Mission |
|---|---|---|---|---|
| — | — | **Autonomous** | Auto: defend + mine | **Running** |
| **Player** | — | Manual | Auto-defend only *(auto-mining OFF)* | **Suspended** |
| — | **Player** | **Autonomous** | Manual | **Running** |
| **Player** | **Player** | Manual | Manual | **Suspended** |

**Passengers are orthogonal to both tables. They never change any cell.**

Rules:
1. **Occupying a station suspends that station's automation, and nothing else.**
2. **A driver additionally suspends the mission** (Tier IV) — a human at the levers has taken the decision about where to go. (`DriverSuspendsMission`, default true.)
3. **With a driver aboard, an unmanned lance keeps auto-defending but stops auto-mining** — otherwise it swings off to chew a tree while you're trying to drive somewhere.
4. **A gunner alone at Tier IV is "escort mode"** — the machine plans and drives its own route while a human rides shotgun. **Below Tier IV the Governor is the poor man's version of this, and it is what makes the mod playable solo.** Both must work.

#### Suspension is non-destructive
Preserve mission state, target, route, ledger. On driver dismount: resume if `ResumeMissionOnDismount` (default true) and the mission is still valid; else `Idle`.

**This is your stuck-recovery escape hatch.** A player walks out to a wedged machine, climbs in, reverses it out, climbs out, and it continues. Put it in the README — it turns the worst failure mode into a feature.

#### Attach-point physics is the hard part
Attaching players to a moving Rigidbody in Valheim is a known pain — sliding off, jitter, desync, camera fighting the parent transform. **Read how `Ship` parents its passengers, and how ValheimRAFT handles moving platforms, before you design this.** Acceptance test: a 30° Black Forest slope at full speed with three players aboard.

#### Controls
Bind via Jötunn's `InputManager`; surface via `KeyHintManager`.
- **Driver:** throttle, steer, brake, **freelook (hold)** (§8.10), **Governor engage / set course**, dismount.
- **Gunner:** mouse aim, hold-to-fire, dismount.
- **Passenger:** dismount.

### 8.8 The Lance — turret, weapon, and mining beam

**The lance is the machine's primary implement, not an accessory.** Both the harvest tool and the weapon. **Do not build a separate harvest arm** — the thing that cuts trees is the thing the gunner shoots with.

**What it is:** a short-range, sustained cutting beam — a superheated steam / eitr arc lance. It *sustains*. It does not burst.

#### Damage model — read this twice

**Valheim has no continuous damage system.** Damage is discrete `HitData` applications, each crossing the network. A naïve `Damage(dps * Time.deltaTime)` every frame will flood the network and melt a dedicated server.

- **Decouple the visual from the damage.** The beam *renders* continuously (LineRenderer / stretched particle + impact VFX + light). It *ticks* damage on a fixed interval — default **5 Hz**, config `LanceTickRate`. Every damage value in config is **per tick, not per second.** Say so in the config description.
- Each tick builds one `HitData` with **both payloads**:
  - `m_damage.m_pickaxe` and `m_damage.m_chop` — the **mining** payload. Meaningful. This is what the lance is *for*.
  - a **small** elemental payload against creatures — `m_fire` by default. **Small.** Poor at killing a Seeker; excellent at cutting rock. A tool that hurts, not a weapon that trivialises combat. **If a player can solo a boss with the lance, you have failed the brief.**
  - `m_toolTier` — **the tier gate.** A Tier I lance cannot scratch Black Marble. A Tier II lance cannot touch Flametal.
- **Damage is always applied by the vehicle's ZDO owner**, never the gunner's client (Directive 3). One peer, one authority, no duplication.

#### Aiming & hit resolution
- Raycast from the emitter, `LanceRange` (default ~15m), against a mask including terrain, static solids, destructibles, characters — **excluding the vehicle's own colliders and all crew.**
- First hit wins. No penetration, no chaining (v1).
- Yaw 360°. Pitch clamped (default −15° to +55°). Hard-block firing into its own chassis, cab, or passengers.
- Gunner aim is **locally predicted** client-side and sent to the owner at ~20 Hz, which is authoritative and replicates via ZDO. A frame or two of latency is invisible on a sustained beam.
- **Do not build nested `ZNetView`s for the turret** unless recon proves otherwise. RPC-to-owner is simpler and keeps authority in one place.

#### Boiler pressure & heat — the crew tension mechanic
- Firing **drains boiler pressure** (§8.5) — the same pool the drivetrain runs on. A gunner who fires nonstop slows the machine and can strand it. **And the driver watches the needle drop on the panel in front of them** (§8.11). *The driver and the gunner spend the same resource, and they can see each other doing it.* This is the best mechanic in the mod. Do not lose it.
- Sustained fire builds **heat**. At the cap the lance **force-vents**: steam blast, plume, forced cooldown. Config-gated.

#### Reflex defence (all tiers) vs. cognition (Tier IV)
- **Reflex (Tier I+):** an unmanned lance fires at the *closest hostile in range*. No prioritisation, no interruption logic, no pursuit. **A tripwire.** Justify it out loud: the vanilla Ballista is a mindless Mistlands-tier auto-turret, so this is not a tier violation. `LanceReflexDefence`, default on.
- **Cognition (Tier IV):** target prioritisation, threat assessment, interrupting a harvest to deal with a threat and then *returning to the node*, engagement policies (`Ignore`/`DefendSelf`/`Aggressive`). This lives in §8.12 and **must not execute below the cognition tier.**

Study vanilla `Turret` (the ballista) for acquisition, arc limits, and valid-target filtering.

#### Harvest routing
Whatever the lance cuts, drops feed the hold via the hopper (§8.3) — **including when a human is gunning** (`LanceHarvestFeedsHold`, default true).

#### VFX without art
A `LineRenderer` with a game-sourced additive material, impact VFX kitbashed from existing spark/ember/steam particle systems, a `Light` that brightens with heat. **In recon, look for any existing vanilla continuous or channelled beam effect.** Audio: rising whine on spin-up, steady roar sustained, vent-hiss on release and overheat.

**Everything in §8.16 applies to the lance with extra force.**

### 8.9 Biome adaptation — the mist lamp & heat shielding

**This is where §2 stops being a pitch and becomes code. Not polish.**

#### The mist lamp (Tier II) — the headline feature
The Mistlands is *defined* by its mist; the vanilla counter is the **Wisplight** (dispels fog in a radius around the player). The same mechanic is on **wisp torches** (static) and the **Mistwalker** sword. Find the component — very likely a `Demister` — and **mount one on the chassis** with a config radius.

A **Tier II machine clears a large radius of mist as it drives.** You can ride inside it. The biome's signature problem is solved by *riding*.

- The radius must feel like a **superpower**, not a Wisplight bolted to a truck.
- Green eitr glow on the housing — the visual identity of Tier II.
- **Verify how the demister replicates.** It may be a purely local visual effect — in which case *every client near the machine* must see cleared mist, not just the owner. **Test with two clients. This is the likeliest hidden bug in this design.**

#### Heat shielding (Tier III) — the mobile bunker
The Ashlands does environmental damage; the ground itself is hostile. Tier III gets high fire resistance, a hull that survives ash rain, and **`PassengersShieldedFromEnvironment`** — crew aboard take reduced or no environmental damage. Verify how Valheim applies biome/environmental damage to players, and whether vehicle attachment already exempts them (it almost certainly does not).

#### The line you must not cross
**None of this makes a biome safe.** Mist-clearing and heat-shielding change *tedium and mobility*, never *danger*.

### 8.10 Camera & viewpoint — the cockpit

> **The riskiest section in this document, the most important one for how the mod feels, and the only place a substantial Harmony patch is pre-authorized (Directive 12).**

#### The design goal
You are **in** the machine, not floating above it. A driver sees an instrument panel and a slit of the world ahead. They do **not** get a free orbiting camera revealing everything around the tank.

**This is not a graphics preference. It is the mechanic that earns the crew** — and now that the crew is *mandatory* for three tiers, it is load-bearing for the entire mod.

| Station | Camera | Can see | Their job |
|---|---|---|---|
| **Driver** | **Cockpit** — anchored inside the cab, look angles clamped to the viewport arc | Gauges, levers, a narrow forward slit | Drive. Trust the crew. |
| **Gunner** | Over the ring, yaws with the turret, wide arc | 360° above the hull | Kill and cut — **and be the eyes** |
| **Passenger** | Free third-person | Everything | Spot. Ride. Take the screenshot. |

Three seats become three *jobs*. Without the restriction, the crew is a novelty. With it, the crew is a **team**.

#### How to build it: geometry, not code
**Do not write "the driver can't see out."** Build a cab with walls, a roof, and a vision slit (§6), anchor the camera to a `driverEye` transform *inside* it, and let the mesh occlude. That's the whole trick.

What you *do* have to write:
- **Camera anchoring.** On driver mount, take over `GameCamera` — patch its update, or drive its transform from a `LateUpdate` after it runs — and hard-anchor position to the `driverEye` transform. **Restore cleanly on dismount, death, wreck, disconnect, and unhandled exception. A camera stuck in a destroyed tank is a mod-uninstalling bug.** Write the restore path *first* and test it by killing yourself in the seat.
- **Look clamping.** Clamp yaw to the viewport arc; clamp pitch to a range that **must include enough downward travel to read the panel.** A cockpit you can't look down in is useless.
- **Freelook.** Hold a key to unclamp and glance around the cab without steering. Standard sim convention; the difference between "immersive" and "claustrophobic."
- **Near clip.** The camera sits inside geometry. Low enough that the panel doesn't clip, high enough that you don't see through the walls. Fiddly. Budget time.
- **The player's body.** Valheim is third-person and the player stays embodied — others must see your Viking at the levers (`AttachStart`). For *your own* view, follow vanilla first-person (Valheim already goes FP at full zoom-in; find how it hides the head). Test both a true-FP anchor and a tight over-the-shoulder anchor *inside* the cab; the latter is often more comfortable and still fully occluded.

#### Comfort — take this seriously or you will make people ill
A camera rigidly parented to a Rigidbody bouncing over Valheim terrain is a **nausea machine.**
- Position: parented, with spring damping.
- **Rotation: damped separately, and *not* fully coupled to chassis roll/pitch.** Expose `ChassisRollInfluence` (0 = camera stays level, 1 = fully rides the chassis). **Default ~0.35.**
- FOV, head-bob, shake: all configurable, all disableable.

#### Accessibility — non-negotiable
**`DriverCameraMode` = `Cockpit` | `ThirdPerson` | `Free`.** Default `Cockpit`. But a forced restricted camera earns one-star reviews from motion-sensitive players and they would be right. The opt-out must be prominent, obvious, and lossless: `ThirdPerson` must be fully playable, just less interesting.

The driver is never *truly* blind: **the minimap still works.** Say that in the README; it defuses the main objection before it is raised.

#### Risk
**Valheim has no vanilla precedent for a constrained vehicle camera** — `Ship` does not touch `GameCamera` at all. This is net-new work against a camera system that was not designed for it.

It is provably possible: **first-person camera mods for Valheim exist.** Find one on Thunderstore, read how it patches `GameCamera`, steal the approach. **Report your findings and a straight feasibility verdict in recon (§15) before committing.**

If it proves infeasible, §8.11 is not a consolation prize — it is required anyway.

### 8.11 Diegetic instruments — no HUD that could be a gauge

**Directive 10, made concrete.** Valheim has almost no HUD. The machine follows suit: **its state is displayed on its body, not in an overlay.**

Two audiences, which is why this is required regardless of §8.10:
- **The driver**, who has no HUD and must read the panel in front of them.
- **The player outside**, watching a machine work. **At Tier IV, when nobody is aboard, the exterior instruments *are* the UI.** You walk up to your machine and read its state off its face.

#### The instrument suite
Each is a `Transform`, a material property, or an emitter, driven from **replicated ZDO state** in `Update()` **on every client** — not just the owner. The ZDO already replicates `gb_pressure`, `gb_fuel`, `gb_lanceHeat`, `gb_hull`. Every client renders a correct needle from it with **zero RPCs.**

| Instrument | Driven by | Where |
|---|---|---|
| **Boiler pressure gauge** — needle on a dial | `gb_pressure` | Driver's panel **and** exterior |
| **Fuel gauge** | `gb_fuel` | Driver's panel **and** exterior |
| **Lance heat gauge** — climbs, reddens, *vents* at the cap | `gb_lanceHeat` | Gun ring **and** exterior |
| **Hull condition** — needle, plus visible dents/scorch material swaps | `gb_hull` | Exterior |
| **Throttle lever** — visibly moves | `DriveIntent.Throttle` | Cab (visible through the slit) |
| **Governor dial** — the course-set lever; visibly engaged | `gb_governorActive`, heading | Cab **and** exterior |
| **Cogitator core (Tier IV)** — a slowly-turning eitr-lit core behind a grille; a dial that does nothing except tell you the thing is thinking | `gb_tier`, mission state | Exterior |
| **Flywheel / gear speed / piston stroke** | velocity | Exterior |
| **Smokestack output** | load | Exterior |
| **Warning lamp** — red, blinking, audible: low fuel, overheat, stranded, hull critical | state | Exterior + cab |
| **THE HOLD FILLS VISIBLY** | Container contents | Exterior — see below |

#### The hold is the best instrument in the machine
**Make the cargo physically appear.** Stacked wood, ore and stone meshes populate the hold bed as it fills — swap or enable meshes at thresholds. The most satisfying readout you can build, it needs **no new art** (the item drop meshes already exist), and it means you can see from a hundred metres that your machine is coming home loaded.

Container contents live in a replicated ZDO, so every client renders the same fill level. Cheap. Correct. Do it.

#### The rule
**No UI element may exist that could be a physical gauge instead.** Controls and key hints are fine. *State display* is not. If you are adding a floating pressure bar, go build a needle.

### 8.12 The Mind — cognition, Tier IV, and nothing before it

> **Build this last. Everything above ships without it.**

At the Galdr Table, the player gives the machine an eitr-brain. This section is everything the machine cannot do until they do.

**Gate it in one place, and assert it** (Directive 9):

```csharp
if (Tier < Config.CognitionTier) return;   // no autopilot, no targeting, no missions
```

#### What the mind adds

1. **Target selection.** A scored priority queue: resource whitelist/blacklist, distance, node richness, **whether the lance's tier can actually cut it** (never path to a node you cannot damage — this is what stops a Tier IV machine with a Tier-II lance wandering uselessly), remaining hold capacity for that item type. All weights in config.
2. **Route planning.** Self-planned exploration: expanding spiral, patrol ring, waypoint circuit, ferry. The machine chooses *where to look*, which the Governor never did.
3. **The decision to come home.** Hold full. Fuel below a reserve that **accounts for distance home**. Hull below a limp-home threshold. Mission expired. These are *judgements*, and they are the difference between a tool and an agent.
4. **Dispatch.** The `Provisioning` → `Seeking` → … → `Depositing` loop (§7).
5. **Combat cognition.** Threat prioritisation, interrupting a harvest and *returning to the node*, engagement policy (`Ignore`/`DefendSelf`/`Aggressive`). *Distinct from the Tier I reflex tripwire (§8.8).*
6. **The expedition (§5).** Mode B, the mission ledger, offline reconciliation. **The single hardest thing in this document — and now, safely, the last thing you build.**

#### How it plugs in
It does not touch the drivetrain. It does not touch the lance. It is **one more `IIntentProducer`** (§8.2) plus a target-selection service. **If you built the intent seam correctly in M2, the mind slots in without modifying a line of locomotion or weapon code.** If it doesn't, you built the seam wrong — go fix that, not this.

#### Make it an event
The first time a player's machine drives off without them and comes back with a full hold, that should be a moment they remember. Give the cogitator a startup sequence. Give it a sound. **Once it has a mind, an unmanned Gullinbursti is no longer furniture** — and it should feel slightly, pleasantly unsettling.

### 8.13 Player interaction & UI

What's left after §8.11 has taken everything it can.

- **Hover-interact** on the chassis gives a `Hoverable` readout with *precise numbers* — tier, state, crew, fuel %, pressure %, heat %, hold %, hull %, governor course, (Tier IV) target and ETA. The gauges are the ambient read; this is the exact read. **If the gauges are illegible without this, the gauges are wrong.**
- Interact on the hold opens the container. Interact on a station mounts it. **Governor controls live on a physical lever in the cab, not in a menu.**
- **Minimap pin** tracking the machine live (Jötunn's `MinimapManager`), coloured by state, distinct marker when `Stranded`. Also what keeps a cockpit-bound driver oriented (§8.10).
- **(Tier IV, optional):** a dispatch console at the depot — resource priorities, patrol radius, return threshold, route mode, lance policy. Only after the machine works.
- **Debug:** Jötunn `ConsoleCommand`s behind a config flag: `gb_spawn <tier>`, `gb_state`, `gb_dump`, `gb_force <state>`, `gb_fill`, `gb_teleport`, `gb_mount <driver|gunner|pax>`, `gb_governor <heading|off>`, `gb_lance <on|off>`, `gb_mist <on|off>`, `gb_cam <mode>`, `gb_tier <n>`, `gb_resolve`. These **are** your test harness.

### 8.14 ZDO schema

One static class, stable hashed keys, a `schemaVersion` int, a migration path.

```
gb_schemaVersion   : int
gb_tier            : int      // 1..4 (data-driven; expect a 5 in September)
gb_state           : int      // machine enum; cognitive states illegal below CognitionTier
gb_governorActive  : bool
gb_governorHeading : float
gb_governorWaypts  : byte[]   // player-set course
gb_missionSuspended: bool
gb_driverId        : long     // player ID, 0 = empty
gb_gunnerId        : long
gb_pax0 .. gb_paxN : long     // ZDO has no array type — fixed keys, or a byte[] blob
gb_targetZDOID     : ZDOID    // Tier IV
gb_homeDepot       : ZDOID
gb_fuel            : float    // ← drives a gauge on every client (§8.11)
gb_pressure        : float    // ← ditto
gb_hull            : float    // ← ditto
gb_lanceHeat       : float    // ← ditto
gb_throttle        : float    // ← drives the lever mesh
gb_lanceYaw        : float
gb_lancePitch      : float
gb_lanceFiring     : bool
gb_missionId       : long     // Tier IV
gb_departTime      : double   // world seconds
gb_plannedDur      : float
gb_routeSeed       : int
gb_routeMode       : int
gb_resolved        : bool
gb_stuckStrikes    : int
```

Lance tier, hull max, hold size, mist radius, resistances, cab variant and **cognition unlock** are **derived from `gb_tier` via config**, not stored per-machine. Cargo: let the vanilla `Container`/`Inventory` serialise itself and piggyback.

### 8.15 Version-resilience adapters

Every call into the game's API goes through a thin `Compat/` layer. When 1.0 lands in September and a signature changes, the fix is one file, not forty. **`GameCamera` is the most likely thing to break** — isolate every camera touchpoint behind one adapter. Log one clear error and cleanly self-disable if a required symbol is missing at startup, rather than throwing a wall of NREs into everyone's `Player.log`.

### 8.16 Safety rails — implement these *before* you implement harvesting

Apply to the human-aimed lance, the reflex defence, and (later) the Tier IV mind alike.

- **Wards.** Before damaging *anything*, check `PrivateArea` access at the target position against **the operator's** permissions (the driver/gunner if crewed; the owner otherwise). No access → not a valid target, and the lance does not fire. Verify the real API.
- **Never touch player builds.** Anything with a `Piece` component is not a resource. Hard exclusion for the mind — not a config option. For the lance, `LanceCanDemolish` exists for people who want a demolition tool, but **even when enabled it is unconditionally blocked inside any ward the operator lacks access to. No config overrides that. Ever.**
- **Never damage the vehicle, its cab, its crew, or tamed creatures.** The gunner cannot shoot the passengers or the driver. **The reflex defence cannot either** — test this specifically; a tripwire turret on a vehicle full of people is exactly where friendly fire bugs live.
- **Never target `Character`s** unless the engagement policy permits it. Other players: never, by default.
- **Cultivated ground and planted crops** excluded by default (config to allow).
- **Rate limits.** Hard caps on lance ticks/second, nodes destroyed/minute, drops vacuumed/second. Not player-configurable above a sane ceiling.
- **Deposit sanity.** Never let offline reconciliation run twice on the same `missionId`. `resolved` is set by the owner, in the same ZDO write, *before* any items are created.
- **Camera restore.** On death, wreck, disconnect, or any exception, the camera returns to normal. A safety rail, not a polish item.

---

## 9. Configuration

BepInEx `ConfigFile`, synced via Jötunn's `SynchronizationManager`, `IsAdminOnly = true` on every balance-affecting value. Every number in this document is a config entry. **There are no magic constants in the source.**

- **`Progression`** — **per-tier records** (must be data, not code): display name, recipe, required station, lance tool tier, lance damage, hull HP, resistances, hold slots, mist radius, environmental shielding, visual/cab variant, **`UnlocksCognition`**. Plus `CognitionTier` (default **4**), `RequireMistlandsGate` (true), `AllowEarlyBuild` (false).
- **`Governor`** — enabled (true), max waypoints, max course length, governed top speed (probably lower than manual — a dumb driver should be a *cautious* one), obstacle-stop behavior.
- **`Camera`** — `DriverCameraMode` (`Cockpit` | `ThirdPerson` | `Free`, default `Cockpit`), yaw/pitch clamps, freelook key, `ChassisRollInfluence` (0.35), position/rotation damping, FOV, head-bob, shake, near-clip. **All player-side, none admin-locked** — comfort and accessibility, not balance.
- **`Instruments`** — gauge update rate, visible-cargo enabled, warning-lamp thresholds.
- **`Lore`** — enabled (true); per-channel toggles (descriptions always ship; the tablet, the raven lines, and the dream individually switchable).
- **`Vehicle`** — speed, turn rate, acceleration, max slope, max water depth, agent type, mass.
- **`Crew`** — stations enabled, `PassengerSeats` (2), `DriverSuspendsMission` (true), `ResumeMissionOnDismount` (true), `LanceAutoDefendWhileDriven` (true), `LanceAutoMineWhileDriven` (false), `CrewCanAccessHoldInMotion` (true), `PassengersShieldedFromEnvironment` (tier-gated), allow dispatch with crew aboard (false).
- **`Lance`** — range, tick rate (5 Hz), pickaxe/chop damage per tick, creature damage per tick + type, yaw/pitch limits, pressure cost per tick, heat per tick, overheat threshold, cooldown, **`LanceReflexDefence`** (true), `LanceHarvestFeedsHold` (true), `LanceCanDemolish` (false), grants skill XP (false).
- **`Harvest`** — hopper radius, hopper rate cap, node handling.
- **`Cognition`** *(Tier IV)* — scan radius, resource whitelist/blacklist, priority weights, allow-crops (false), max nodes/min, engagement policy, return-at-fill-%, fuel reserve margin, hull limp-home threshold.
- **`Cargo`** — weight capacity, auto-deposit (Tier IV), deposit target.
- **`Fuel`** — fuel items + efficiencies, tank capacity, burn rates (idle/drive/cut), `SharedBoilerPressure` (true), pressure regen, low-pressure speed penalty, auto-refuel at depot.
- **`Expedition`** *(Tier IV)* — offline mode enabled, max duration, route mode, max range, hazard rate/severity, yield multiplier.
- **`BiomeAdaptation`** — mist lamp enabled, mist radius (per tier), heat resistances (per tier).
- **`Debug`** — verbose logging, gizmos, console commands.

---

## 10. Repository & deliverables

```
/src/Gullinbursti/
  Plugin.cs                    BepInPlugin entry, config binding, Jötunn hooks
  Compat/
    CameraCompat.cs            THE fragile one — isolate every GameCamera touchpoint (§8.15)
    ...                        thin adapters for every other game API
  Progression/
    TierDefinition.cs          per-tier data record, incl. UnlocksCognition (§2, §9)
    Recipes.cs                 Black Forge (I–III) + Galdr Table (IV) registration
  Prefabs/                     kitbash, cab construction, tier variants, instrument transforms
  Components/
    MachineController.cs       the state machine (ZDO-backed)
    Motor.cs                   consumes DriveIntent. Knows nothing about who produced it.
    DriveIntent.cs             the intent contract (§8.2) — BUILD THIS IN M2
    HumanDriver.cs             IIntentProducer — ZInput
    Governor.cs                IIntentProducer — a player-set course (Tier I) (§8.2)
    Crew.cs                    driver / gunner / passenger stations, attach, ownership (§8.7)
    StationCamera.cs           cockpit / ring / free camera per station (§8.10)
    Instruments.cs             gauges, needles, lamps, visible cargo (§8.11)
    LanceController.cs         aim, tick damage, heat, pressure, reflex defence (§8.8)
    Hopper.cs                  the drop-vacuum sweep (reflex, all tiers)
    BiomeAdaptation.cs         mist lamp, heat shielding (§8.9)
    Fuel.cs                    fuel + boiler pressure
    Cargo.cs
    DepotController.cs
  Mind/                        ← TIER IV ONLY. Gated in one place. Built last. (§8.12)
    Autopilot.cs               IIntentProducer — a machine-made plan
    TargetSelector.cs
    RoutePlanner.cs
    MissionController.cs
    MissionLedger.cs           offline reconciliation (§5) — the last thing you build
  Net/                         ZDO keys, RPCs, schema migration
  Config/  Commands/
  Lore/                        localization strings, tablet & raven text, the dream (§2)
/docs/
  API-NOTES.md                 verified signatures from your recon pass
  MATERIALS.md                 verified prefab + item names for every tier recipe
  CAMERA.md                    what you learned about GameCamera, and your verdict (§8.10)
  ARCHITECTURE.md              your design, before you code it
  TESTING.md                   manual in-game test script, incl. solo + 3-player crew tests
/package/                      Thunderstore: manifest.json, icon.png (256x256), README.md, CHANGELOG.md
LICENSE                        MIT
Directory.Build.props / Environment.props   VALHEIM_INSTALL, deploy path
.gitignore                     excludes all game DLLs and publicized assemblies
```

Namespace / GUID: `com.<yourhandle>.gullinbursti`. Name: **Gullinbursti** (Old Norse, *"Gold-bristle"*) — the boar forged by the Dvergr smiths Brokkr and Sindri (the latter also recorded as **Eitri**) in Loki's wager: a beast *built in a forge*, tireless over any terrain day or night, its glowing bristles a light its rider never loses, drawing Freyr's chariot. A Dvergr-manufactured vehicle-animal that carries its own lamp — this machine already had a name, and one of its makers is named after the Tier II resource. *(The earlier working name Járnvagn was dropped: to modern Scandinavian eyes järnväg/spårvagn reads as rail transport.)* Alternates if you dislike it: **Járngaltr** (*iron-boar* — plainly descriptive, and modern Swedish *järngalten* still parses perfectly), **Svaðilfari** (the tireless stallion that hauled the stone for Asgard's wall), **Grotti** (the mill that grinds without rest — better suited as a name for the Tier IV mind or the depot than for the machine itself).

---

## 11. Milestones

**Note the shape of this plan: the mod is a complete, shippable, genuinely good vehicle mod at M6, before a single line of automation exists.** The two hardest, riskiest things — the cockpit camera and offline reconciliation — sit at opposite ends, so they cannot compound.

| # | Deliverable | Done when |
|---|---|---|
| **M0** | Recon: `ARCHITECTURE.md`, `API-NOTES.md`, `MATERIALS.md`, `CAMERA.md`. | **Stop and get review before proceeding.** |
| **M1** | Skeleton: plugin loads, config binds, kitbashed prefab registers (**including a real enclosed cab**), buildable piece places, ZDO persists across relog. | Build it, log out, log in, it's still there — and the cab is a box you could sit in. |
| **M2** | **It drives.** Driver's seat, ownership claim, **the `DriveIntent` seam** (Directive 4), Rigidbody physics, fuel + boiler pressure, **the cockpit camera**, and the core instruments (pressure, fuel, throttle lever). Bulletproof camera-restore path. | Climb in, read the gauges, drive through a slit, die in the seat — camera returns cleanly. **This alone is a fun toy.** |
| **M3** | **It harvests.** Gunner's seat + ring camera. The lance: aim, hold-to-fire, tick damage, tool tier, heat, pressure drain. Trees *and logs*. The hopper. The hold. **Safety rails (§8.16) land here, before harvesting ships.** | Two players: one drives half-blind, one is the eyes and cuts a copse apart. The driver *sees the pressure needle drop* as the gunner fires. **This is a complete, shippable mod.** |
| **M4** | **The Governor + the passenger seat.** A solo player sets a course, climbs into the ring, and harvests. Reflex defence. Passenger station + free camera. | **A solo player can use this machine.** If they can't, the Governor is wrong — fix it before moving on. |
| **M5** | **The tiers (I–III).** Data-driven tier records, Black Forge recipes, tool-tier gating enforced end-to-end, full node matrix (`MineRock5` sub-areas, pickables, stone), wards, minimap pin, hover UI, live config, tier visual variants. **Tier flavor text + localization land here.** | A Tier I lance *cannot* scratch Black Marble. A Tier II lance can. |
| **M6** | **Biome adaptation.** Mist lamp (II) — **verified across two clients.** Heat shielding + crew environmental protection (III). | Ride a Tier II through the Mistlands and *see*. Ride a Tier III into the Ashlands and *live*. **The mod now delivers its entire §2 promise, with zero automation. Ship it here if you must.** |
| **M7** | **Tier IV: the Mind (Mode A).** Galdr Table recipe. The cogitator. `Mind/` behind the cognition gate: autopilot as an `IIntentProducer`, target selection, route planning, the decision to come home. The full mission state machine. | Build a Tier IV, walk away, and watch it clear a forest and dock **while you stand there.** And it should feel slightly uncanny. |
| **M8** | **Tier IV: the Expedition (Mode B).** Dispatch, the mission ledger, deterministic offline reconciliation. Crew-aboard correctly rejected. | Send it out, play elsewhere for two in-game days, come home to a full hold. Two clients agree on the result. |
| **M9** | Polish: the full instrument suite (**visible cargo**, warning lamps, hull scorch/dent states), particles, audio, gear/piston animation, lance overheat vent, wreck loot, distress beacon, the cogitator startup sequence. The lore tablet, the raven lines, the dream. | You can tell your machine is coming home loaded from a hundred metres away. |
| **M10** | *Optional.* Custom AssetBundle. Dispatch console UI. Tier V (Deep North). | — |

---

## 12. Definition of done

- Builds clean, zero warnings, from a fresh clone with only `VALHEIM_INSTALL` set.
- Loads on a **dedicated server** with no exceptions. **No camera or instrument code executes headless.**
- **Two clients + dedicated server:** same place, same state, same cargo, same gauge readings. Ownership transfer mid-operation is seamless.
- **The solo test (release blocker):** one player builds a Tier I, sets a Governor course, climbs into the gun ring, and harvests a copse without ever touching the driver's seat again. **If this doesn't work, the mod is broken for most of its audience.**
- **Crew tests:**
  - A drives, B guns, C rides. All three see the same beam, the same aim, the same nodes breaking. Drops land in the hold **once** — never twice, never zero times.
  - A mounts, drives 500m, dismounts. Nothing is lost.
  - Three players attached at a 30° Black Forest slope at full speed: nobody falls off, jitters, or desyncs.
  - The driver disconnects mid-drive with a gunner and passenger aboard. Nothing breaks; ownership reassigns.
  - The machine is wrecked with crew aboard. Everyone is ejected cleanly and alive, **with cameras restored.**
  - **The reflex defence never hits the crew.** Test it deliberately.
- **Camera tests (§8.10) — release blockers:**
  - The cockpit genuinely occludes. You cannot see through the cab walls; you can see the panel and the slit.
  - Camera restores on: dismount, death in the seat, wreck, disconnect, and an unhandled exception mid-drive.
  - `ThirdPerson` mode is fully playable.
  - Twenty minutes of driving over Black Forest terrain at default comfort settings does not make a tester nauseous. **Actually test this on a human.**
- **Instrument tests:** every gauge reads correctly **on a non-owner client.** Walk up to someone else's machine and read its fuel, pressure, and hold level off its body without opening anything.
- **Cognition-gate tests (§9, Directive 9):**
  - A Tier I/II/III machine, left unmanned in a forest full of trees, **does nothing** but reflex-defend and idle. It does not move. It does not target. It does not think.
  - A Tier IV machine does all three.
  - Force `gb_tier` from 4 → 1 on a machine mid-mission: it must cleanly stop thinking, not crash.
- **Progression tests:** a Tier I lance cannot damage Black Marble and (at Tier IV) is never selected as a target; the machine cannot be built pre-Mistlands (unless `AllowEarlyBuild`); a Tier II mist lamp clears fog **for every nearby client**, not just the owner.
- Survives: relog, server restart, world save/load, depot destruction, burial in terrain, sustained attack, running out of fuel in a Swamp.
- **The lance cannot damage a player build inside a ward. Ever. Under any config.**
- **Zero item duplication under any tested path.** Test adversarially.
- `README.md` explains: the four tiers and what each gives you; the world-time model (§5); the §2 design intent; **and the camera options up front** — the restricted view is the first thing people will ask about, and the second is how to turn it off.

---

## 13. Non-goals

Do not build in v1: flight; water traversal; player-buildable structures on the chassis; physical lance ammunition (it burns boiler pressure); cross-mod integration; a custom 3D model; a full non-diegetic HUD.

And the two that matter most:

> **This is not an entry ticket to any biome.** It cannot be built before the Mistlands. It does not make an unbeaten biome survivable. It removes *tedium*, never *danger*.

> **There is no cognition below Tier IV.** No autopilot, no target selection, no route planning, no missions, no expeditions. Three tiers of this machine are a *tool*. If a design choice blurs that line, it is out of scope.

---

## 14. Prior art worth reading

- **ValheimRAFT** (zolantris) — the reference implementation for custom vehicles, including land vehicles and **players attached to moving platforms.** Your single most valuable source. **Note that it has no automation and is enormously popular — that is the shape of this mod's first six milestones.**
- **Valheim first-person camera mods** (several on Thunderstore) — **read one before you touch §8.10.** Your proof that `GameCamera` can be bent, and your template for how.
- Vanilla **`Ship` + `ShipControlls`** — how a player mounts a control station, claims ownership, and stays parented to a moving Rigidbody. *(Two Ls. It's real. Don't correct it.)* Note that `Ship` does **not** touch the camera.
- Vanilla **`GameCamera`** — including its existing first-person path at full zoom-in.
- Vanilla **`Turret`** (ballista) — acquisition, arc limits, **and the mindless tripwire circuit you're modelling reflex defence on.**
- Vanilla **`Vagon`** (cart) — wheeled physics and attach behavior.
- Vanilla **Wisplight / wisp torch / Mistwalker** — the demister mechanic (§8.9).
- Vanilla **`Smelter`** (fuel loop), **`MonsterAI`** (autonomy — for M7, not before), **`Container`** (persistence), **`Chair`** (attach points), **windmill** (driven rotating meshes → your gauge needles).
- **Jötunn's mod stub** (`Valheim-Modding/JotunnModStub`) — start from this.
- **Automatics**, **MassFarming** — automation patterns and bulk `Pickable`/`ItemDrop` handling done correctly. **For M7+. Do not read them early; you will be tempted to automate too soon.**

---

## 15. Your first response

**Do not write gameplay code.** Produce:

1. **`docs/API-NOTES.md`** — the *actual* verified signatures from the decompiled `assembly_valheim.dll` for:
   `Pathfinding`, `HitData`, `IDestructible`, `TreeBase`, `TreeLog`, `MineRock5`, `MineRock`, `Destructible`, `Pickable`, `ItemDrop`, `Inventory`, `Container`, `PrivateArea`, `ZNetView`, `ZDO`, `ZRoutedRpc`, `ZoneSystem`, `Smelter`, `WearNTear`, `StaticTarget`, **`Turret`**, **`Ship`**, **`ShipControlls`**, **`Chair`**, **`Player.AttachStart` / `AttachStop`**, **`ZInput`**, **`GameCamera`**, and **whatever component implements the Wisplight's mist-dispelling effect** (likely `Demister` — find it). Flag anything you could not find.
2. **`docs/CAMERA.md`** — the highest-value thing you can produce in this pass:
   - How does `GameCamera` compute its transform, and what is the cleanest interception point?
   - How does vanilla first-person work (full zoom-in), and how does it hide the head?
   - How does an existing Thunderstore first-person mod patch it? **Go read one.**
   - Your restore strategy on death / wreck / disconnect / exception.
   - **Is §8.10 feasible? A straight yes or no, and your confidence.** If no, say so *now* — §8.11 stands alone and I would rather know in the first response than in week three.
3. **`docs/MATERIALS.md`** — verified item and prefab names for every material in the §2 tier table, **including the Galdr Table's requirements for Tier IV.** Confirm or refute: *the Mistlands adds no new smeltable metal ore.* Get the **Ashlands** names right — I deliberately did not guess them, and neither should you.
4. **Four specific answers:**
   - **How does Valheim apply repeated damage from a sustained source?** Find an existing example (burning, AoE ticks, ballista repeat fire). If none exists, say so — that confirms the tick model in §8.8.
   - **How does the demister replicate in multiplayer?** Local visual effect, or networked? This determines whether §8.9's mist lamp works for anyone but the owner. **The likeliest hidden bug in this design.**
   - **Does the `IIntentProducer` seam (§8.2) hold?** Sketch how the Tier IV `Autopilot` will plug into the same motor the `HumanDriver` and `Governor` use, **without modifying `Motor.cs`.** If it can't, tell me now — that seam is the entire build plan's load-bearing wall.
   - **What are the moddable lore channels?** The readable-runestone component and custom-text registration, the Hugin/Munin tutorial trigger path, whether Munin exists as a distinct in-game raven, and whether the sleep `DreamTexts` list accepts additions (§2 lore).
5. **A prefab inventory** — the real parts you intend to kitbash. Include your candidate for the beam VFX, the lamp housing, **the gauge-needle mechanism**, **the Tier IV cogitator core**, and **which pieces make a cab that occludes correctly from the inside.**
6. **`docs/ARCHITECTURE.md`** — the state machine (and how the cognition gate enforces itself), the control-mode axis, the ZDO schema, the intent seam, the lance damage/authority path, the tier data model, the camera approach, and your proposed solution to §5.
7. **Every place this brief is wrong or underspecified**, and what you propose instead. I would rather argue with you now than debug your assumptions later.

Then stop and wait.

---

## 16. Open decisions (for the human, not the agent)

Called *for* you. Reverse any and hand the agent the amendment:

1. **Automation is Tier IV, and that is the best decision in this document.** For three tiers you *are* the machine's mind. Automation becomes the reward for having done it by hand — which finally makes §2's "exploitation engine, not entry ticket" *true* rather than just stated.
2. **It also defers the hardest problem in the mod to last.** Offline reconciliation (§5) was "solve this before anything else." It is now a Tier IV concern at M8. **The mod is complete, playable and genuinely good at M6 without it.** That is an enormous de-risking that fell out of a pure design decision.
3. **But it breaks solo play, and the Governor is the fix.** Without automation, a solo player owns a two-person vehicle with no second person — they can drive *or* gun, never both. **The Governor** (Tier I): a dumb mechanical course-regulator. Hold a heading, follow player-set waypoints. The player supplies the goal; the machine supplies the driving. Set a course, climb into the ring, cut. It is also period-correct — a flyball governor *is* a Victorian steam-engine part. **The solo test is a release blocker in §12.**
4. **Reflex / Governance / Cognition.** The taxonomy that makes the tier line rigorous. Reflex (the hopper vacuums, the boiler burns, the lance tripwire-defends — *exactly the vanilla Ballista's mindless circuit, at exactly this tier*) and Governance (executes a player-set goal) are Tier I. **Cognition — the machine supplying its own goal — is Tier IV.** Every "should this be automated?" argument resolves against this table.
5. **Tier IV is conjured at the Galdr Table, not forged at the Black Forge.** The body is metalwork; the **mind is not.** That structural split already exists in the game and it makes Tier IV feel like a different *kind* of upgrade rather than a bigger number. You are not bolting on armour. You are giving it a soul.
6. **force → sight → shelter → mind.** Each tier grants a capability the machine lacked. The last one grants it a self.
7. **Tier I must be worth it without automation** — and it is: a beam that fells a tree in seconds with no stamina, a hopper that hoovers every drop, a hold that carries ten times what you can. **You still do the work; you just do it with a machine instead of a hand tool.** That is the Industrial Revolution in one build, and it makes the steampunk theme *earn itself*.
8. **The intent seam (§8.2, Directive 4) is now the load-bearing wall of the build plan.** Build `DriveIntent` in M2, before any automation exists, or you rewrite locomotion at M7. I've made the agent prove the seam holds in its first response.
9. **The camera is what earns the crew** — and now that the crew is *mandatory* for three tiers, §8.10 is no longer a flavour feature. It is structural. It is also still the riskiest thing in the doc, which is why the agent owes me a feasibility verdict up front.
10. **Exterior instruments are required either way.** At Tier IV, when nobody is aboard, **the gauges are the UI.** And the hold fills visibly — best readout in the machine, free, and you can see from a hundred metres that your machine is coming home heavy.
11. **The gate is Black Cores, not metal.** There is no Mistlands metal. You loot the Dvergr's own machine cores and reverse-engineer them.
12. **Motion sickness is real** and I treated it as such: `ChassisRollInfluence` defaults to 0.35, and `ThirdPerson` is a prominent, lossless opt-out.
13. **The lore is the real myth, recast onto Valheim's own cast.** Brokkr and Eitri stay named in flavor text (your Skoll-and-Hati precedent); the liar stays unnamed, as vanilla keeps him; and the keystone substitution is that **the myth's saboteur fly becomes the Seeker swarm — the Queen is the fly.** Black Cores are canonized as the eitr-hearts of the original boars, upgrading the §2 gate from "loot Dvergr parts" to "loot the dead originals' hearts." All lore is additive flavor behind a config flag; none of it gates function.

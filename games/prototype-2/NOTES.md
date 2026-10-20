# Prototype 2 (2012) — co-op working notes

Working notes for the Prototype 2 co-op project. Written as things happen, not at the end.
Labels: **[OBS]** observed, **[DER]** derived from observation, **[GUESS]** hypothesis,
**[UNK]** unknown / not verified yet.

Owner asked for Prototype 2 co-op with friends. Repo: `cranebeaker/coop-anything`.
Owner decisions (2026-10-21): game on **Steam appid 115320**, target **2-player online
story co-op**, tools (git / gh / Python) may be installed.

---

## 0. Triage

### 0.1 Identity

| Field | Value | Source |
|---|---|---|
| Title | Prototype 2 | SteamDB [OBS] |
| Year | 2012 (released 24 July 2012) | SteamDB [OBS] |
| Developer / Publisher | Radical Entertainment / Activision | SteamDB [OBS] |
| Steam AppID | **115320** | Steam store search; `store.steampowered.com/api/appdetails?appids=115320` returns `success:false` (age gate / region), but the app exists and SteamDB resolves it. [OBS] |
| Steam build id | **19788008** (public branch, last public update 29 Sep 2025) | SteamDB depots [OBS] |
| Engine | **Titanium 2.0** (Radical's proprietary engine) | PCGamingWiki infobox + `Engine:Titanium` page [OBS] — see §0.4 and the correction note below |
| Architecture | **32-bit (i386 / PE32)** for both `prototype2.exe` and `prototype2engine.dll` | PE header parsed from the installed files [OBS] |
| Store | Steam (owner's copy) | [OBS] |
| Install path | `<game dir>` = `E:\SteamLibrary\steamapps\common\Prototype 2` | [OBS] |
| `prototype2.exe` SHA-256 | `1D1F1E15A7630552387C3459BF03B349954996363B575BEE260340AA9358DFE2` (3,241,472 bytes) | [OBS] |
| `prototype2engine.dll` SHA-256 | `C3D51D6355387963A63CBAC058381B8FD1CE39F51088FA50276A49285EE94174` (15,517,184 bytes) | [OBS] |
| OS | Windows 11 x64 (host) | [OBS] |
| Save location | **[UNK]** — no save folder exists yet; the game has not been run. `sgm_ProfileSave` / Steam Cloud are the candidates. |

### 0.2 Prior work in this repo

**[OBS] None.** Checked before starting:

- `DIRECTORY.md` — no entry for Prototype or Prototype 2 (case-sensitive search for
  "Prototype 2"; every "prototype" hit is the `status: prototype` enum or an unrelated
  project such as *Prey Coop (CoopPrototype)*).
- `games/` — 28 game folders, none for Prototype.
- `data/directory.yaml` — no Prototype entry.
- GitHub issues search `repo:cranebeaker/coop-anything prototype` — 19 hits, all unrelated
  (the word "prototype" in other games' titles). `anvil` → 3 hits, all Assassin's Creed.
  `heller` → 0 hits.
- GitHub open PR list (10 open) — none for Prototype.

=> Prototype 2 is **new ground**. Both a gap entry and a first project entry are valid;
§1.4 explains why a *gap* entry is the honest one today.

### 0.3 Existing co-op attempts (Phase 1 research)

**[OBS] A. Reddit project, unreleased and apparently stalled.**
r/PrototypeGame, "Possible prototype 2 multiplayer mod ??", author `u/usbdrivetaken`.
<https://www.reddit.com/r/PrototypeGame/comments/1w1eqpi/possible_prototype_2_multiplayer_mod/>
(Reddit blocks plain fetches with 451; read through the browser.)

- OP: "Hello, posting in advance, required volunteers for testing multiplayer mod … Please
  comment below if you would be interested, provide contact information (email, discord name)".
- OP's latest in-thread update: "UPDATE: sync work in progress , host lobby & client join
  and chat seems to work good for now will keep you updated after further testing as it still
  needs some tweaking. I also plan to include custom radnet skins for selection".
- OP reply: "you will be able to host up to 20 players on your home computer … with ability
  to continue from the latest saved point … send lobby code to your friends so then they join
  and you can play together main missions an advance through story together".
- **Post published 2026-08-29 06:21 UTC; last activity in the account 2026-09-04 00:50
  UTC.** The account `u/usbdrivetaken` has exactly **four activities in its entire
  history**, all inside this one thread (captured via the Reddit Atom feed
  `reddit.com/user/usbdrivetaken/.rss`). Silent for 47 days as of 2026-10-21.
- The OP's screenshots show Prototype 2 in the Green Zone with two Heller models on a
  rooftop, a **scripted in-engine chat panel** (top right, `Player(1): lol bot` with a
  text-input field), a **floating nametag `redfl_bot (2)`** over the remote character and
  an FPS counter. **[DER]** the chat panel is drawn in the engine's own 2-D GUI style, not
  as an external overlay, which matches the Lua debug-menu architecture (see 1.3).
- **No download link, no repository, no Discord invite anywhere in the thread**, and no
  GitHub, Nexus, ModDB, GameBanana, YouTube or Steam presence for the handle.
- **[UNK]** whether the code is obtainable. The author asked for testers by email or
  Discord, so reaching out is a decision for the owner (hub ground rule 1: ask before
  contacting anyone).

**[OBS] B. Cheat Engine + Parsec co-op video, by MysteriousPowers.**
"Prototype 2 Co-op w/ MysteriousPowers" — <https://www.youtube.com/watch?v=n85kmU7LqEs>
Description: "This was done via cheat engine and Parsec, all thanks to MysteriousPowers, he
will do a special soon with more pvp and storyline co-op."
GitHub user `MysteriousPowers` exists (created 2024-04-08) but has **zero public repos**.
**[OBS] B. MysteriousPowers: two dead ends, and a Patreon.**

- **"Prototype 2 Co-op w/ MysteriousPowers"** (uploaded by *JKing*),
  <https://www.youtube.com/watch?v=n85kmU7LqEs>, 2021-12-24, 4,642 views. Description:
  "This was done via cheat engine and Parsec, all thanks to MysteriousPowers, he will do a
  special soon with more pvp and storyline co-op." **[DER]** Parsec here is screen
  sharing, not networking, so this was two machines with one memory editor - a proof of
  concept, not a netcode.
- **The thread it came from** (u/Cursed_user19x),
  <https://www.reddit.com/r/PrototypeGame/comments/ro99y6/prototype_2_coop_with_me_and_the_one_who_made_it/>
  (2021-12-25, last comment 2026-01-05). The author: "Yeah I plan to, myself, add
  splitsreen into this script." So it was a **script**, and split-screen was never done.
  In 2025 commenters asked "Any news or link to download? Can't find it anywhere";
  unanswered.
- **MPFMM** - <https://www.youtube.com/watch?v=VL0lAVrqNXE>, 2024-06-18, 338 views, by
  *TheMysteriousPowers*. The description says outright "please be aware that i have not
  released this mod yet" and "I made new improvements and cleaner code". The project
  files are **Patreon gated** (<https://www.patreon.com/MysteriousPowers>). The acronym
  is probably "MultiPlayer FinalMercerMod" but the author never says so, and his
  non-co-op *FinalMercerMod* is a separate Nexus mod (`prototype2/mods/19`).
- GitHub user `MysteriousPowers` exists (created 2024-04-08) with **zero public repos**.

**[OBS] C. No released co-op mod exists on the usual platforms.**
- **Nexus Mods**, game "Prototype 2": 91 mods (game page, 2026-10-21), **all** in the
  Miscellaneous category, all single-player (graphics, skins, fixes, a debug-menu
  re-skin, Lua script collections). Keyword search on the game page: `multiplayer` ->
  **0 results**, `coop` -> **0 results**, `co-op` -> **0 results**.
- **GameBanana** PT2 hub: 28 mods, none multiplayer.
- **Steam Workshop**: not supported for this app (no workshop category).
- **Steam Guides**: 30 guides, all crash fixes, achievements, black boxes, DLC unlocks,
  language and graphics - no co-op guide.
- **GitHub**: repository and user searches for prototype 2 multiplayer, anvil/titanium
  coop mod, gibbed prototype and MysteriousPowers -> no Prototype 2 modding repositories.
- **r/CoOpGaming's "mega list"** of co-op mods (1,286 entries) -> "Prototype" = **0 hits**
  (for comparison: Postal 2 = 2, Halo = 6, Resident Evil = 20).
- **unmoddable.com** multiplayer-mod index -> Prototype absent.
- **[UNK]** ModDB could not be enumerated (Cloudflare).

**[OBS] D. The community hub: "The Hive - Prototype Modding and Community Server"**,
<https://discord.gg/Ye832UktvC> (created 2021-07-24, about 6,400 members). This is where
the modding talk happens, where u/usbdrivetaken asked people to leave their Discord
name, where MysteriousPowers points, and where a 2024 thread says people had a **local**
co-op hack. **[DER]** the highest-value single action for the next agent.

**[OBS] E. Useful technical critique in the Reddit thread** (commenter
`Glum-Highlight-6724`, 2026-09-01), listing what an open-world co-op must sync:
"pedestrians, props, cars, camera will move for every player during consumption animation of
any player, time dilation, screen shake, enemy targeting … require precise sync too", plus
"custom characters with Hit_Reaction/Expression/Grabable behaviour stuff for proper sync.
Because that way you could achieve complete relationships without any desync and Player1 Will
see exactly what Player2 did."
**[DER]** This is a good concrete scope list and it matches the hard parts of open-world co-op.
It is the checklist the design doc (§3) is built from.

**[OBS] F. The first Prototype (2009) is a second gap.** Multiplayer was planned by
Radical and cut before release, and no co-op mod for it was found either. A
"Project M" co-op mod for the first Prototype could **not** be verified - every search
for that name returns the Super Smash Bros. Brawl mod of the same name, so treat the
premise as a likely conflation and do not build on it.

### 0.4 Engine

**[OBS] The engine is Titanium 2.0**, Radical's proprietary engine. PCGamingWiki's
Prototype 2 infobox lists "Engines: Titanium 2.0" and links a dedicated
`Engine:Titanium` page (archived snapshot 2026-09-20), whose only listed game is
Prototype 2. API is Direct3D 9.0c, UI middleware Scaleform GFx, cutscenes Bink Video
1.9.99.0.

> **Correction.** Earlier in this session the engine was recorded here as "Anvil
> (Monolith)" on the strength of a derivation from the game's modding ecosystem. That was
> **wrong**. Anvil is Ubisoft's engine; the Radical/Monolith-lineage engine in these games
> is Titanium, later reworked into AnvilNext for Middle-earth: Shadow of Mordor. Corrected
> everywhere in this PR. (The derivation was not baseless — Titanium *is* in the Monolith
> lineage and does share the open `.rcf`/Pure3D content format — but the engine's actual
> name is Titanium 2.0, and a directory entry with the wrong engine sends the next agent
> looking for the wrong prior art.)

**[OBS]** Command line arguments the engine understands (PCGamingWiki §Command line
arguments): `Level=[level]`, `LanHost=[value]`, `Monkey=[value]`, `MonkeyOptions`,
`HideHUD`, `NoGameState`, `DetachedRenderer`, `SkipMovies`, `BatchMode`, `Autotester`,
`Paranoid`, `NoVSync`, `NoSleepOnLostFocus`, `RandomStreamingDelay=[value],[value]`,
`ReproducibleAmbientManager`, `Resolution=WxH@Hz`, `Windowed`, `WindowX/Y/Width/Height`,
`DebugLevel=[value]`, `DebugFilter=[value]`, `MemoryMonitor`, `NoCrashRecord`,
`NoInteractiveAsserts`, `LookUpNameStrings`, `SubmitNameStrings`.
- `Level=` **starts a chosen level on launch** and the argument list is the mission
  manifest: `story_a1`..`story_m3`, `story_intro1/2`, `story_l2pre`, `story_l3pre`,
  `story_FinalBoss`, the `yz_*`/`gz_*`/`rz_*` zone levels, `ow_*_black_net_hunt_target`,
  `location_*`, `meta_*`, and ~60 `test_*` levels.
- **[OBS]** PCGamingWiki warns that `Level=` "can result in unlocking of achievements and
  overwrite the automatic save file".
- `LanHost=` is a **latent LAN/host entry point in the engine** — the multiplayer that
  Radical planned and cut for the first Prototype left a seam here. **[DER]** worth
  looking at before building a lobby from scratch.
- **[DER]** `Level=` plus host-authoritative AI is what makes the mission-scoped co-op in
  §3 realistic: each `story_*` level is a discrete, self-contained mission that both
  clients can be dropped into, instead of trying to synchronise the whole open world.

**[OBS]** Engine quirks that matter for a co-op design:
- the engine is **internally locked at 60 FPS** and duplicates frames above that;
- it **cannot use more than 8 threads** (crash);
- it over-polls USB I/O, which costs frame rate;
- the script layer is **event driven** — there is no per-frame Lua callback, so the
  per-frame sync loop has to live in the native plugin, not in Lua.

**[OBS]** Save and config location: `%USERPROFILE%\Documents\Activision\Prototype 2\`
(Steam Cloud supported).

Relevant sibling rows in `DIRECTORY.md`: **none for Titanium**.
`playbook/engine-families.md` did not mention Titanium at all — added in this PR (§6).

### 0.5 Path decision

**Path 2, build from zero** — but with a much better starting position than "closed binary":
the game ships its own script layer, the content format is documented, and the engine
exports a clean C API. See §2 and §3. The only "revive" candidate (the Reddit project) has
no published code.

---

## 1. The game on disk

### 1.1 File layout

**[OBS]** The whole install is 106 files (SteamDB depot 115322 "Content") plus 2 files in
depot 115321 "Binaries":

```
prototype2.exe            3,241,472 B   32-bit PE, the game
prototype2engine.dll     15,517,184 B   32-bit PE, the engine
steam_api.dll               124,296 B   Steamworks flat API
binkw32.dll                 226,304 B   video playback
cementfiles.p3d                 472 B   name-hash -> filename map (Pure3D)
skuinfo.p3d                     193 B
installscript.vdf              1,365 B
audio/  redist/  scripts/           (3 loose directories)
00audio.rcf 943 MB   01audio.rcf 940 MB   art.rcf 1.06 GB
audio_russian.rcf 2.10 GB   boot.rcf 237 MB   cells.rcf 557 MB
movies.rcf 704 MB   scripts.rcf 219 KB   shaders.rcf 5.8 KB
story_a_to_h.rcf 1.09 GB   story_i_to_z.rcf 1.84 GB   woi.rcf 1.39 GB
```

**[OBS]** `scripts/` contains exactly one loose file, `statsachievements_tod.p3d` (23,017 B).
Everything else that looks like a script lives inside the `.rcf` archives.

### 1.2 The .rcf container

**[OBS]** `.rcf` is a "Cement" library (Radical's container format), not a zip. The format
was published by Rick "gibbed" in 2012 as the **BSD-3-clause** project
[gibbed/Gibbed.Prototype](https://github.com/gibbed/Gibbed.Prototype) (unpack / pack /
compress / uncompress tools plus the Pure3D and Cement format code).
**[DER]** BSD-3-clause permits forking, modifying and redistributing, so the tooling can be
reused in this project; attribution is kept in the tool headers.

Layout, confirmed byte-for-byte against the shipped `scripts.rcf`:

```
0x00  24 B  magic "ATG CORE CEMENT LIBRARY" (23 chars + NUL)
0x18   8 B  padding
0x20   1 B  major version = 2
0x21   1 B  minor version = 1
0x22   1 B  endian flag (0 = little)
0x23   1 B  unknown = 1
0x24   4 B  index offset   (0x3C)
0x28   4 B  index size     (12 per entry)
0x2C   4 B  metadata offset
0x30   4 B  metadata size
0x34   4 B  unknown = 0
0x38   4 B  entry count
0x3C  12 B  first index entry: name hash (u32), offset (u32), size (u32)
```

then the index table, 2048-aligned, the metadata table (u32 alignment, u32 unknown, then per
entry: type hash, alignment, unknown2, u32 name length, name **including its NUL**, 3 zero
bytes), and finally the payloads, each 2048-aligned.

**[OBS] The name hash is case-insensitive**: `hash = hash*31 + c` over the ASCII bytes with
letters folded to lower case. Verified against the shipped archive: the name
`scripts\credits.txt` hashes to `0x4EC82608`, which is exactly the name hash in the index
table of `scripts.rcf`. The Python implementation reproduces it.

### 1.3 The script layer — the key finding

**[OBS]** Five Lua files live inside `boot.rcf`:

| Entry | Size |
|---|---|
| `scripts\init.lua` | 23,866 B |
| `scripts\datainit.lua` | 565,191 B |
| `scripts\missiondefinit.lua` | 221,328 B |
| `scripts\black_net.lua` | 21,624 B |
| `scripts\gameinit.lua` | 15,990 B |

**[OBS]** They are **compiled Lua 5.1 bytecode**: signature `1B 4C 75 61 51 00 01 04 04
04 04 00` = ESC "Lua", version 0x51, format 0, little endian, 4-byte int / size_t /
Instruction, **4-byte `lua_Number` (float, not double)**, non-integral, and debug info
stripped (source string length 0, no line info).

**[OBS]** `init.lua` is the **mission system**: it declares the mission table, mission
variables, collectible definitions, the register/reset/destroy entry points
(`mm_RegisterMissions`, `mm_RegisterCollectibles`, `mm_MissionVarReset`,
`mm_MissionVarDestroy`, `mm_MissionTable`, `mm_MissionsDef`, `mm_CollectiblesDef`) and the
three open-world colour zones with their mission lists.

**[OBS]** `black_net.lua` is the **story / hunt-target table**: `BlackNetData`,
`mms_HuntTargetData`, `missionUnlockKey`, `preHuntDialog`, `targetTemplate`, and per-district
entries (`District1`..`District3`) of the form `gz_woi_s01e02` (open-world event),
`gz_re_01` (hunt target), `gz_fi_01`, `gz_it_01`, `gz_vc_01`, `gz_sc_01`, each with
`_log_005` / `_log_010` / `_log_100` audio-log collectibles. Zone prefixes seen:
`gz_` (green), `yz_` (yellow), `rz_` (red), `woi_` (World of Infection), `l1_`, `a1_`,
`b2_`, `b3_`, `m1_`, `d3_`, `e1_`..`e3_`, `f3_`, `g1_`..`g3_`, `i1_`..`i3_`.

**[OBS]** Across the five scripts there are **1,544 distinct engine function names** and
~7,150 unique strings. The engine exposes a large, stable Lua API, grouped by prefix:

| Prefix | Domain | Examples |
|---|---|---|
| `go_` | game objects | `go_GetPosition`, `go_SetTransform`, `go_GetHealth`, `go_SetHealth`, `go_GetLocalPlayer`, `go_IsPlayer`, `go_SetFaction`, `go_Teleport`, `go_SlowMotion`, `go_BroadcastControllerStateChanges` |
| `ai_` | AI / NPCs | `ai_SetTarget`, `ai_SetTask`, `ai_SetExclusiveTask`, `ai_Autonomous`, `ai_Spawn_CPP`, `ai_SetMovementSpeed`, `ai_GetTarget` |
| `mm_` / `mms_` | mission manager | `mms_CompleteMission`, `mms_IsMissionCompleted`, `mms_UnlockMission`, `mms_StartTransition`, `mms_TeleportToMission`, `mms_SpawnPlayer`, `mms_CopyToSelf` |
| `db_` | profile / save database | `db_GetInt`, `db_SetInt`, `db_GetString`, `db_SetString`, `db_GetFloat`, `db_SetFloat`, `db_ResetProfileSubjects` |
| `sgm_` | save game | `sgm_ProfileSave`, `sgm_AutoSave` |
| `fes_` / `fe_` | front end / HUD | `fes_ShowObjective`, `fes_ShowMissionObjective`, `fe_FadeToBlack`, `fes_StartTransition` |
| `enc_` | encounters | `enc_AddNewEncounterAndSpawn`, `enc_SpawnNewWave`, `enc_CountEnemies` |
| `fsm_` | state machines | `fsm_StartStateMachine`, `fsm_GetCurrentStateUID` |
| `as_` | address/pathing | `as_BuildPath`, `as_NamedAddressLookup` |
| `time_` / `tod_` | timing, time of day | `time_GetElapsedTime`, `time_GetRealTimeDelta`, `time_GetSimulationDelta`, `tod_GetTimeOfDay`, `tod_SetTimeOfDay` |
| `ach_` | Steam achievements | `ach_SetAchievement`, `ach_EnableAllAchievements` |
| `ols_` | **online services** | `ols_IsProfileOnline`, `ols_IsSteamEnabled`, `ols_IsUbiconnectEnabled` |
| `stats_` | statistics | `stats_ForceResetAndSync`, `stats_AddStat` |
| `dlc_` | RadNET / DLC | `dlc_GetIsPackageDownloaded`, `dlc_GetActiveSkin` |
| `comp_`, `bfd_`, `mms_` | structures, bases | `comp_SpawnInstance`, `bfd_InitializeBase` |

**[OBS]** Two names show the game already thinks in terms of synchronisation:
`mms_SynchProfileEventDataAndSaveGameData` and `stats_ForceResetAndSync`. **[DER]** these
are the RadNET profile sync, not a multiplayer netcode, but they show the engine's own
vocabulary for "make two machines agree".

### 1.4 Where the co-op can and cannot be built from Lua

**[DER]** The Lua layer is the right place for a **co-op rules layer** (mission state,
objective text, enemy ownership, the second player's avatar) but it is **not** a networking
API: there is no `net_*` function in the 1,544 names. Transport has to come from a native
plugin.

**[OBS]** `steam_api.dll` in the install exports `SteamNetworking` and `SteamNetworking005`
(the flat-interface accessors for `ISteamNetworking` and `ISteamNetworking005`), plus
`SteamFriends`, `SteamMatchmaking`, `SteamUser`, `SteamUserStats`, `SteamUtils`.
**[OBS]** The engine itself only *imports* `SteamAPI_Init`, `SteamAPI_RegisterCallback`,
`SteamAPI_RestartAppIfNecessary`, `SteamAPI_RunCallbacks`, `SteamAPI_Shutdown`,
`SteamAPI_UnregisterCallback`, `SteamAPI_WriteMiniDump` and uses the `SteamApps`,
`SteamFriends`, `SteamUser` and `SteamUserStats` interfaces. Its RTTI names mention
`SteamServersConnected_t` / `SteamServersDisconnected_t` / `SteamServerConnectFailure_t`,
i.e. `ISteamGameServer` callbacks.
**[DER]** The game links Steamworks for user/friends/stats/achievements and for RadNET
leaderboards, but ships **no multiplayer netcode**. A co-op plugin can still call
`SteamNetworking005()` itself from inside the game process to get Steam's UDP P2P relay,
which gives a NAT-friendly transport with no port forwarding — the same approach the
co-op-anything playbook uses for Deus Ex, Stalker and C&C3.
**[DER]** The engine also accepts the command line argument `NoSteam` ("To bypass this pass
NoSteam as a command line argument"), which makes offline testing possible.

### 1.5 How a mod gets loaded (the shipping mechanism)

**[OBS]** The engine DLL exports a clean C API, visible in gibbed's
`Gibbed.Prototype.Game/Engine.cs`: `EngineInitialize`, `EngineTerminate`, `EngineService`,
`EngineKeyboardService`, `EngineMouseService`, `EngineWindowFocus` (plus
`EngineSetGraphicsMode` and friends in the real DLL). For Prototype 1 the DLL is
`prototypeenginef.dll`; for Prototype 2 it is `prototype2engine.dll`.

**[OBS]** gibbed's [ThunderGemios10/Prototype2-DebugMenu](https://github.com/ThunderGemios10/Prototype2-DebugMenu)
ships **plain-text** `scripts/debug_boot.lua`, `debug_buttons.lua`, `debug_toggles.lua`,
`debug_gibbed_spawner.lua` plus a patched `enable_debug.rcf` and a `cementfiles.p3d` name
map. The boot script is one line: `dofile("scripts/debug_gibbed_spawner.lua")`.
**[DER]** the engine's Lua VM loads **both** compiled and source `.lua` files from the
`scripts\` path, and runs `scripts\debug_boot.lua` when the debug menu is enabled.

**[OBS]** emoose's **Prototype2Fix** (Nexus, `prototype2/mods/42`, v1.6, 1,500 endorsements,
24 Jan 2024) is an ASI plugin whose description states: *"Allows loose/modded files to take
priority over the vanilla files inside .RCF - allowing modded files to be loaded without
needing to unpack the RCF files first."*
**[DER]** therefore a co-op mod can ship as: an ASI plugin DLL + loose `scripts/*.lua` files
+ an `.ini`. **No archive repacking and no 8 GB patch are needed.**

**[OBS] But Prototype2Fix is not safe to depend on.** PCGamingWiki (2026-06-27) says it
"Seemingly never got updated for full compatibility with the 29th of September, 2025 update
of the game. **It might be causing character movement to drift** and some fixes to cease
working."
**[DER]** movement drift would be fatal for a co-op mod, and it is the one thing
Prototype2Fix might cause. So the **archive route is the primary route** and the
loose-file route is the fallback:
- *archive route* (primary): the installer writes `coop.rcf` containing our replacement
  `scripts/datainit.lua` plus the extracted vanilla script, and patches
  `cementfiles.p3d` so `coop.rcf` is listed last. This is gibbed's proven mechanism and
  it needs **no third-party ASI plugin at all**.
- *loose-file route* (fallback): ship the files loose in the game folder, which needs
  Prototype2Fix (or an equivalent) to override the archives.

**[OBS] The ASI loader plumbing.** The game folder already contains the original
`binkw32.dll` (226,304 bytes, the Bink video decoder). The **Ultimate ASI Loader**
(ThirteenAG) is installed *by replacing* that file with a 2.2 MB proxy and moving the
original to `binkw32Hooked.dll`; the proxy then loads every `*.asi` in the folder.
**[DER]** so our native plugin has to be loaded that way, and **if `binkw32Hooked.dll` is
missing the game crashes one to two seconds after launch** - a classic "the mod broke the
game" false alarm that the installer should check for.

---

## 2. What we built so far

Game code lives in its own repository, per the hub's rules:
`<workspace>/prototype2-coop` (to be published as `cranebeaker/prototype2-coop`).

### 2.1 `tools/cement.py` — Python reader/writer for the .rcf format

**[OBS]** Written from the format description, so the project needs no .NET toolchain
(gibbed's tools are C#/.NET Framework and would need MSBuild or Mono on Windows).
Features: `list`, `extract` (seek-based, so a 237 MB archive costs only the bytes you
actually read), `pack`, plus `hash_file_name()` and `decompress_rz()`.

**[OBS] Test suite: `python tools/tests/test_cement.py` → 14 tests, all pass.**
The suite covers the hash function against a known engine value, synthetic round-trips,
alignment of payload offsets, rejection of non-Cement input, and a full
extract → repack → re-extract round trip against the **real** `scripts.rcf` from the
installed game (9 entries, all names resolve, all payloads byte-identical).

### 2.2 `tools/cement_scan.py` — list an archive's contents without reading payloads

**[OBS]** Reads only the header and the metadata table, so it lists a multi-gigabyte
archive instantly. This is how the five Lua files were located inside `boot.rcf`.

### 2.3 `tools/luac_strings.py` — constant/identifier extractor for compiled Lua

**[OBS]** The shipped scripts are stripped Lua 5.1 bytecode, so they cannot be read as text.
This tool surfaces every string constant, which is where the engine API names, the mission
ids and the story vocabulary live. Used to build the API table in §1.3.

**[UNK]** full decompilation of the five scripts is not done yet. `unluac` is the standard
Lua 5.1 decompiler and its source is BSD-licensed and public
(<https://github.com/HansWessels/unluac>), but this PC has a JRE without `javac`; building
it needs a JDK, and it also needs the chunk's custom VM to be stock Lua 5.1. A Python
Lua 5.1 chunk parser is the cheaper route and is the next tool to write.

---

## 3. Design (see also the game's own `DESIGN.md`)

### 3.1 Architecture

```
host game (32-bit)                        guest game (32-bit)
  prototype2engine.dll                       prototype2engine.dll
    + coop_plugin.asi (our DLL)                + coop_plugin.asi (our DLL)
      - Lua bridge (binds the 1,544 engine fns)   - same
      - state sync (host authoritative)           - applies snapshots
      - ISteamNetworking005 P2P transport  <====>  (Steam relay, no port forwarding)
    + scripts\datainit.lua (our wrapper)       + scripts\datainit.lua (our wrapper)
    + scripts\coop_init.lua  (our rules)       + scripts\coop_init.lua
```

The native plugin is the only thing that needs a C compiler; the mission and gameplay
rules live in Lua and can be iterated on without rebuilding.

### 3.1b Scope: mission-level, not free-roam

**[DER]** The realistic target is **two players, one mission at a time, host
authoritative**, not a synced open world. The reasons, in order of weight:

1. The open world is streamed by the engine (`go_DeleteWhenOffCamera`,
   `ai_DeleteWhenOffCamera`), so two clients cannot both simulate New York Zero.
2. The campaign is a set of **discrete levels** - `story_a1`..`story_m3`, `story_intro1/2`,
   `story_l2pre`, `story_l3pre`, `story_FinalBoss` - and the engine has a **`Level=` command
   line argument** that starts a chosen level on launch. That is the mission manifest, and
   it lets both clients be dropped into the same mission.
3. Cutscenes are **Bink video**, i.e. non-interactive, so both players can play the same
   one locally and the host can simply wait for it.
4. The engine is **internally locked at 60 FPS** and duplicates frames above that, so the
   host's frame rate drives the pacing of a host-authoritative design. That has to be
   called out to the players rather than hidden.
5. `LanHost=` is a **latent LAN/host entry point in the engine** - multiplayer was
   planned for the first Prototype and cut, and this argument is the seam it left.
   **[DER]** worth looking at before building a lobby from scratch.

Free-roam co-op stays on the list as a later goal, not as the first milestone.

### 3.2 What is shared, in priority order

1. **Players visible** — spawn a second `Heller` avatar with `go_*` / `mms_SpawnPlayer`,
   stream transform + animation state.
2. **Story** — the mission manager is script-driven (`mm_*` / `mms_*` over `db_*` profile
   variables), so the host can drive the mission state machine and replicate the flags
   with `db_SetInt` / `db_SetString` on the guest. `mms_CopyToSelf` already exists and is
   the hook for copying the host's mission state into the guest's context.
3. **Enemies** — host-authoritative: the host owns NPCs (`ai_SetTarget`, `ai_SetExclusiveTask`)
   and streams enemy transforms + health (`go_GetHealth`, `go_SetHealth`).
4. **World state** — the big one. Props, vehicles, pedestrians, time of day
   (`tod_*`), time dilation (`go_SlowMotion`, `mm_` time dilation) and screen shake all
   have to be decided by one machine. The Reddit commenter in §0.3 D is right that these
   are the real work.
5. **Items** — weapons, upgrades and consumables. `ep_*` (evolution points) and `db_*`
   give a clean per-player vs shared decision.

### 3.3 The hard parts, honestly

- **Time dilation** (`go_SlowMotion`) changes the simulation rate, so the two clients must
  agree on the rate or everything desyncs.
- **Screen shake** and **camera** are view state, not world state, but the Reddit thread
  notes that a *consumption* animation in one client moving the other client's camera is a
  visible desync.
- **Pedestrians** and **props**: the engine already streams them by distance
  (`go_DeleteWhenOffCamera`, `ai_DeleteWhenOffCamera`), so a naive "both clients simulate
  everything" design cannot work. Regional ownership is required.
- **Story flags**: `db_*` writes go to the local profile, so the host must replicate them
  explicitly, and `mms_SynchProfileEventDataAndSaveGameData` shows the engine already has a
  profile-sync concept to hook into.
- **Saves**: co-op saves are not a vanilla save. The host's profile is authoritative;
  the guest's local save must not overwrite it.

### 3.4 Test strategy (bots, not the owner)

Per the hub's rules, a human playtest is not a test plan. The plan:

- a **headless sync harness** that runs the sync layer over a fake world
  (`tools/fakeworld` in the hub) and checks the invariants the hub's fuzzer uses
  (converge, no double apply, one owner, no dupes, nothing stale);
- a **flight recorder** on the plugin so a desync between two real clients can be
  shrunk to a minimal repro (the hub's `tools/flight`);
- a **network chaos** pass with the hub's `tools/chaos` (lag, loss, reorder, one-way cuts);
- only then, a scripted in-game bot.

---

## 4. Open questions / next steps

1. **[UNK] Is the Reddit project alive, and is its code obtainable?** The account went
   silent on 2026-09-04 with no build, code or invite ever published. If
   `u/usbdrivetaken` shares it, Path 1 (revive) becomes cheaper than Path 2. Owner-side:
   the author asked for testers by email/Discord, so reaching out is a decision for the
   owner, not for the agent (hub ground rule 1: ask before contacting anyone). The
   "The Hive" Discord (<https://discord.gg/Ye832UktvC>) is where that conversation
   happens and where the modding community is.
2. **[UNK] The exact save file format** — location is known
   (`%USERPROFILE%\Documents\Activision\Prototype 2\`), the format is not. Needs one run
   of the game. Note PCGamingWiki's warning that `Level=` can overwrite the automatic
   save file.
3. **[UNK] Does the archive route actually work?** The `cementfiles.p3d` manifest route
   is proven by gibbed, but we have not run it in the game. The loose-file route depends
   on Prototype2Fix, which PCGamingWiki says may cause **character movement drift** on
   the current build — so the archive route is the primary one.
4. **[UNK] Full decompilation of the five Lua scripts** — not needed for the mod (we ship
   plain text) but needed to understand the mission system properly. `unluac` is the
   standard Lua 5.1 decompiler and its source is BSD-licensed and public
   (<https://github.com/HansWessels/unluac>), but this PC has a JRE without `javac`, and
   the chunk's opcode stream may not be stock Lua 5.1 anyway.
5. **[UNK] 32-bit toolchain on this PC.** The plugin must be x86. No MSVC Build Tools
   installed; would need them (or MinGW-w64) — **ask the owner first**.
6. **[UNK] Whether the engine's Lua VM is stock Lua 5.1.** The header says 5.1 with
   4-byte floats; the opcode stream does not obviously match stock Lua 5.1, so the
   Titanium build may have customised opcodes. This decides whether `unluac` is usable.
7. **[UNK] `ols_*` (online services) and the Steam GameServer callbacks** — if the engine
   still has a usable online-service layer, it may be a better transport than rolling our
   own on `ISteamNetworking`.
8. **[UNK] What does `LanHost=` actually do?** A latent LAN/host entry point left over
   from the multiplayer that Radical planned and cut. Worth reversing before building a
   lobby from scratch.
9. **[UNK] Whether the loose-file override applies to `scripts\` at all** — it has to, for
   the debug menu to work, but that is inference from gibbed's setup, not an observation.

---

## Log

- 2026-10-21 — Session start. Cloned coop-anything (zip; `git`/`gh` not installed).
  Triage from public sources, then from the installed game. Phase 1 research.
- 2026-10-21 — Owner approved installing git, gh and Python; all three installed via
  winget (Git 2.55.0.5, GitHub CLI 2.102.0, Python 3.13.15). Owner installed Prototype 2
  (Steam 115320, build 19788008) to the Steam library on drive E:.
- 2026-10-21 — Found the RCF format, wrote `tools/cement.py` and its test suite, listed
  and extracted the five Lua scripts from `boot.rcf`, and mapped 1,544 engine API names.
- 2026-10-21 — Found the `cementfiles.p3d` archive manifest, the engine's hard-coded
  script table, and gibbed's patched `datainit.lua`, which together settle how the mod
  gets into the game.
- 2026-10-21 — Second research pass corrected the engine name from "Anvil" to
  **Titanium 2.0** (PCGamingWiki) and added the `Level=`/`LanHost=` command line
  arguments, the save location, the Prototype2Fix movement-drift warning, the ASI loader
  plumbing, and the second community hub (The Hive).

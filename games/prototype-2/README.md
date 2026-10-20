# Prototype 2 (2012)

| | |
|---|---|
| Engine | Titanium 2.0 (Radical; closed binary plus a shipped Lua 5.1 script layer) |
| Versions tested | Steam appid 115320, build 19788008 (32-bit). `prototype2.exe` SHA-256 `1D1F1E15A7630552387C3459BF03B349954996363B575BEE260340AA9358DFE2`, `prototype2engine.dll` SHA-256 `C3D51D6355387963A63CBAC058381B8FD1CE39F51088FA50276A49285EE94174` |
| Projects | none released; one unreleased attempt in progress (see below); our work: `prototype2-coop` |
| QA level | none (no co-op session has been run; the archive tooling has automated tests) |
| Last updated | 2026-10-21 |

## Where things stand

**No co-op mod for Prototype 2 exists.** As of 2026-10-21 there is no released co-op mod
on Nexus Mods (the game page lists 91 mods, all single-player; keyword searches for
"multiplayer", "coop" and "co-op" return 0 results), none on GameBanana (28 mods) or
ModDB, no Steam Workshop support for the app at all, no co-op guide among the 30 Steam
guides, and no public repository. Prototype 2 is also absent from the r/CoOpGaming
"mega list" of co-op mods (1,286 entries) and from unmoddable.com's index.

What *does* exist is two stalled, unreleased prototypes:

- **u/usbdrivetaken** posted "Possible prototype 2 multiplayer mod ??" on r/PrototypeGame
  on **2026-08-29**, and their last word was on **2026-09-04**: "sync work in progress,
  host lobby & client join and chat seems to work good for now". Their screenshots show
  two Hellers in the Green Zone, a scripted in-engine chat panel and a nametag over the
  remote character, so the prototype was further along than the words suggest. They
  asked for volunteer testers by email or Discord, and never published a build, a
  repository or an invite. Their account has exactly four activities in its entire
  history, all in that one thread.
- **MysteriousPowers** made a Cheat Engine + Parsec proof of concept in 2021 ("he will do
  a special soon with more pvp and storyline co-op"), which never happened. A later video
  (2024) shows a cleaner rewrite called MPFMM whose description says outright "please be
  aware that i have not released this mod yet"; the files are Patreon-gated.

So this game is currently an **open gap**, and the entry in `data/directory.yaml` is
recorded as one.

## Existing work

- **u/usbdrivetaken's Prototype 2 multiplayer mod** — r/PrototypeGame thread
  <https://www.reddit.com/r/PrototypeGame/comments/1w1eqpi/possible_prototype_2_multiplayer_mod/>.
  Posted 2026-08-29, last activity 2026-09-04, silent since. In-thread update: *"sync
  work in progress, host lobby & client join and chat seems to work good for now"*.
  Described plan: host up to 20 players on the host's machine, configurable player count,
  continue from the latest save, lobby codes, play the main missions and advance the
  story together, plus custom RadNET skins. **No code or build published anywhere that we
  could find; the author asked for testers by email or Discord.** The screenshots show
  an in-engine chat panel and a nametag over a remote character, so a working prototype
  existed at some point. Note that a plan of "20 players" in a single-player open world is
  a much larger problem than the two-player story co-op that is actually being attempted
  here.
- **MysteriousPowers** — three artefacts, none released:
  - "Prototype 2 Co-op w/ MysteriousPowers" (uploaded by JKing),
    <https://www.youtube.com/watch?v=n85kmU7LqEs>, 2021-12-24. "This was done via cheat
    engine and Parsec … he will do a special soon with more pvp and storyline co-op."
    Parsec here is screen sharing, not networking.
  - The thread it came from, u/Cursed_user19x,
    <https://www.reddit.com/r/PrototypeGame/comments/ro99y6/prototype_2_coop_with_me_and_the_one_who_made_it/>:
    "Yeah I plan to, myself, add splitsreen into this script" — so it was a script, and
    split-screen was never done. Commenters still asking for a download in 2025.
  - **MPFMM**, <https://www.youtube.com/watch?v=VL0lAVrqNXE>, 2024-06-18: a cleaner
    rewrite that says "please be aware that i have not released this mod yet". The files
    are Patreon-gated (<https://www.patreon.com/MysteriousPowers>).
  - The GitHub user `MysteriousPowers` exists (created 2024-04-08) with **no public
    repositories**.
- **"The Hive" — Prototype Modding and Community Server**,
  <https://discord.gg/Ye832UktvC>. Where the modding talk happens, where
  u/usbdrivetaken asked people to leave their Discord name, and where a 2024 thread says
  people had a **local** co-op hack. The most useful place for the next agent to look.
- **Prototype (the first game, 2009)** — multiplayer was planned by Radical and cut
  before release; no co-op mod for it was found either, so the first game is a second
  gap. A "Project M" co-op mod for the first Prototype could **not** be verified — every
  search for that name returns the Super Smash Bros. Brawl mod, so treat it as a likely
  conflation and do not build on it.
- **Postal 2 "Nicks Coop"** (<https://www.moddb.com/mods/nickscoop-postal-2-coop>) — not
  Prototype 2, but the closest precedent in genre: an open-world super-powered game
  turned into a real story co-op (2–16 players, host-authoritative, on Steam Workshop).
  Use it as a feature checklist, not a code reference: Postal 2 is LithTech.

## How the game works (for co-op)

This is the part that made the difference between "impossible" and "a project".

- **Everything is in `.rcf` archives.** The install is 106 files: `prototype2.exe` (3.1 MB,
  32-bit), `prototype2engine.dll` (15.5 MB, 32-bit), `steam_api.dll`, and 11 `.rcf`
  "Cement" archives holding 8 GB of assets, plus three loose directories.
- **`.rcf` is a documented container**, not a zip: a header, a table of entries
  (name hash, offset, size) and a metadata table with the real paths. The name hash is
  case-insensitive `hash*31 + c`. The format was reverse engineered by Rick "gibbed" in
  2012 and published as the BSD-3-clause
  [Gibbed.Prototype](https://github.com/gibbed/Gibbed.Prototype).
- **The game ships a Lua 5.1 script layer.** Five scripts live inside `boot.rcf`:
  `scripts/init.lua`, `scripts/datainit.lua`, `scripts/missiondefinit.lua`,
  `scripts/black_net.lua`, `scripts/gameinit.lua` (compiled Lua 5.1 bytecode, debug info
  stripped, 4-byte floats). Their string constants name **1,544 engine functions**:
  `go_*` (game objects), `ai_*` (NPCs), `mm_*`/`mms_*` (missions), `db_*` (profile),
  `fes_*` (front end), `enc_*` (encounters), `time_*`/`tod_*` (timing), `ols_*` (online
  services), `stats_*`, `ach_*` (Steam achievements), and more.
  - `init.lua` is the mission system: the mission table, mission variables, collectible
    definitions, and the three open-world zones.
  - `black_net.lua` is the story table: hunt targets, mission unlock keys, pre-hunt
    dialogue, and the per-district open-world events (`gz_woi_s01e02` and friends) with
    their audio-log collectibles.
- **The engine loads its scripts from hard-coded paths.** `prototype2engine.dll` contains
  the literal strings `scripts/gameinit.lua`, `scripts/black_net.lua`,
  `scripts/missiondefinit.lua`, `scripts/datainit.lua` in one table, and `scripts/init.lua`
  separately. The responsible class is `engine::LuaScriptManager`
  (`code\engine\scripting\luascriptmanager.cpp`).
- **`cementfiles.p3d` in the game root is the archive manifest** — a Pure3D
  `ContentCementFileDefinition` listing the `.rcf` files to open, in order. Entries are
  looked up by name hash, so **a later archive overrides an earlier one**. This is how a
  mod replaces a game file.
- **The engine is Titanium 2.0** (Radical's proprietary engine; Direct3D 9.0c, Scaleform
  GFx UI, Bink Video 1.9.99.0 cutscenes). PCGamingWiki has a dedicated `Engine:Titanium`
  page whose only listed game is Prototype 2. Titanium is the engine later reworked into
  **AnvilNext** for Middle-earth: Shadow of Mordor, so there is no sibling co-op work to
  reuse there either.
- **The engine has useful command line arguments.** `Level=[level]` starts a chosen level
  on launch, and the argument list is the mission manifest: `story_a1`..`story_m3`,
  `story_intro1/2`, `story_l2pre`, `story_l3pre`, `story_FinalBoss`, the `yz_*`/`gz_*`/`rz_*`
  zone levels, `ow_*_black_net_hunt_target`, `location_*`, `meta_*` and ~60 `test_*`
  levels. `LanHost=[value]` is a **latent LAN/host entry point** left over from the
  multiplayer Radical planned and cut. Also `Monkey=`, `HideHUD`, `NoGameState`,
  `DetachedRenderer`, `SkipMovies`, `BatchMode`, `Autotester`, `Paranoid`, `NoVSync`,
  `RandomStreamingDelay=[value],[value]`, `Resolution=WxH@Hz` and `Windowed`.
- **Engine quirks that shape the design:** internally locked at 60 FPS with frame
  duplication above it; **crashes with more than 8 threads**; over-polls USB I/O; and the
  script layer is **event driven with no per-frame callback**, so the per-frame sync loop
  has to live in the native plugin rather than in Lua.
- **Save and config location:** `%USERPROFILE%\Documents\Activision\Prototype 2\`
  (Steam Cloud supported). PCGamingWiki warns that `Level=` can overwrite the automatic
  save file.
- **There is no netcode.** The engine links Steamworks for user, friends, stats and
  achievements (and mentions `ISteamGameServer` callbacks in its RTTI), but ships no
  multiplayer transport. `steam_api.dll` does export `SteamNetworking` and
  `SteamNetworking005`, so a plugin running in the game process can use Steam's UDP P2P
  relay and avoid port forwarding entirely.
- **Story state is in the local profile.** `db_GetInt`/`db_SetInt`/`db_GetString`/
  `db_SetString` and `sgm_ProfileSave` read and write the player's own save, so the two
  machines must not both write it.

## What we changed

Nothing in the game yet. What we built is the research and the tooling that makes the
project possible, in the project's own repository (the hub holds only this write-up and
the directory entry, per the hub's rules):

- **`tools/cement.py`** — an independent Python reader/writer for the `.rcf` format, so
  the project needs no .NET toolchain. Supports `list`, seek-based `extract` (a 237 MB
  archive costs only the bytes you actually read) and `pack`.
- **`tools/cement_scan.py`** — lists an archive's contents from its header and metadata
  table alone, instantly, even for multi-gigabyte archives. This is how the five Lua
  scripts were located inside `boot.rcf`.
- **`tools/luac_strings.py`** — pulls the string constants out of a compiled Lua chunk,
  which is where the engine API names, mission ids and story vocabulary live.
- **`install/install.py`** — the installer. It reads the player's own `boot.rcf` to
  extract the vanilla `scripts/datainit.lua`, because our replacement wrapper has to run
  the original script and then hand over to our co-op rules. **No game file is shipped in
  any repository**; the extracted file is written into the player's install at install
  time.
- **`mod/scripts/datainit.lua`** — a plain-text wrapper that becomes the game's
  `scripts/datainit.lua`. It runs the vanilla chunk, then our rules, then returns the
  original value untouched, and it is written so that a co-op bug can never stop the
  game from starting.
- **The native half** (32-bit C++, because the game is a 32-bit binary):
  - **`include/coop/protocol.h`** — the wire format. A 16-byte header (version, type,
    flags, sequence, length, CRC-32) and a payload, with every read bounds checked and
    a failed read latching an error flag, so a malformed message from the network can
    neither crash the game nor be half interpreted.
  - **`include/coop/detour.h`** — an x86 inline detour. This is needed because the
    executable does not import the engine DLL statically: it loads it at runtime and
    resolves six exports by name, and `EngineService` (the engine's per-frame call) is
    resolved *before* any ASI plugin is loaded, because the ASI loader is reached
    through `binkw32.dll`, which the engine loads later when it sets up video. Patching
    the code is the only approach that works regardless of when the address was
    resolved. The detour is conservative: it decodes the instructions it is about to
    move and refuses to install rather than risk corrupting the target.
  - **`include/coop/steam_transport.h`** — Steam P2P. The game ships the legacy
    `steam_api.dll` with no flat exports, so calls go through a COM vtable, and the
    interface changed shape across Steamworks versions. Only the first eight slots of
    `ISteamNetworking` are declared, the P2P packet API, which has had the same order
    and signatures since SteamNetworking001. Steam's transport does NAT traversal and
    falls back to Valve's relay, so no port forwarding is needed.
  - **`src/plugin.cpp`** — the ASI plugin: waits for the engine DLL, detaches
    `EngineService`, opens the transport and runs the per-frame loop.
- **`third_party/coop_ownership`** — the actor-ownership library from the hub, vendored
  verbatim with its MIT licence. Prototype 2's open world is streamed by the engine, so
  something has to decide which machine owns which actor and something has to reject
  updates from a machine that is no longer the owner. Sharing the library means the
  ownership rules that Fallout: New Vegas co-op and Prototype 2 co-op use are the same
  rules.

### How the mod gets into the game

Proven by gibbed's [Prototype2-DebugMenu](https://github.com/ThunderGemios10/Prototype2-DebugMenu),
whose `enable_debug.rcf` carries a **patched** `scripts/datainit.lua` and whose
`cementfiles.p3d` appends that archive to the manifest. Diffing the patched script
against the vanilla one shows what was added: `loadfile`, `loadstring`,
`scripts/debug_boot.lua`, `scripts/debug_buttons.lua`, `scripts/debug_toggles.lua` and
error trapping. So:

1. a **replacement** `scripts/datainit.lua` is enough to change the script chain;
2. the engine's Lua has `loadfile`/`loadstring` in the normal environment;
3. the replacement can be **plain text**, not compiled bytecode.

Our version ships the wrapper as a loose file (the game folder's own
`scripts/datainit.lua` wins over the archive entry), with the vanilla script extracted
at install time. If the loose-file override turns out not to apply, the installer falls
back to the archive route: build `coop.rcf` with our own writer and write a patched
`cementfiles.p3d` that lists it last.

**The archive route is the primary one**, because the loose-file route depends on
emoose's Prototype2Fix, and PCGamingWiki reports (2026-06-27) that Prototype2Fix "might
be causing character movement to drift" on the current build. Movement drift would be
fatal for a co-op mod, and the archive route needs no third-party plugin at all.

The native plugin is loaded the way the community loads them: the **Ultimate ASI Loader**
replaces the game's `binkw32.dll` (the Bink video decoder) with a proxy and moves the
original to `binkw32Hooked.dll`. If `binkw32Hooked.dll` is missing, the game crashes a
second or two after launch — a classic "the mod broke the game" false alarm, so the
installer should check for it.

## QA

- **Archive tooling: automated.** `python tools/tests/test_cement.py` → **14 tests, all
  pass**. The suite covers the name hash against a known engine value
  (`scripts\credits.txt` → `0x4EC82608`, taken from the shipped `scripts.rcf`),
  synthetic round-trips, 2048-byte payload alignment, rejection of non-Cement input, and
  a full extract → repack → re-extract round trip against the **real** `scripts.rcf`
  from the installed game: 9 entries, all names resolve, all payloads byte-identical.
- **Native code: automated, as 32-bit binaries.** `native/build.bat` builds for i686,
  checks that the output really is 32-bit, and runs three suites:
  - protocol: **15 tests, 352 checks** — round trips, rejection of short buffers, wrong
    versions, wrong lengths and bad CRCs, a reader that never reads past the end, and
    two fuzz loops (garbage in, and single-bit corruption of a valid message);
  - detour: **4 tests, 23 checks** — the instruction decoder against hand-computed
    lengths, a detoured function still reaching the original through the trampoline,
    and the case that breaks naive trampolines (a relative branch inside the moved
    block);
  - ownership: **30 tests, 134 checks**, vendored from the hub unchanged.
  The tests found three real bugs while being written: an x86 decoder that did not
  know `push ebp`, a ModR/M length calculation that was wrong for several encodings,
  and a test whose function pointer the compiler folded away so the patched code was
  never called. All three are fixed.
- **Installer: checked by hand** against the installed game. `--find` locates the
  install, the identity check passes against the known build (both SHA-256s match), and
  the extraction returns the 565,191-byte vanilla `scripts/datainit.lua` with the
  expected Lua 5.1 header. The install itself has **not** been run against the owner's
  copy.
- **Co-op: nothing.** No two-player session has been attempted, and no code has been run
  inside the game. The QA level for this game is therefore **none**.

## How to play

Not yet. The plan, once there is something to play:

- one player runs the installer, then starts the game normally;
- one player sets `host=1` in `coop_config.ini`, the other leaves it at `host=0`;
- they meet over Steam's relay, so no port forwarding is needed.

## Known issues

- The **archive route** (patched `cementfiles.p3d` + our own `coop.rcf`) is **unverified
  in-game**; the loose-file override is the fallback. Neither has been run.
- The engine is **locked at 60 FPS** with frame duplication, so a host-authoritative
  design makes the host's frame rate drive the pacing. That has to be said out loud to
  the players.
- The engine **crashes with more than 8 threads** and over-polls USB I/O, both of which
  matter on a modern PC with a lot of peripherals.
- The 2025-09-29 Steam update removed CEG, changed `art.rcf` and `scripts.rcf`, and
  removed the loose `scripts/init.lua`; binaries and content have to be a matched set
  (old exe with new data trips an engine assert).
- The engine's Lua is Lua 5.1 with 4-byte floats, but the opcode stream does not
  obviously match stock Lua 5.1, so the Titanium build may have customised opcodes. We do not
  need a decompiler, because we ship plain text, but it means the game's own scripts
  cannot be read as source.
- The script layer has **no per-frame callback** — the API is event driven — so the
  per-frame sync loop has to live in the native plugin, not in Lua.
- A 32-bit C++ toolchain is **not installed** on this machine; the native plugin cannot
  be built yet.

## Next steps

1. Build the native plugin (32-bit C++): `ISteamNetworking005` transport, a Lua bridge,
   and the per-frame tick. Needs MSVC Build Tools or MinGW-w64.
2. Run the installer and the wrapper **inside the game** and confirm the engine loads
   our plain-text `scripts/datainit.lua` and that the game still starts. This is the
   biggest unknown and the cheapest to resolve.
3. First milestone: **two players can see each other** and share enemy state, in one
   `story_*` level launched with `Level=`. That is the smallest thing that proves the
   transport, the ownership model and the Lua bridge all work.
4. Then the story: the host owns the mission state machine and replicates the story
   flags to the guest through the `db_*` API.
5. Then the hard part — regional ownership of the streamed open world (pedestrians,
   props, vehicles, time dilation, screen shake, consume animations), which is exactly
   what the r/PrototypeGame commenter warned about.
6. Reverse the `LanHost=` engine argument before building a lobby from scratch: it is a
   latent host entry point left over from the multiplayer Radical cut.
7. Reach out to `u/usbdrivetaken` (the owner of the stalled project) and to The Hive
   Discord, to ask whether the code is available, so the next agent does not rebuild
   what already exists.

The project lives in its own repository so that the hub holds only write-ups and
directory entries. See the write-up's "What we changed" section for what that
repository contains.

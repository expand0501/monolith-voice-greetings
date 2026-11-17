# Monolith Voice Greetings by Expand

Goal: Monolith stalkers greet the player with other factions' greeting lines (factions chosen in MCM), because
every Monolith greeting file is silent. Works standalone (vanilla walking-past greeting) and with our Dialog
Greetings (dialog greeting, following its settings). Standalone, no replaced scripts or voice files.
Shipped readmes: `src/README_EN.txt`, `src/README_RU.txt` (cp1251). Started 2026-11-17 (Expand's request).

## Files
- `src/gamedata/scripts/mvg_greet.script`: all logic (log tag `[MonolithVoiceGreetings]`).
- `src/gamedata/scripts/mvg_greet_mcm.script`: MCM (id `mvg`, single page). Defaults must match `DEFAULTS`.
- `src/gamedata/configs/misc/sound/script_sound_mvg.ltx`: 32 themes `mvg_<voice>_<1|3>` (walking past, same as
  vanilla `[meet_hello]`: idle 7-10 s) and `..._dlg` (same as Dialog Greetings' `[dlg_greet_hello]`: idle 0,
  `play_always`). Generated; voices = greh, military, stalker, csky, dolg, freedom, bandit, ecolog.
- `src/gamedata/configs/text/{eng,rus}/ui_st_mvg_mcm.xml`: MCM strings (rus = Windows-1251).

## Facts found (verified 2026-11-17)
- Vanilla `sounds_voices.db0`: `monolith_3\states\meet\meet_hello_1.ogg` (the only Monolith greeting) is the same
  3587-byte, 0.2 s silent file as `killer_1/3` and `no_speach`. Dux voice pack ("305- Dux Characters Kit Voices
  Pack", disabled in `G.A.M.M.A` and `v31.1 - radiation rework`) ships the same placeholder for `monolith_1/2`
  meet lines; its real Monolith audio is only combat/idle/loot/wounded/sleep.
- "29- Dux's Innemurable Characters Kit" (enabled in both profiles) gives Monolith profiles `monolith_1` (no files
  without pack 305) or `monolith_3`.
- `_1` = clear voice, `_3` = the same recordings with a gas-mask filter (identical durations). Expand: keep that
  pairing (Monolith `_3` voice -> faction `_3` lines, other Monolith voices -> `_1`); never use the monolith files.
- Extraction tools used (scratch, not in the workspace): Python decoder for the X-Ray LZHuf file table
  (MAX_FREQ 0x4000, see `reference\repos\xray-monolith\src\xrCore\LzHuf.cpp`) + LZO1X for compressed files.
  Listening page of all vanilla greetings: `reports\monolith-greeting-candidates\index.html` (git-ignored).

## How it works (keep)
- Target NPCs: `npc:sound_prefix()` matches `monolith_(%d)`. Community is not checked: a Monolith with another
  voice keeps his own greeting.
- Faction per NPC: `pool[npc_id % #pool + 1]` (pool = enabled factions in FACTIONS order) -> stable, nothing saved.
- Themes are on demand: `avail_communities = mvg_on_demand` (no real community) so `sound_theme.init_npc_sound`
  never loads them at spawn; `ensure_loaded` calls `theme:init_npc(npc)` once per NPC while online and saves /
  restores `npc:sound_prefix()` around it (vanilla init_npc with `npc_prefix = false` returns without restoring the
  prefix when files are missing). `npc_on_net_destroy` clears our entries in `theme.npc` / `theme.sound_path`
  (engine sounds die with the object; vanilla re-inits its themes in xr_motivator net_spawn, ours would go stale);
  `actor_on_first_update` / `actor_on_net_destroy` reset all our themes.
- Walking past: wrap `xr_sound.set_sound_play` once (guard `orig_set_sound_play`); `meet_hello` -> walk theme for
  target NPCs unless Dialog Greetings mutes walking-past greetings (`dlg_greet.walk_hello_muted()`, fallback for
  DG <= 1.0.4: MCM `dlg_greet/mute_hello`, default true). Leaving `meet_hello` unchanged keeps DG's mute working no
  matter which wrapper is outer.
- Dialog: `dlg_greet.add_greeting_provider(dialog_provider)` (Dialog Greetings 1.0.5+), registered in
  `on_game_start` and again on first update (guard `provider_added`). DG runs its own checks, then plays our `_dlg`
  theme. DG <= 1.0.4: our wrapper swaps `dlg_greet_hello` -> `_dlg` theme (works only for NPCs DG already accepts,
  i.e. `monolith_3` voices; `monolith_1` NPCs are skipped by DG before any sound).

## Profiles
- No enabled mod overrides `xr_sound.script` / `sound_theme.script` (see Dialog Greetings notes). Voice packs that
  replace the borrowed factions' `meet_hello_*` files are used automatically ("8- Better Merc voicelines",
  "RU_UA_Voice_Profiles" touch killer / stalker_4 only, not in our list).

## Open items
- 0.1.0: Expand tested the build in game (2026-11-17, "Build tested"); released as v0.1.0 with Dialog Greetings 1.0.5. GitHub: `expand0501/monolith-voice-greetings` (public, created 2026-11-17, released v0.1.0).
- Needs Dialog Greetings 1.0.5 for dialog greetings of `monolith_1` NPCs.

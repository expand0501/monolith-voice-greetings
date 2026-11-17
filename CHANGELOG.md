# Changelog - Monolith Voice Greetings by Expand

- 1.0.0: first public release (version label only, no code changes).
- 0.1.0: first version. Monolith stalkers (voice set `monolith_N`, whose `meet_hello` files are a 0.2 s silent
  placeholder) play other factions' greetings (MCM, default Sin + Military): `_3` voice -> masked `_3` lines, other
  voices -> `_1`. Walking past: `meet_hello` swapped in `xr_sound.set_sound_play` (left alone when Dialog Greetings
  mutes it). Dialog: greeting provider for Dialog Greetings 1.0.5+, plus `dlg_greet_hello` swap for older versions.

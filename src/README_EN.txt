Monolith Voice Greetings 0.1.0 by Expand
Monolith stalkers greet you with the greeting lines of other factions.
For S.T.A.L.K.E.R. Anomaly / G.A.M.M.A.

WHY
Every Monolith greeting in the game is silent: the vanilla Monolith voice and the
Monolith voices of Dux's Innumerable Characters Kit only have a 0.2 s empty file there.
So a friendly Monolith stalker opens his mouth and says nothing.

WHAT IT DOES
- Monolith stalkers say a greeting line of another faction instead. You choose the
  factions in MCM (default: Sin and Military).
- Each stalker always uses the voice of the same faction.
- Masked Monolith speak through their mask (the game's masked voice lines), the others
  speak with a clear voice.
- Without Dialog Greetings: they greet you like any stalker when you walk up to them.
- With Dialog Greetings 1.0.5+: they greet you when Dialog Greetings says so (by default
  only when you open a dialog), with all its settings (chance, cooldown, companions...).
  Dialog Greetings 1.0.4 and older: only Monolith with the vanilla voice greet in dialogs.
- Monolith stalkers whose voice isn't a Monolith voice keep their own greeting.

REQUIREMENTS
- Anomaly 1.5.2+ / G.A.M.M.A. No other mods needed.
- Optional: Dialog Greetings 1.0.5+ by Expand.
- MCM (included in GAMMA) for the settings; without MCM the defaults below are used.

INSTALLATION (MO2)
1. Install this archive with MO2 ("Install a new mod from an archive") and enable it.
2. Restart the game. Safe to add or remove mid-playthrough.

MCM -> "Monolith Voice Greetings" (all settings apply at once, no restart)
  Monolith greetings                - on
  Borrow greeting lines from:
    Sin                             - on
    Military                        - on
    Loners, Clear Sky, Duty,
    Freedom, Bandits, Ecologists    - off
  Other:
    Debug log                       - off

COMPATIBILITY
- Doesn't replace any game script or voice file. Adds sound sections that use the
  vanilla greeting files of the chosen factions, so voice packs that replace those
  files are used too.
- Replaces the vanilla greeting sound (meet_hello) only for stalkers with a Monolith voice.

PERFORMANCE
No per-frame work: one check when a stalker starts a voice line; the borrowed voice is
loaded for a stalker only when he first greets you.

VERSION HISTORY
0.1.0 - first version.

CREDITS
Author: Expand.

# Changelog

All notable changes to **Stellar-Sail**, newest first.

Versions follow `MAJOR.MINOR.PATCH`. Builds for Windows, Linux, and macOS are
released to the [releases page](https://github.com/danieltkach/StellarSail-Web/releases);
existing installations auto-update via Velopack on next launch.

---

## v0.6.1 — 2026-05-13

### Fixed

- After launching from a station, the telescope and other side panels could be
  left showing pre-launch values in the new flight cockpit. They now redraw
  cleanly on the Launch → Flight handoff.

---

## v0.6.0 — 2026-05-13

### Added

- **Real take-off sequence.** Boarding the ship at the atrium drops you into
  the cockpit on the pad with everything powered down — mirror of the
  post-landing shutdown checklist, in reverse:
  - Press **1-6** to power each engine and **7-0** to bring each ship system
    back online.
  - The envelope hint shows what's still blocking launch ("ENGINES: 3/6
    ONLINE", "SYSTEMS: 2/4 ONLINE") and turns green once everything is on.
  - Press **Enter** at the green prompt to enter active ascent.
- **Active ascent.** Climb from the pad through 500m of atmosphere with the
  same drift physics as landing — wind, attitude drift, off-pad clearance
  zone all apply in reverse. Above 500m, the ship hands off to free flight.
- **Press E from the launch pad to disembark** — step back out and decide
  later. Ship stays at the atrium.

### Notes

- No autolaunch counterpart yet — pilot flies the ascent manually.
- Departure clearance comms (a short tower dialog) is still on the to-do list.
  For now, "all systems on" is the only gate before launch.

---

## v0.5.3 — 2026-05-13

### Changed

- **All autosaves removed.** F2 is now the only way to write to disk, and
  works from any mode (not just the station menu). Closing the game without
  pressing F2 means the session's progress is gone. Makes test runs and
  experimentation cleaner; save when you've earned something worth keeping.

---

## v0.5.2 — 2026-05-13

### Fixed

- Hangar dialog lines no longer get truncated with `…`. Long responses from
  the crew chief now word-wrap across multiple rows, indented under the
  speaker prefix. Older messages scroll off the top when the log fills.

---

## v0.5.1 — 2026-05-13

### Fixed

- In Flight mode with the telescope in scan mode, rotating the ship now also
  re-aims the telescope. Previously the two were independent, so centring a
  target in the scanner display didn't actually point the telescope at it —
  leading to confusing "no signal to lock" failures that required toggling
  the telescope off and back on. Lock and Auto modes still pin the telescope
  on the locked target as before.

---

## v0.5.0 — 2026-05-13

### Added

- **Ship and pilot are now tracked separately on the station.** Your ship
  sits in its hangar bay (yellow square on the map) or out on the atrium
  (yellow ▲ on the pad); your pilot avatar (웃 in yellow) shows where *you*
  are standing.
- **Directional navigation around the station.** Arrow keys move the pilot
  between connected locations (Atrium hub → Hangar / Fuel / Repair / Data;
  Data hub → Ops / Trading). The bottom of the screen shows where each
  direction leads and what **Enter** will do at the current location.
- **Real hangar dialog.** Press Enter at the Hangar to talk to Crew Chief
  Vance. Two actions: `[1]` ask the crew to bring your ship to the atrium
  (or take it back to the bay), `[2]` request a status check on hull/fuel/
  battery/engines. Press `[E]` to leave.
- **Service detail views.** Fuel Depot, Repair Bay, Ops Center, Data Library,
  and Trading Post all have their own detail screens with module-themed
  decor. They're "service coming soon" stubs for now — real interactions
  will land in future updates.
- **Take-off pad.** When you press Enter at the atrium with your ship there,
  you get a pre-launch view of the ship on the pad before actually launching.
  Stub for the eventual startup-checklist sequence.

### Changed

- New pilots now start at the station, ship in the hangar bay — same way a
  real day at the spaceport begins. Walk to the hangar, request your ship,
  walk to the atrium, board, launch.
- The old single-letter shortcuts (H, F, R, M, T, D, V) are gone — navigation
  is purely directional now. F2 (save) is the only remaining global key.

---

## v0.4.0 — 2026-05-12

### Added

- **Title screen** with `[R]esume game / [N]ew pilot / [Q]uit`. Boots into
  this menu instead of dropping you straight into the cockpit.
- **Pilot identity.** Each player chooses an 8-character alphanumeric Pilot
  ID + a display name when they create a new pilot. The ID is what the
  station's ATC asks for during the landing handshake (no more hardcoded
  `314159`).
- **Persistent save state.** Each pilot has their own save file. Saved data
  includes pilot identity, credits, ship position/velocity/full attitude,
  fuel, battery, hull damage, engines, systems, telescope state, discovered
  objects, last station, and mission slots reserved for future use.
- **Resume picker** on the title screen when saves exist — pick a pilot to
  continue.

### Fixed

- Multiple cockpit-pixels-bleeding-through-the-title bugs.

---

## v0.3.0 — 2026-05-12

### Added

- **Autoland.** Once you're below 20m altitude during a landing, pressing
  **Enter** engages a hands-off automatic descent that drives every axis
  to zero and lands the ship for you. Engaging while stable gives a clean
  touchdown; engaging while wobbly still completes but the resulting
  envelope violations apply hull damage at touchdown.
- **Momentum-based attitude controls.** Pitch / Yaw / Roll now use angular
  velocity. W/S/Q/E/A/D add to the rate rather than slamming the angle
  directly — same feel as the J/L translation keys. A single well-timed
  correction settles the axis instead of needing to be hammered constantly.
- **Per-pilot landing physics simulator** (developer tool) — drives the
  landing system headlessly with scripted "pilots" and dumps trajectory
  CSVs. Used to retune drift constants so each axis is actually cancellable.

### Changed

- Drift physics retuned. Sinusoidal turbulence dialed way down; steady wind
  bias dominates. Result: corrections you make stick, axes can be settled
  with a sustained tap rate instead of chasing oscillation forever.
- **Live numeric pitch readout** on the landing HUD (the old static "0° ⇕"
  label was misleading — it never moved even when pitch was swinging through
  several degrees).
- **0..500m altimeter** (was 0..100m) with alternating yellow long lines and
  red short connectors. Reads better at altitude and at touchdown.
- Touchdown status label respects the actual outcome (`CRASH` /
  `HARD LANDING` / `TOUCHDOWN`) instead of always saying TOUCHDOWN.
- **Off-pad crash radius** only applies below 25m. Drifting wide at altitude
  is now recoverable instead of an instant ship-destroying clip.

### Fixed

- Ship-icon trail on the landing pad when moving between tile columns.

---

## v0.2.1 — 2026-05-12

### Changed

- **Landing made significantly harder.** Wind amplitudes doubled, gravity
  reduced (longer descents), envelope tightened across the board, attitude
  step from 2° to 1° for finer control. Sloppy landings now actually hurt.
- **Q/E control roll** with very slow auto-recovery, so single inputs persist
  for seconds. Roll matters now.
- Landing HUD's roll indicator finally renders correctly — five overlapping
  draw calls used to wipe each other out.
- Envelope status banner became altitude-aware so you can tell when
  pressing Enter will actually lock the landing.

### Added

- Disembarking requires ALL engines off AND all 4 ship systems off. The
  post-landing checklist now has weight.

---

## v0.2.0 — 2026-05-12

### Added

- **Radio telescope.** Right-column panel renders spectrum + waterfall +
  signal cursor for the telescope. Pulsars, stations, nebulae, and planets
  each have their own spectral profile + time-domain signature — the player
  identifies sources by what they sound and look like, not by automatic
  tagging.
- **Identity-hidden detection.** Detected signals show as `UNKNOWN SOURCE
  / ?` until you lock the telescope onto them; lock then reveals the source
  type, name, and frequency and persists the discovery.
- **Active landing.** No more "press to land and done" — descents are
  real-time, with drift physics on translation and attitude axes that the
  pilot must actively correct. Hard landings carry hull damage; off-pad
  drift can total the ship.
- **Post-landing shutdown checklist.** After touchdown, power each engine
  and ship system off (1-6 then 7-0) before disembarking into the station.
- **Comms-mediated landing clearance.** Lock the telescope onto a station,
  hail them, transmit your pilot ID, get a landing-code assignment, enter
  the code, and only then are you cleared to land.

### Changed

- Telescope gain costs fuel proportional to `gain²`. Cranking it past 3×
  drains power faster than you'd expect.

---

## v0.1.0 — 2026-05-10

### Added

- **First public release.** Renamed from the prototype `CockpitUITest` to
  **Stellar-Sail**.
- **Velopack auto-update** + cross-platform release pipeline. Builds for
  Windows, Linux, and macOS published per tag; existing installations
  auto-update on next launch.

---

## Pre-release development (2025-10 → 2026-05)

The project began life as **CockpitUITest**, a prototype console-based space
sim exploring radio-telescope simulation, realistic ship navigation, and
atmospheric landing mechanics. The work below predates the public release
lineage but is the same codebase that became Stellar-Sail in v0.1.0.

### Cross-platform audio · May 2026

- Audio backend migrated from NAudio (Windows-only) to **Silk.NET.OpenAL**.
  Game now runs identically on Windows, Linux, and macOS. Final prep before
  the first public release.

### Landing HUD · December 2025

- **Landing HUD panel** — bespoke instrument cluster for active descent:
  compass / pitch ladder / roll indicator / altimeter / pad bullseye, all
  rendered in the centre column when in landing mode.
- **Landing input system extracted** from the general flight controls into
  its own dedicated handler. Keeps the descent control surface clean and
  separate from free-flight inputs.

### Architecture refactors · late Nov – early Dec 2025

- **Telescope system** extracted into its own module.
- **Comms system** promoted to a first-class engine system (dialogues, ATC,
  permission states, dialog-tree JSON files).
- **Multi-stage input system refactor** — per-mode input dispatch so each
  control scheme lives in its own file.
- **Border panel system** for consistent bordered UI windows.
- **Project folder structure** reorganised by domain (Panels / Systems /
  Input / Controllers / World / Audio / Communication).

### Engine and resource model · late November 2025

- **Engines as first-class objects** with activation states + heat damage
  + per-engine fuel draw. Six engines per ship, individually controllable.
- **Fuel efficiency model** — telescope gain, engine activity, and other
  ship systems all draw from a shared fuel/power budget.
- **Spatial scanner layout struct** — codifies the radar's geometric
  layout for cleaner per-frame rendering.
- **Ship status fields** (position, speed, propulsion state, info readouts).

### Navigation and instruments · mid–late November 2025

- **2D navigation** with realistic propulsion physics (acceleration,
  inertia, momentum-preserved attitudes).
- **Cockpit animation** — radar sweep, telescope panel pulses, scanner
  twinkle stars.
- **Dashboard + info panel** with live position / heading / velocity
  readouts.
- **Spatial scanner (radar)** with type-coded contact glyphs and
  proximity discovery.
- **Comms beat** introduced — radio communication with stations as a
  prelude to the eventual full comms system.

### Core ship mechanics · mid November 2025

- **Lock-on-target controls** — first working version of "point at a thing,
  press L to lock, fly toward it".
- **Ships factory** + state system.
- **View manager** — cockpit view vs station view as switchable contexts.
- **Initial input system** with dedicated key handlers.
- **Landing sequence (v1)** — first iteration of touchdown logic.

### Audio framework · early November 2025

- **Tone generator** synthesising audio at runtime instead of relying on
  pre-recorded clips. Foundation for the radio-telescope's
  source-driven audio later.
- **Sound providers** for engine, mode switches, lock/unlock, proximity
  warnings.
- **Console display framework** — coordinate-based panel rendering with
  cursor positioning + colour control.

### Foundation · late October 2025

- Initial project scaffold + game loop.
- **Story builder** with multi-ending support — narrative system seeded
  before any flight mechanics existed.

---

[Project on GitHub](https://github.com/danieltkach/StellarSail-Web) ·
Built with C# / .NET 9 / Silk.NET / Velopack.

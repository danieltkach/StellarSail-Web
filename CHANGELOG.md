# Changelog

All notable changes to **Stellar-Sail**, newest first.

Versions follow `MAJOR.MINOR.PATCH`. Builds for Windows, Linux, and macOS are
released to the [releases page](https://github.com/danieltkach/StellarSail-Web/releases);
existing installations auto-update via Velopack on next launch.

---

## v0.9.7 — 2026-05-16

### Changed — manual mission delivery

Mission completion is no longer a silent auto-payout on touchdown.
The pilot now finishes a contract in **three explicit steps**:

1. **Land** at the destination station. Messages panel says
   `Destination reached — file your manifest at the Ops Center to
   get paid.` No credits yet.
2. **Ops Center → `[4]` Report cargo for active contract.** Dax
   validates the manifest (right station, cargo on board for
   Prospect runs) and flips the contract to *DeliveryApproved*.
   Green `✓ DELIVERY APPROVED` toast.
3. **Hangar → `[3]` Unload mission cargo (collect payment).** Vance
   unloads the cargo (Prospect runs lose `RequiredCargoUnits` from
   the hold; CargoHaul cargo is abstract). Credits hit
   `Pilot.Credits`. Mission moves to `MissionsCompleted`. Green
   `✓ +N¤ CREDITED` toast.

Save schema: `Mission.DeliveryApproved` bool added. Older saves
load as `false` — in-progress contracts are treated as still in
transit, which is the safe cautious default.

### Fixed

- **Direction-readout bleed-through.** The scanner panel showed
  `(↻ 1°, ⊤ 0°)°)°)` when the ship's azimuth or polar shrank from a
  longer to a shorter number — trailing chars from the previous
  print survived. Both rows now use fixed-width number formatting +
  trailing padding so older content always gets overwritten.

---

## v0.9.6 — 2026-05-16

### Added

- **Five swappable title-screen themes.** Pilot can flip through
  visual variants of the "STELLAR SAIL" title with the F-keys:
  - **F5** — Default cyan (the original).
  - **F6** — Dark blue letters, dark-yellow tagline, three orange
    `✦` accents around the title.
  - **F7** — Magenta shaded — letters rendered with `▓` (medium
    shade) instead of `█`, dotted underline.
  - **F8** — Yellow + stars — yellow letters with a scattered field
    of `✦` / `✸` / `·` around the block.
  - **F9** — Green + ship — green letters with a small ASCII ship
    silhouette flying off the right edge.
  - Theme persists across sub-screens (main menu, pilot picker, ID
    entry) until the player picks another or starts the game. Pure
    cosmetic — no game-state effect.

---

## v0.9.5 — 2026-05-16

### Refactor (intended invisible)

Mode-transition rendering is now exclusively the View dispatcher's
job. The old `DrawCockpit`, `DrawInitialLayout`, and `OnTitleHandoff`
trio are gone. Every input handler that flips `Mode` just flips
`Mode` — the dispatcher runs the destination view's `Activate`
which does `Console.Clear` + paints.

### Side benefit / probable fix

- **`POST-LANDING SHUTDOWN` + `AUTOLAND ACTIVE` bleed-through.** Two
  modes used to render at once because the Landing → LandingShutdown
  transition didn't invalidate `LandingHud`'s dedupe trackers; the
  envelope text "▼ AUTOLAND ACTIVE ▼" stayed on screen. The View
  refactor makes `LandingShutdownView.Activate` run `LandingHud.
  ForceRedraw`, so the stale text is overwritten with the correct
  post-touchdown label.

---

## v0.9.4 — 2026-05-16

### Added

- **Toast banner for save.** Press F2 and a floating `✓ GAME SAVED`
  badge appears at the top-center of the console for two seconds,
  then clears itself. Failed saves get `✕ SAVE FAILED` for a few
  seconds in red. The classic Messages-panel log line still records
  the position too. The same toast plumbing is reusable — upcoming
  mission-credit notifications will land on it.

### Refactor (no observable change intended)

- View-architecture work, mid-flight. Adds a `View` abstraction
  (`Core/View.cs`) + `ViewRegistry`, 14 concrete subclasses (one per
  ControlMode), and a main-loop dispatcher that runs
  `View.Activate / View.Deactivate` on mode transitions. The station-
  detail Exit handlers (Hangar / FuelDepot / RepairBay / OpsCenter /
  DataLibrary / TradingPost) and the PDA exit no longer do their own
  `Console.Clear + panel.Draw` — the dispatcher does it. Cockpit-
  internal transitions and the title handoff are next.

---

## v0.9.3 — 2026-05-16

### Fixed

- **Asteroid takeoff actually works now.** LaunchPad's clearance gate
  used to demand ATC sign-off on *every* takeoff. Asteroids have no
  tower, so the pilot could only escape via a back-door that left the
  ship in a weird state. Non-station bodies now skip the clearance
  check entirely — `Enter` lifts off when engines + systems are on.
  `H` on an asteroid LaunchPad just prints `No tower to hail — lift off
  when ready (Enter).` instead of opening a doomed comm panel.
- **"Space does nothing after the weird takeoff."** Root cause: thrust
  warmup only ticks while `Ship.Mode` (the physics mode) is `Flight`.
  An old path — `AbortLanding → Communication → Esc to Flight` — set
  the control mode but left the physics mode at `Landing`, so the
  pilot ended up in space unable to thrust. Comms-exit-to-Flight now
  syncs both.

### Changed

- **Asteroid A-1138** — renamed from `Asteroide A-1138`. Per the
  project's American-English convention.

---

## v0.9.2 — 2026-05-16

### Fixed

- **Nav Calc RPN calculator was painting past the panel bottom.** The
  layout placed the input + status 5 rows past the body, so anything
  you typed went off-screen — the "I can't type" symptom was actually
  "your input is visible but in the gutter." PDA panel bumped from 29
  to 32 rows, calculator stack reduced from 4 to 2, calc starts right
  under the formulas. Input + status now sit safely inside the body.
- **Esc now clears the error first.** Two-stage: pressing Esc with an
  error/result on the status line clears that and stays focused.
  Pressing Esc again (status empty) leaves calc focus like before.
- **Typing or backspacing clears a stale error.** Any character
  appended or removed wipes Status. Moving forward in your expression
  doesn't have to step over the previous mistake.

---

## v0.9.1 — 2026-05-16

### Fixed

- **Scanner painted on top of the station map on fresh start.** The
  v0.7.9 defensive redraw inside `DiscoverySystem` was unconditional,
  but a fresh new pilot at home `(0, 0, 0)` proximity-discovers the
  home station on the very first frame *while sitting in
  StationMenu mode* — the redraw painted the radar on top of the
  station map. Discovery now only repaints the scanner when the
  scanner is actually the visible foreground (`Mode == Flight`).
  Discovery itself, the reward, and the log line still happen in
  every mode.

---

## v0.9.0 — 2026-05-16

### Added — asteroid landing + surface gameplay

A whole new place to go and a whole new thing to do once you're there.

- **New world entries**:
  - **Piedrabuena Station** at `(1200, 200, 0)` — a third station,
    cargo and passenger hub.
  - **Asteroide A-1138** at `(1000, 2000, 3000)` — an uncatalogued
    rocky body, landable but uninhabited.
- **Silent hail.** `H` now opens comms on any locked target, not just
  stations. Asteroids and other non-station bodies return silence:
  `...no response.` The channel stays open.
- **Manual descent.** After a silent hail, the comm panel offers
  `[D] Attempt manual descent` — no ATC, no landing code, drops you
  straight into active landing physics 200m above the target.
- **Asteroid surface mode.** After shutting down on a non-station body,
  the pilot disembarks onto a 5×5 grid of cells:
  - `WASD` / arrows to move the pilot avatar between cells.
  - `R` scans the cell you're standing on. Cell contents are
    hidden until scanned (rendered as `?` inside a dim border).
    Distribution per asteroid is deterministic from the body's name:
    ~50% Empty, ~25% Debris, ~15% Ore, ~10% Hazard.
  - `X` extracts whatever's there: Debris drops 1–3 cargo units,
    Ore 3–6 units. Hazard pays similarly but charges 8% hull stress.
  - `E` returns to the ship's launch pad to take off again.

### Added — Prospect mission type

Mission board now has two kinds of contracts:

- **Cargo Haul** (existing) — station-to-station; completes on
  touchdown at the destination station.
- **Prospect** (new) — pays you to bring N units (8–15) of cargo from
  a named asteroid back to a station. Each asteroid in the world
  generates one prospect listing on the Ops Center board. Pays
  roughly `2× distance + 500¤ danger bonus + 0..400 random` —
  meaningfully more than a regular haul because of the no-ATC
  landing, surface survey, and hazard exposure.

### Save schema

- `Mission` gains a `Type` field (`CargoHaul` | `Prospect`) and a
  `RequiredCargoUnits` int. Older saves without them deserialize as
  `CargoHaul` / `0`. Backwards compatible.

---

## v0.8.3 — 2026-05-15

### Changed

- **Nav Calc no longer gives the answer.** The PDA's Nav Calc tab
  used to print the required azimuth, polar, distance, and turn-
  deltas for free. That violated the "don't give it away" design
  rule. The tab now shows only the **raw inputs** (ship and target
  XYZ, plus `Δx`, `Δy`, `Δz`) and the **formulas**:
  ```
  azimuth  = atan2(Δx, Δy)
  polar    = atan2(Δz, sqrt(Δx² + Δy²))
  distance = sqrt(Δx² + Δy² + Δz²)
  ```
  Pilot does the math themselves.
- **Built-in RPN calculator** at the bottom of the Nav Calc tab.
  Token-based: type `300 200 atan2`, get the answer. Trig in/out
  degrees throughout. Supports `+ - * / sin cos tan asin acos atan
  atan2 sqrt sq abs neg pi e dup drop swap clear` plus a four-deep
  stack readout and an eight-line history. Press `F` to focus the
  calculator (typing routes to its input buffer), `Esc` to leave
  focus.

### Test scenario

- **World temporarily stripped to two stations** for clarity:
  - Home: **Base Puerto San Julián** at `(0, 0, 0)`
  - **Glaciar Perito Moreno** at `(300, 200, 350)` — 510 km from
    home, all three axes non-trivial, designed to actually exercise
    the 3D nav math.

---

## v0.8.2 — 2026-05-15

### Changed

- **Tab-cycle target selection is now scoped to the inner radar ring.**
  Only contacts within `1000 km` (1/3 of the current `3000 km` radar
  range) are eligible for the selection bracket. Far-rim blips stay
  reachable via the radio/frequency lock path, but `Tab` is now a
  "things I can fly to right now" tool — not "everything visible on
  the radar." If nothing is in range, the messages panel says
  `No contacts within 1000 km — fly closer.` instead of doing nothing.

---

## v0.8.1 — 2026-05-15

### Added

- **Tab-cycle target selection.** Press `Tab` in Flight to cycle
  through visible radar contacts, sorted by distance (nearest first).
  The selected target gets a static `> · <` yellow bracket — distinct
  from the animated lock brackets. `L` locks the *selected* target
  directly, bypassing the frequency-tuning ceremony, so the lock no
  longer feels random. A message confirms what you're about to lock
  before you press `L`.
- The original frequency-based lock path is preserved as a fallback
  for when nothing is Tab-selected. Comms hailing and radio
  signal-hunting still work as before.

### Fixed

- **Locked object reverted to `?` after flying past and back.** When
  `L` locked an object, the scanner only repainted on the next ship
  motion — so a stationary pilot kept seeing the stale unknown blip
  even though the object was now known. Lock now triggers a scanner
  redraw immediately. Same defensive redraw added to proximity
  discovery so newly-rewarded discoveries flip to their real glyph
  in lock-step with the reward message.

---

## v0.8.0 — 2026-05-15

### Added — discovery payoffs

Discovery is no longer a flat "INFO ACQUIRED" log entry. First
proximity contact with an unknown object now grants a per-type
reward, so chasing any `?` becomes a real decision.

| Type | Payoff |
|---|---|
| Comet | +25¤ "scientific data" |
| Wreck | salvage 1–3 random cargo units |
| Derelict | salvage 3–8 random cargo units |
| Anomaly | +1 intel point |
| Beacon | reveals the nearest still-hidden object within 1500 km onto your chart — "Beacon X transmits coordinates of Y" |
| Pulsar | +10¤, frequency catalogued |
| Black Hole | +50¤ + 2 intel — **but** hull stress: −5% (capped) |
| Station | +5¤ |
| Others | +5¤ generic chart bonus |

AlwaysVisible objects (charted from the start) still log the older
`INFO ACQUIRED` line with no payout — they weren't actually
discovered.

### Added

- **Intel points** — a new resource on the pilot, reserved for the
  future puzzle/intel-exchange chart-unlock system. Accumulates from
  anomaly and black-hole discoveries; shows on the PDA Cargo tab
  below your credits. Save schema bumped to round-trip it; older
  saves load with 0.

### Notes

- Salvage rolls are seeded on the object's name, so a given wreck
  always drops the same loot (helps debugging and would-let-you-replay).

---

## v0.7.9 — 2026-05-15

### Changed

- **"Mostly question marks near home" — backfilled the originals.**
  v0.7.8 fixed the new-type icon rendering, but the radar near home
  still looked sparse because the 224 pre-v0.7.0 objects had no
  `alwaysVisible` field at all and defaulted to false. With only the
  400 newer objects split 50/50, the global ratio was 31% visible /
  69% hidden — and the originals dominate the near-home zone where
  the pilot spawns. Flipped roughly half the originals to
  `alwaysVisible: true` (home station forced true unconditionally).
  New global ratio is ~47% visible / 53% hidden, and the count of
  visible objects within 3000km of origin rose from 23 to 62.

---

## v0.7.8 — 2026-05-15

### Fixed

- **"All question marks on the radar" — root cause finally found.**
  `ContactInfo.GetRadarSymbol`'s `switch` on `Object.Type` was written
  before the v0.7.0 new types existed; for `Comet`, `BlackHole`,
  `Wreck`, `Anomaly`, `Beacon`, `Derelict`, `IceField`, and
  `RadioCloud`, it fell through to `'?'`. So even objects flagged
  `AlwaysVisible` rendered as `?` on the radar — they *were* being
  treated as known, but their glyph happened to be `?`, exactly the
  same character used for unknown blips. The discovery gate was
  never the bug. Added the eight new types to the switch so they
  render with their type-default glyph (`☄`, `●`, `✕`, `✦`, `⚐`,
  `⌬`, `❄`, `≋`) at close range.

---

## v0.7.7 — 2026-05-15

### Changed

- **Multi-color blink patterns for unknown radar blips.** Each `?`
  blip now cycles through a hashed pattern of 1–5 color slots.
  Slots can be any of 11 palette colors, or blank (the blip
  disappears for that fraction of its cycle). Pattern length and
  per-slot colors are independent hashes of the object name, so
  adjacent blips look genuinely different — one might be a steady
  cyan, another flashes red/yellow/red/yellow, another vanishes
  briefly. Old 6-color bright/dim pulse retired.

### Added (diagnostic)

- **Position logged on save and load.** F2 now reports
  `Game saved (pos X, Y, Z).`; Resume reports
  `Save loaded (pos X, Y, Z, mode M).` Helps verify whether a
  reported "came back at origin" bug is a save bug or a load bug.

---

## v0.7.6 — 2026-05-15

### Changed

- **Per-object blink rate for unknown radar blips.** Each `?` now has
  its own pulse period, hashed from the object's name into one of
  twelve discrete rates spread across `0.4..1.6 s`. Neighboring blips
  beat noticeably out of sync, so the radar looks like a field of
  independent blinking lights instead of one synchronized strobe.
  The period hash is seeded separately from the color hash, so an
  object's color and blink rate don't correlate.

---

## v0.7.5 — 2026-05-15

### Fixed

- **Unknown blips looked identical to known objects.** The pulse's dim
  phase used `◌` (U+25CC DOTTED CIRCLE), which renders the same as
  `○` (U+25CB WHITE CIRCLE) — the glyph the scanner uses for known
  objects at medium/far range bands. So a radar full of mixed knowns
  and unknowns read as 100% question marks. Pulse is now color-only;
  `?` stays `?` in both phases, with the per-object hashed color
  flashing bright → dim.
- **Radio telescope header rows blank after takeoff.** The panel uses
  differential paint with cached trackers (`_lastFreq` etc.); after
  the `Console.Clear` in `HandoffToFlight`, the cached values still
  matched current state, so all four header rows (FREQ / BW / GAIN /
  SNR) silently skipped redraw. Added `RadioTelescopePanel.ForceRedraw`
  that resets the trackers, and switched the two callers to use it.

---

## v0.7.4 — 2026-05-15

### Added

- **PDA Navigation Calculator** — a fifth tab. Lists your charted
  targets (home + everything in your DiscoveredObjects); for the
  highlighted target it shows distance, the **required** azimuth and
  polar to point the ship at it, your **current** azimuth and polar,
  and the **signed shortest turn** to get from one to the other.
  - The Az convention matches the game: `0°` points to `+Y` (north),
    increasing clockwise; polar is `-90°` straight down, `+90°`
    straight up.
  - `↑`/`↓` (or `W`/`S`) pick the target inside the Nav Calc tab.
    Same keys still scroll on other tabs.
- This is a **calculator, not a compass needle.** It doesn't reveal
  any uncharted positions and it doesn't auto-steer — it just does
  the atan2 math so the pilot can actually point the ship.

---

## v0.7.3 — 2026-05-15

### Fixed

- **Empty radar after flying.** With the world expanded to 624 objects
  spread across `±5000 km`, the old `600 km` scanner range left the
  radar effectively empty everywhere except right next to home —
  expected object count inside the radar sphere was about half an
  object. Bumped to `3000 km`, restoring the dense radar feel.
- **`?` blips not pulsing when stationary.** The scanner only refreshed
  on ship motion, so unknown blips appeared frozen if the pilot wasn't
  moving. Pulse animation now runs every frame and only actually writes
  when the bright/dim phase flips (~2 writes/sec per blip).

---

## v0.7.2 — 2026-05-15

### Added

- **PDA Charts tab.** Press `P` in Flight, then `[4]`, for a top-down
  2D map of the slice of the world you've actually charted. Home is
  always marked; every other entry is something you discovered by
  flying close to it. Stations get a short name label; everything else
  is icon-only. Your ship sits on the chart as a white `✦`.
- **Charts is deliberately incomplete.** Nothing auto-appears just
  because it exists in the world. Mission acceptance does *not* reveal
  the destination — knowing the name isn't knowing the position.
  Future releases will add intel-exchange paths (riddles, NPC
  dialogs, side-missions) for unlocking chart entries by gameplay
  other than direct proximity.

### Changed

- **Contract tab's navigation line** now reflects the charted state:
  if the destination is on your chart it says so; if not, it tells
  you you'll need to discover it or earn intel.

---

## v0.7.1 — 2026-05-15

### Added

- **PDA — pilot data assistant.** Press `P` in Flight to open. Three
  tabs:
  - **Contract** — your active cargo haul: origin, destination,
    cargo description, payment, plus a navigation note about
    finding the destination on a star chart.
  - **Cargo** — your hold inventory by item type, plus current credits.
  - **Logbook** — completed contracts (human-readable) + discovered
    objects (alphabetical). Scrolls with `PgUp`/`PgDn`.
  - Charts tab arrives in v0.7.2 — for now the PDA Contract tab tells
    you to consult it once it ships.
- **250 more objects in the world** (374 → 624 total). Same 50/50
  known/hidden split, same ZX/ZY profile (118 of 250 with |Z| ≥ 2000).
  The cube now reads as genuinely populated.

### Changed

- **Unknown radar blips now have per-object colors and a stronger
  pulse.** Each `?` blip is hashed to one of six accent color pairs
  (cyan, magenta, green, yellow, red, white) keyed off the object's
  name. Distance is no longer encoded in unknown-blip color — that's
  reserved for known blips. The radar reads as a varied chromatic
  field instead of a three-band gradient. Pulse cadence raised from
  1 Hz to ~1.4 Hz so the heartbeat is clearer.

---

## v0.7.0 — 2026-05-15

### Added

- **150 new objects in the world** (224 → 374 total), with explicit
  density across the ZX and ZY planes — 87 of the new ones placed
  with |Z| ≥ 2000, so the radar isn't a flat equatorial belt anymore.
- **Mixed visibility.** New `AlwaysVisible` flag on each space object.
  When true, the scanner shows the real icon + type-coded color
  regardless of discovery state. When false, the existing
  discovery-gated behavior applies (a pulsing `?` until you fly
  close enough). The 150 new objects split exactly 50/50 — half
  visible-from-start, half hidden-until-proximity.
- **Eight new object types.** Each gets a distinct icon + color:
  - Comet (`☄`, cyan)
  - Black Hole (`●`, dark red)
  - Wreck (`✕`, dark yellow)
  - Anomaly (`✦`, green)
  - Beacon (`⚐`, white)
  - Derelict (`⌬`, dark gray)
  - Ice Field (`❄`, dark cyan)
  - Radio Cloud (`≋`, dark green)
- **Distinct discovery message for hidden objects.** First encounter
  with a previously-`?` object now logs as `DISCOVERED: <name>`
  (vs the existing `INFO ACQUIRED: <name>` for pre-charted objects)
  — louder, marks the moment.

---

## v0.6.9 — 2026-05-15

### Fixed

- **Takeoff without clearance.** ATC clearance was a session-sticky
  flag — once you got cleared at any station, the next takeoff from
  any other station also passed the gate without hailing the tower.
  Clearance now invalidates on every successful touchdown and again
  on a successful ascent.
- **Pilot ending up far from station after launch.** A unit mismatch
  in the Launch physics layer meant the 500m altimeter readout was
  integrating as 500 km in world coordinates — plus a few seconds of
  horizontal wind drift, the pilot dropped out of Launch hundreds of
  km from their origin station. Fixed by snapping the ship to a clean
  post-launch state (≈100m above origin, velocity zero) at handoff;
  same as what `LandingSystem` already did at touchdown.

---

## v0.6.8 — 2026-05-15

### Removed

- **Dead `ServiceDetail` code dropped.** The shared "coming soon" stub
  that early builds used for the five non-Hangar station services had
  zero callers after v0.6.7 shipped the last real detail view. The
  panel, input system, control mode, and all references are gone —
  ~170 lines lighter, behavior unchanged.

---

## v0.6.7 — 2026-05-15

### Added

- **Trading Post is now a real detail view** — completing the set; all
  six station modules now have full panels. Broker *Quill* shows a
  market quote table for five cargo items (Fuel Cells, Machine Parts,
  Biocultures, Medical Supplies, Scrap Metal). Each row lists the
  station's per-unit buy price (station → you), sell price (you →
  station), and how many of that item you're carrying.
- **Per-station, per-item pricing.** Base prices fall in `8¤..60¤`,
  derived from a station+item hash, with a 10% spread between buy and
  sell. Each station naturally favors different items — that's what
  makes buy-low-sell-high routes viable.
- **Cargo hold inventory.** Pilot now has a `CargoHold` keyed by item
  type; save schema rounds it across sessions. Older saves load
  cleanly with an empty hold.
- Trading controls: `↑`/`↓` highlight item; `B` buys 1, `Shift+B`
  buys 10; `S` sells 1, `Shift+S` sells 10; `E` exits. Partial
  buys/sells if credits or stock run short.

---

## v0.6.6 — 2026-05-15

### Added

- **Data Library is now a real detail view.** Walk in from the
  station map and browse three categories with `[1]`/`[2]`/`[3]`:
  - **Star Charts** — six short author-written region notes covering
    the Belt, the North/South/East/West quadrants, and the Beacon Pair.
  - **Lore** — seven backstory entries about the Merchant Pact, the
    pilot guilds, the abandoned relay, the long-haul trade, the
    Founding, common cargo, and the credit (¤) system.
  - **Logs** — your personal logbook, auto-built from your completed
    contracts and the objects you've discovered. Fills as you play.
- Navigation: `↑`/`↓` (or `W`/`S`) move the entry selection;
  `PgUp`/`PgDn` (or `J`/`K`) scroll a long body; `E` exits.

### Changed

- **Cargo-haul completions are now human-readable in your logbook.**
  Completed contracts log as `Cargo haul A → B — 247¤` instead of as
  opaque GUIDs, so the Logs view actually means something.

---

## v0.6.5 — 2026-05-14

### Added

- **Ops Center is now a real detail view.** Walk into Ops from the
  station map and dispatcher *Dax* offers three procedurally-generated
  cargo-haul listings. Each lists a destination station, the cargo
  description, and a payment that scales with distance (+ a small
  random bonus). Accept one with `[1]`, `[2]`, or `[3]`; only one
  contract active at a time. The mission board re-rolls every time
  you re-enter Ops.
- **Cargo-haul completion on touchdown.** Land at the destination
  station and the payment is credited to your account automatically.
  Crashes don't qualify — destroyed ships don't deliver cargo. The
  active contract persists across saves.

### Fixed

- **Cockpit panel-rendering pass.** The transition from launch pad to
  the departure-comms screen was leaking `AWAITING CLEARANCE — [H]
  HAIL TOWER` text into the comm panel's header line. Root cause: the
  `Draw()` call on a panel that takes over a shared cockpit slot
  doesn't clear leftover cells the new panel doesn't touch. Fixed at
  the source plus three more transition points found in the same
  sweep (LaunchPad → Launch, Landing → Communication via abort,
  Launch-crash → LandingShutdown).

### Changed

- **Project-wide spelling pass to American English.** All British
  spellings (`colour`, `centre`, `cancelled`, etc.) replaced with the
  American forms. One player-visible string changed
  (`Landing approach cancelled` → `canceled`); the rest were comments.

---

## v0.6.4 — 2026-05-14

### Added

- **Repair Bay is now a real detail view.** Press Enter at the Repair
  node on the station map to walk into the welding shop. Welder *Greta*
  shows the hull integrity gauge (color-shifted by damage tier),
  current credits, and a per-station repair price. Actions [1]/[2]/[3]/[4]
  weld the hull back to 25/50/75/100% integrity; if credits run short,
  Greta does as much as the money will buy and tells you the new
  integrity level.
- **Per-station repair pricing.** Each station has a stable cost in the
  range `2.00¤..5.00¤ per damage-point`, derived from a different hash
  seed than fuel pricing — so a cheap-fuel port is not automatically a
  cheap-repair port. A 100-damage full repair therefore ranges from
  200¤ to 500¤.

### Changed

- **Project-wide spelling pass to American English.** All British
  spellings (`colour`, `centre`, `cancelled`, etc.) replaced with the
  American forms. One player-visible string changed
  (`Landing approach cancelled` → `canceled`); the rest were comments.

---

## v0.6.3 — 2026-05-14

### Added

- **Fuel Depot is now a real detail view.** Press Enter on the Fuel node
  from the station map to walk into the depot. Attendant *Mara* greets
  you with flavor that depends on the station's price tier (cheap,
  mid-range, or surcharge), and the panel shows your tank gauge, your
  credits, and the per-unit price. Numbered actions top the tank up to
  25/50/75/100%; if your credits don't cover the full top-up, Mara
  fills as much as you can afford and tells you the new tank level.
- **Per-station fuel pricing.** Each station now has a stable, distinct
  fuel price in the range `0.30¤..1.00¤ per unit`, derived
  deterministically from its name (no save-game footprint). The
  difference between cheapest and dearest is over 3×, so a full tank
  ranges from 300¤ to 1000¤.

### Fixed

- Station-map view's outer border no longer collides with the bottom
  nav hint — the panel grew by one row so the frame line, hint, and
  rectangle border each sit on their own row.

---

## v0.6.2 — 2026-05-14

### Added

- **Departure clearance comms.** On the launch pad, press **H** to hail the
  tower and run a short dialog with the station's ATC. Until clearance is
  granted, the envelope hint reads `AWAITING CLEARANCE — [H] HAIL TOWER`
  and Enter refuses to launch. Mirror of the landing-side clearance flow,
  with per-station personality flavor.

### Changed

- **Ship no longer auto-parks after disembark.** When you shut down and
  step off the ship, it stays on the atrium pad where you touched down.
  Walk to the hangar and tell Vance ("Bring my ship into the bay") to
  stow it. Returning trips work the same way in reverse — the move
  between bay and atrium is always explicit.

### Fixed

- Messages panel no longer overflows past its right border. Long lines
  are truncated with an ellipsis instead of bleeding into adjacent panels.
- Identical consecutive messages are dropped (no more four-deep repeats
  of "Cannot launch — awaiting departure clearance from tower.").
- Station map view gains a matching bottom frame line — the layout now
  mirrors the top.

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
  rendered in the center column when in landing mode.
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
  cursor positioning + color control.

### Foundation · late October 2025

- Initial project scaffold + game loop.
- **Story builder** with multi-ending support — narrative system seeded
  before any flight mechanics existed.

---

[Project on GitHub](https://github.com/danieltkach/StellarSail-Web) ·
Built with C# / .NET 9 / Silk.NET / Velopack.

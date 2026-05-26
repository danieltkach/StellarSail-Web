# Changelog

All notable changes to **Stellar-Sail**, newest first.

Versions follow `MAJOR.MINOR.PATCH`. Builds for Windows, Linux, and macOS are
released to the [releases page](https://github.com/danieltkach/StellarSail-Web/releases);
existing installations auto-update via Velopack on next launch.

---

## v0.22.8 — 2026-05-25

### Changed — SJ city: Av. San Martín widens, Maruja's ↔ Tostadora swap

- **Av. San Martín** now renders with a real median strip — top `═══`
  lane, blank middle row with "AV. SAN MARTÍN" centered in it, bottom
  `═══` lane. Pellegrini crosses both lanes with a `╪` at each, the
  median between them stays open.
- **La Tostadora Moderna** moved to Col 1 (Pellegrini foot); **Maruja's**
  moved to Col 2. Row 4 is now Mercado → Maruja's → Plaza España →
  Cascada.
- Rows 2/3/4 shifted down one to absorb the new avenue median row,
  preserving spacing.

---

## v0.22.7 — 2026-05-25

### Changed — SJ city: layout polish

- **Cascada** moved further down to Row 4 (was Row 3) so it sits at
  the bay edge in the south band.
- **Plaza España** shifted one column to the right (Col 4 instead of
  Col 3) to match the pilot's map.
- **Casa Maruja** renamed to just **"Maruja's"** (was "Maruja's Piano
  School").
- Description text dropped one row (now y=22) for visual breathing
  room between the bottom building row and the dialog.

Graph edges updated: Plaza España now connects up to Argensud and
right to Cascada; Cine Talia no longer reaches Cascada horizontally
(walk down via Argensud instead).

---

## v0.22.6 — 2026-05-25

### Changed — SJ city: description auto-shows for current place

In Base Puerto San Julián, the dialog pane now always shows **only**
the description for the cell the pilot is standing on. Walking with
the arrow keys auto-replaces the text — previous descriptions never
linger.

- Dialog moved from a 4-entry scrolling log to a single-entry display.
- Each arrow press auto-shows the new place's description (third-person
  narration or NPC line).
- Enter is now reserved for *real* interactions. Today that's just the
  market door at **La Tostadora Moderna** (opens the Trading Post).
  Future patches will hang home-rest and NPC mini-dialogs off it.

---

## v0.22.5 — 2026-05-25

### Added — ESC cancels the pending END quit

Press END to arm the quit-confirmation window (5 seconds), and any of
these now work:

- **END again** within the window → quits as before.
- **ESC** within the window → **cancels** the pending quit (new). The
  ESC is swallowed so it doesn't double-fire on whatever panel happens
  to be open.
- Otherwise the window expires silently after 5 s.

Toast and message wording updated to mention ESC as the cancel option.

---

## v0.22.4 — 2026-05-25

### Changed — San Julián city: hand-drawn map layout, 16 locations

Re-laid out the city to match the pilot's own hand-drawn map of Puerto
San Julián. Av. San Martín runs **horizontally** as a thick double-line
avenue across the upper-middle; Pellegrini runs **vertically** along
the western edge; the Bahía San Julián curves the east edge.

**New locations:** Laguna Seca, Camping, Muelle Nuevo, Centro
Artesanos, Monumento a la Primera Misa, Salón de Arcades, Muelle
Viejo, Cine Talia, La Cascada, Plaza España.

**Removed:** Hospital, Faro, Museo Nao Victoria, Municipalidad, Banco,
Gimnasio, Heladería (not on the pilot's working map of the town).

**Renamed:** Cine-Teatro → **Cine Talia** (the real Patagonian
cinema's name), Plaza San Martín → **Plaza España** (Pellegrini-side
plaza per the pilot's map).

**Repositioned:** La Cascada moved to the bay edge directly below
Muelle Viejo (its real geography); Muelle Nuevo sits in the north band
between Camping and Centro Artesanos.

The map now renders the avenue as a thick `═` double-line with the
label between the two lines, Pellegrini as dashed `:` verticals
crossing the avenue at a `╪` intersection glyph, and the bay as a
`~~~` coastline on the east edge.

---

## v0.22.3 — 2026-05-25

### Changed — San Julián city: streets cross (Av. SM × Pellegrini)

Av. San Martín and Pellegrini are **perpendicular** in the real
Puerto San Julián, not parallel. The previous layout had them as
parallel horizontal bands — fixed.

The new map:

- **Av. San Martín** runs vertically through column 2 (Municipalidad,
  Iglesia Sagrado Corazón, Plaza, Gimnasio).
- **Pellegrini** runs horizontally across row 3 (Casa Maruja, Plaza,
  Pellegrini 1285 — the pilot's home, Hospital).
- **Av. Costanera** runs vertically along column 4 (Faro,
  Nao Victoria, Hospital — bay-side).
- **Plaza San Martín** sits at the **intersection** of Av. San Martín
  and Pellegrini — faithful to the real grid.

The map renders all three streets as dashed overlays with labels at
their ends so the geometry is visually obvious.

### Fixed — City map crash on certain direction hints

`SanJulianCityPanel.DrawEntranceHint` was crashing with an
`ArgumentOutOfRangeException` at cells whose four-direction hint plus
the action label exceeded panel width — the centered-x coord went
negative. Now: hint is truncated to inner width before centering, and
the start coord is clamped to ≥ 1. Direction hints also use shorter
location nicknames (e.g., "Pellegr. 1285", "Tostadora") to fit on one
line.

---

## v0.22.2 — 2026-05-25

### Changed — San Julián city now matches real Puerto San Julián geography

The 15-location city map has been re-laid out against actual street
data for the real Puerto San Julián in Santa Cruz, Argentina (parish
records of the Diócesis de Río Gallegos, the argentino.com.ar
municipal directory, and the Codigo Postal street index were the
source).

**Real-world corrections applied:**

- **Iglesia** renamed **Parroquia Sagrado Corazón de Jesús** (its
  actual dedication; sits at Av. San Martín 235).
- **Hospital Distrital Dr. Miguel Lombardich** moved from the
  institutional row to **Av. Costanera** — its real address.
- **Museo Regional** replaced by **Museo Temático Nao Victoria** —
  the real full-size replica of Magellan's flagship at "punto cero"
  of Patagonia, on the costanera.
- **Argensud Cultural** flavor now reflects that the real venue is
  a restaurant in the historic former general store.
- **Municipalidad** added at the civic row (Av. San Martín 165 esq.
  Zeballos — its real address).
- **Teatro** renamed **Cine-Teatro** to reflect typical
  small-town-Patagonia usage.

**Layout** is now organized as four bands faithful to the actual
street grid: civic / commercial north of Av. San Martín → plaza belt
on Av. San Martín → Pellegrini (residential, including Pellegrini
1285) → Av. Costanera (bay-side).

---

## v0.22.1 — 2026-05-25

### Added — Trading Post reachable from inside the city

City-bearing stations no longer drop trading from the loop. The
Trading Post is now reachable as a building **inside** the city:

- At **Glaciar Perito Moreno**, `[Enter]` on the **Market** opens
  Quill's floor.
- At **Base Puerto San Julián**, the Mercado is renamed
  **"La Tostadora Moderna"** and serves the same purpose — `[Enter]`
  there opens the Trading Post.

`TradingPostPanel.ReturnMode` (new field) captures which mode entered
trading, and exit restores it — so finishing a transaction inside a
city walks the pilot back onto the city floor instead of all the way
to the station map. Piedrabuena's station-tile entry keeps the
original "back to StationMenu" behavior unchanged.

---

## v0.22.0 — 2026-05-25

### Added — Base Puerto San Julián city map (real-world Puerto San Julián, Argentina)

A second walkable city behind a station's atrium, mirroring the Glaciar
Perito Moreno pattern shipped earlier. Inspired by the real Puerto San
Julián in Santa Cruz Province, Argentina.

Fifteen sub-locations across four bands:

- **Institutional (north)** — Hospital Padre Quinteros ✚ · Iglesia
  San José ✟ · Museo Regional ◇ · Faro / Mirador ★
- **Av. San Martín (commercial spine)** — Casa Ingrid ❀ (gifts) ·
  Plaza San Martín ✦ (hub) · Restaurante Argensud ⌖ · Gimnasio
  Municipal ⚒
- **Pellegrini (residential)** — Maruja's Piano School ♬ · **Pellegrini
  1285 ⌂ (the pilot's home)** · Mercado Central ⊞ · Banco Santa Cruz $
- **Costanera (waterfront)** — Heladería ✱ · Costanera · Nao Victoria
  replica ≈ · Cine-Teatro ♪

Same idiom as Glaciar — the Trading Post slot in the station map is
repurposed as a City Gate ↗ that walks the pilot into the city; the
StationMenu arrow hints relabel `Trading` → `City Gate`. Trading still
works at Piedrabuena Station (the only station of the three Pact
charters that keeps its market).

Each location currently shows flavor dialog on `[Enter]` — mechanical
effects (sleeping at home for a save, taking piano lessons for a
skill bump, etc.) land per-location in follow-up commits.

---

## v0.21.1 — 2026-05-25

### Changed — RPN history shows results, not just inputs

`RpnCalculator.History` entries now capture both the submitted line and
its result. Each line reads as `<input>  =  <top>` after a successful
evaluation, or `<input>  →  err: '<token>'` when a token didn't parse.
The NavCalc HISTORY pane reads naturally now — e.g. `300 400 atan2  =
36.869898` — instead of just echoing the command back.

### Changed — Asteroid renamed Plgn-1285 → PLGRN-1285

Save migration shim updated: `_objectRenames` chains the rename so
saves that still carry the older `Plgn-1285` (or the original
`Asteroid A-1138` / `Asteroide A-1138`) all resolve to `PLGRN-1285` on
load. No data loss on the in-progress save.

---

## v0.21.0 — 2026-05-25

### Added — battery recharge: engine alternator + Fuel Depot top-off

Battery now has both ends of a real resource loop:

- **Engine alternator** — each powered-on engine trickle-charges the
  ship battery at 0.10/sec. Six engines net ~0.6/sec, enough to be
  power-positive against the telescope at gain ≤ 1.5 with two
  engines on. In-flight pilots stay charged passively; parked with
  engines off, the receiver still drains until the next refuel.
- **Fuel Depot top-off** — Mara's `[3]` action now refills the ship
  battery alongside the suit + ship O2, same flat 50¤. Action label
  updated to `[3] Top off ship O2 + battery (50¤)`.

Closes the half-built battery system left open in v0.20.1.

### Changed — Asteroid A-1138 renamed to Plgn-1285

`test_objects.json` and the description updated. Save migration shim
(`SaveSystem._objectRenames`) silently rewrites `lockedTargetName`,
`DiscoveredObjects`, and `MissionsCompleted` references on load —
older saves keep loading without losing their lock or logbook lines.

### Changed — pilot default age 28 → 142

Long-lifespan-civ-from-longevity-treatment-population vibe. Existing
saves carry whatever age they had; new pilots default to 142.

### Fixed — Engineering skill bar collided with the skill name

`Engineering` is exactly 11 characters; the v0.20 skill row padded
names to 11 and started the bar at the next col, so the bar abutted
the final `g`. Padding bumped to 12 + bar shifted to `x+14` so the
11-char names get a breathing space.

### Changed — NavCalc deltas as integers; formulas without units

Δx/Δy/Δz now print with no decimal places (`-1988` instead of
`-1988.66`) — full precision lives in the RPN calc anyway. Formula
lines no longer carry `→ degrees` / `→ km` annotations.

### Fixed — NavCalc bottom overflow

The calculator + history pane was rendering past the panel body
(input row at row 29, status at row 30) and spilling into the device
chrome. Layout compressed: stack down to a single visible top entry,
separators under FORMULAS + RPN header removed, history empty-state
trimmed to one line, footer hint pinned to the last body row.
Everything fits inside the screen frame now.

---

## v0.20.1 — 2026-05-25

### Fixed — telescope was draining the fuel tank instead of the battery

`TelescopeController.ConsumeReceiverPower` was deducting its per-tick
draw from `gameState.Fuel` (rocket propellant!) every frame the
telescope wasn't `Off`. With the telescope on `Auto` from a docked
spawn, the tank drained while the pilot sat idle in the bay.

`GameState.Battery` was always the intended sink (per its existing
comment: "drained by power-hungry systems (telescope, life support)")
— it just wasn't wired up. The receiver now draws from `Battery`
(0..100), so at gain 1.0 the reserve lasts ~33 minutes of continuous
use; gain² escalates the drain quadratically so the gain knob has
real economic weight without leaking into the fuel system.

Renames `BaseReceiverFuelRate` → `BaseReceiverPowerRate` and
`GetReceiverFuelRate` → `GetReceiverPowerRate` to match. No save
migration needed (Battery already round-trips).

Note: Battery has no recharge path yet — a follow-up will wire an
"engine alternator" recharge while engines are active plus a Fuel
Depot battery top-off. For now the receiver drain is just a slower
trickle than it was, and the fuel tank stays put when you do.

---

## v0.20.0 — 2026-05-25

### Added — 8 new skills + 3-category structure (18 total)

`SkillCatalog` grew from 10 to 18 entries across three categories:

- **Science (6)** — Math `∑`, Physics `φ`, Chemistry `⚗`, Astronomy `⊙`,
  *Biology `❀`*, *Geology `⛰`*
- **Engineering (6)** — Rocketry `⚛`, Engineering `⚙`, *Mining `⛏`*,
  Piloting `✈`, *Cryptography `⚷`*, Research `λ`
- **Practical (6)** — Combat `⚔`, Trading `⚖`, *Diplomacy `⚜`*,
  *Survival `⛺`*, *Medicine `⚕`*, *Stealth `◐`*

Italicized entries are new. All start at level 1 (novice); progression
hooks still pending.

### Changed — Profile tab layout (3-column top + 3-column skill matrix)

The v0.19 Profile tab had three problems: BIO block ate 4 rows for a
placeholder, Achievements sat in a column wider than its content
needed, and the 10-skill two-column matrix overflowed the panel frame
on the last 3 rows. v0.20 fixes all three:

- **BIO block removed** — its 4 rows go back to skills.
- **Top section** is now three columns: Identity (Name/Age + Life
  Support) on the left, Pilot Ledger (Credits/Intel/Reputation) in
  the middle, **Achievements on the right** as a dedicated third
  column.
- **Skills band** is now three columns (Science / Engineering /
  Practical), 6 skills per column, with a compressed 7-segment bar
  to fit the 36-col column width. 18 skills fit inside the screen
  frame with margin to spare.

### Fixed — bay-cockpit right panel flicker

`StationInfoPanel.DrawServices` and `DrawConditions` were re-wiping
their rows to black + repainting every frame, called from
`CockpitLandingView.Update`. With the ship parked in a bay the panel
content never changes, but the wipe → repaint cycle was visible as
a black flash on every frame.

Both methods now diff-cache their content by `(target name + type)` /
`(target type)` respectively and skip the wipe + repaint when the key
is unchanged. `Clear()` invalidates the new keys so a full
re-Activate still produces a fresh paint.

---

## v0.19.0 — 2026-05-25

### Changed — PDA tab rework: Cargo out, Profile + Contacts in

The PDA's full-screen Cargo tab is gone — that role belongs to the
dedicated Cargo Bay panel (`[C]`) shipped in v0.16. Two new tabs take
its place:

- **[2] PROFILE** — pilot identity (name, age, health gauge), the
  ledger (credits / intel / reputation), life support (suit + ship O2),
  a bio line, a **skill matrix**, and a derived **achievements**
  block.
- **[3] CONTACTS** — list/detail of every NPC the pilot can encounter,
  with comm-channel address ("RB-7.3 / SJulián"), short bio, and the
  services each one provides. Contacts the pilot hasn't met yet show
  as dim "you haven't met this contact yet" stubs until the
  associated station is discovered.

New tab order: `[1] CONTRACT [2] PROFILE [3] CONTACTS [4] LOGBOOK
[5] CHARTS [6] NAV CALC`.

### Added — Pilot Skills (data model)

Ten skills tracked on `Pilot.Skills`, split into two groups:

- **Science** — Math `∑`, Physics `φ`, Chemistry `⚗`, Astronomy `⊙`,
  Rocketry `⚛`
- **Practical** — Piloting `✈`, Engineering `⚙`, Combat `⚔`,
  Trading `⚖`, Research `λ`

Each is an integer 0..10, all starting at **1** (novice). Round-trips
through the save file. Progression mechanics (which actions raise
which skill) are deferred — the data model is the v0.19 hook.

Also new on `Pilot`: `Age` (default 28), `Bio` (string, empty by
default — gets populated by future background/arc content),
`Health` (0..100, separate from `GameState.HullDamage`).

### Added — Contact Registry

Static catalog of station NPCs in `ContactRegistry.All`: Vance (Crew
Chief), Mara (Pumps), Greta (Welder), Quill (Broker), Dax (Dispatcher),
plus the three ATC voices (San Julián / Glaciar / Piedrabuena). Each
contact has a comm channel handle, a paragraph bio reused from the
existing in-game dialog, and a short services list. "Met" status is
derived from `GameState.DiscoveredObjects` so the list reveals itself
as the pilot travels.

### Changed — Nav Calc layout: three columns + history pane

The Δx/Δy/Δz block used to live cramped next to the raw coordinate
readout. Now there are **three header columns**: TARGETS (left),
RAW INPUTS (middle, ship + target coords only), DELTAS (right, just
the three deltas with more breathing room). Formulas band stays full
width below. The **calculator gets a taller stack pane** (3 entries
visible instead of 2) and a **HISTORY column on the right** showing
the most recent submitted lines — pilot can read back what they
already computed without losing their stack.

---

## v0.18.3 — 2026-05-25

### Fixed — PDA ↔ Cargo soft-lock

Opening one with the other already open used to leave both `_returnMode`
and `CargoSection.PreviousMode` referencing each other, so neither
close-key could find its way back to the original cockpit mode. Now
the two are **mutually exclusive**:

- Global `[P]` doesn't open the PDA while CargoSection is the active
  mode (existing exclusion list extended to include `CargoSection`).
- Global `[C]` doesn't open the Cargo Bay while PDA is the active mode
  (and only fires at all while the pilot is on-ship — see below).

### Changed — Cargo Bay only opens when on-ship

The bay is part of the ship, so `[C]` only triggers from cockpit modes:
`Flight`, `LandingShutdown`, `LaunchPad`, `BayCockpit`. Pressing `[C]`
from a station module (Trading Post, Repair Bay, etc.), the asteroid
surface, or the mine shaft no longer does anything; the pilot can
still see the same data via the existing module screens.

### Fixed — PDA screen-frame right-margin asymmetry + missing bottom edge

The inner CRT-phosphor frame was drawn at `right = InnerWidth - 3` and
`bottom = Height - 4`, which left a 3-column gap on the right (vs 1
column on the left) and put the bottom edge directly above a row that
NavCalc occasionally writes into, so it would appear partially erased.
Frame now uses `right = InnerWidth - 1` (1-column symmetric gap on
both sides) and `bottom = Height - 3` (one row farther from the
content area).

### Changed — pitch numeric readout removed

The polar-angle text next to the pitch ladder (`+25.0° ⇕`) duplicated
the value already shown in `DrawDirection` at the bottom of the
SpatialScanner. Removed; the diamond marker `◇` on the ladder is the
single attitude indicator. Pre-existing cells get wiped each tick to
clear ghosts from older paints.

### Changed — landing/launch right panel no longer shows altitude text

`StationInfoPanel.DrawLive` used to print `Altitude: NNNm` near the top
of the right column during the cockpit-landing views. The same number
already lives on the LandingHud altimeter ribbon, so the duplicate
text was visual noise. Removed.

### Changed — compass tape labels every 60° + cardinals only

The 30°-interval labels (`030`, `060`, `120`, `150`, ...) crammed
together at the cone edges, e.g. "120150" with no breathing room. The
tape now shows N / E / S / W (cardinals, yellow) plus the 60° marks
(60 / 120 / 240 / 300, gray) — 8 labels per 360° instead of 12, plenty
of horizontal margin.

### Changed — starfield density tweaked + clustered

Star count down ~17% (180 → 150). The 150 are split: 120 uniformly
distributed across the celestial sphere as before, plus 30 split among
**3 clusters** with tight Gaussian-ish jitter (`±0.12` stddev on each
axis, then renormalized to the sphere). Some patches of sky now read
as crowded knots while the rest of the cone is sparser — closer to a
spiral-arm sky than a uniform fog of dots.

---

## v0.18.2 — 2026-05-25

### Changed — stars get spectral color + per-star twinkle

Background starfield is no longer 180 identical DarkBlue dots. Each
star now carries a fixed color sampled from a real-sky-ish palette
(white/gray weighted, with rarer cyan, yellow, red, magenta), plus
its own twinkle rate (0.2–1.5 Hz) and phase offset. Three brightness
tiers per tick:

- **bright** (`b ≥ 0.7`) → full color, `✦` glyph
- **dim** (`b ≥ 0.3`) → dark variant of the color, `·` glyph
- **off** (`b < 0.3`) → not drawn

The draw is throttled to ~5 Hz so the twinkle reads as deliberate
rather than seizure-inducing, and it now runs on every frame rather
than only when the ship moves — the sky breathes while the pilot is
parked.

### Fixed — pitch ladder no longer shows gaps after object pass

The pitch ladder (col 4, rows 7-15) lived inside the radar's drawing
area; when a tracked space object's old cell happened to overlap the
ladder column and the object-clearing pass wrote a blank, the ladder's
"skip if unchanged" guard meant the wiped cell stayed empty. Ladder
background now repaints unconditionally each tick (9 cheap writes),
and the starfield's new HUD-avoidance check skips ladder cells +
compass strip + numeric polar readout so neither pass stomps the HUD.

---

## v0.18.1 — 2026-05-25

### Added — background starfield in the flight view

180 fixed unit-vector "stars" sprinkled on a unit sphere, projected
through the ship's `WorldToShipRelative` transform each frame. As the
pilot pitches/yaws, the stars slide across the radar cone — a low-cost
parallax cue that the cockpit looks out on a 3D world rather than a
flat panel.

Stars draw as small DarkBlue `·` dots so they sit visually beneath the
gray radar ring dots and the colored object glyphs. Cells stomped by a
tracked object are skipped (the object glyph wins). Star positions are
diff-painted just like the object pass: previous-frame cells are wiped
and ring dots restored before the new frame paints, so flicker stays
minimal even at the per-frame cadence of ship rotation.

Star seed is fixed (`0x5A11`) so the same starfield reappears every
session — gives the cockpit a recognizable sky.

---

## v0.18.0 — 2026-05-25

### Added — retro-futuristic PDA chrome

The PDA used to read as a flat panel — title at the top, hint at the
bottom, generic frame all around. Now it presents as a handheld
device the pilot carries:

- **Device header (row 1)** — `◉ PWR` power LED on the left, brand
  badge `STELLARSAIL P-DA · D-7 · {PILOT}` in the middle, **live
  battery gauge** on the right tied to `ship.Battery` (five-segment
  bar that flips green → yellow → red as the reserve drops). The
  gauge refreshes per frame via `PdaView.Update`.
- **CRT screen frame** — phosphor-green inner box drawn at row 2
  (top), row Height-4 (bottom), wrapping the entire tab strip + tab
  body so the content reads like it sits on an LCD/CRT.
- **Device footer (row Height-2)** — speaker-grille pixel art on the
  left (`▘▝ ▙▟`), controls hint in the middle, antenna icon (`≣⟀`)
  on the right.

Tab and body coordinates didn't move — the chrome is overlaid around
existing content. Side effect: the cargo/contract/logbook bodies pick
up an implicit "screen" border without anything in those drawers
needing changes.

---

## v0.17.0 — 2026-05-25

### Fixed — PDA Cargo: Credits/Intel/Reputation moved to a right column

Those three lines used to print after the cargo list, which pushed them
off the bottom of the PDA panel once the catalog grew past ~12 items.
They now live in a `PILOT LEDGER` block on the right side of the Cargo
tab, in the same vertical band as `LIFE SUPPORT` — always visible,
never clipped by the catalog length.

### Added — icons in the Cargo Bay

Every cargo item now carries an `Icon` + `IconColor`, and each section
has a header glyph in `CargoCategoryInfo.Icon`:

- Sections: General `⚙`, Mineral `⛏`, Fuel/Cryo `⚡`, Hazmat `☢`, Passengers `웃`
- Items: Fuel Cells `⚡`, Machine Parts `⚙`, Biocultures `❀`, Medical
  Supplies `⚕`, Scrap Metal `⛓`, ores `◉/◈/▪/♦/✱` (mirroring the shaft's
  mineral glyphs), Water/Methane Ice `≈`, Sulfur `✦`.

### Changed — Autopilot v2 (visible phases + brake-to-stop arrival)

Three problems from v0.15:
1. Phase transitions were silent — pilot saw "Autopilot ON" then guessed
   what was happening from the speed gauge.
2. Rotation at 30°/s was fast enough that the heading read as instant.
3. The Approach phase held 5 km/s into the disengage radius, which felt
   like the ship was about to slam the station.

v0.17 fixes all three:

- Each phase transition writes a Navigation-class line to the message
  log: `Correcting course toward Glaciar Perito Moreno…` →
  `Course corrected. Cruising at 50 km/s.` → `Approach phase — braking
  into Glaciar Perito Moreno.` → `Arrived. Autopilot disengaged.`
- Rotation slowed to 15°/s so the cockpit reads "correcting" rather than
  "snapped." Alignment threshold tightened to 0.98 so the Aligning phase
  actually runs in most cases.
- Approach phase uses a **cubic** taper from the captured cruise speed
  down to 1 km/s (was 5 km/s linear) — gentle deceleration far out,
  harder braking near the pad. At disengage radius the autopilot zeroes
  the velocity vector and hands off to the pilot at a true stop.

---

## v0.16.0 — 2026-05-25

### Added — Cargo Bay panel with separate compartments

New full-screen view, opened with the global `[C]` key from any non-text /
non-active-flight mode (the same exclusion list as the PDA's `[P]`).
Closes with `[Esc]` or `[C]` again, restoring the mode you came from.

The bay shows the pilot's stock split across categories instead of one
flat list — each item is now tagged with a `CargoCategory`:

- **General** (100 cap) — MachineParts, Biocultures, MedicalSupplies, ScrapMetal
- **Mineral** (150 cap) — iron / nickel / copper / silicates / quartz / titanium / palladium / rare-earth / platinum
- **Fuel / Cryo** (80 cap) — FuelCells, Water Ice, Methane Ice
- **Hazmat** (30 cap) — Sulfur Deposit (sealed compartment for fume-prone goods)
- **Passengers** (4 souls) — head-count, driven by new `Pilot.Passengers` int

Per-section capacities are advertised but storage is still the unified
`Pilot.CargoHold` dictionary — v0.16 ships the "all-upgraded" stock
configuration. Future work will introduce real per-section capacity,
per-ship-class configs (smaller ships locked out of Hazmat etc.),
upgrade gating, and section-to-section transfer.

`Pilot.Passengers` round-trips through the save schema; older saves
load with 0 souls aboard.

---

## v0.15.0 — 2026-05-25

### Changed — autopilot now owns velocity, not just attitude

Pre-v0.15, engaging autopilot (lock target + Enter from Telescope Lock
mode) only rotated the ship's nose to face the target — the pilot had
to brake and re-thrust manually. If you locked while cruising at
50 km/s in another direction, the autopilot would point you correctly
but you'd sail right past.

`AutopilotController` is now phased and takes the velocity vector
through three explicit stages on engage:

- **Aligning** — brake toward zero while rotating. Skips to Cruising
  early if residual velocity is already roughly aligned (≥ 0.95 dot)
  with the target heading.
- **Cruising** — re-accelerate along the heading to the **captured
  cruise speed**. The captured speed is the ship's velocity magnitude
  at the moment of engagement, floored at 25 km/s so a pilot sitting
  still still gets a usable approach. The engage-message now reports
  it: `Autopilot ON (1217km, cruise 50km/s)`.
- **Approaching** — within 80 km of the target, linearly taper down
  to **5 km/s arrival speed** so the ship doesn't slam into the pad.
  Autopilot disengages at 0.5 km, leaving the pilot in final-approach
  control.

Acceleration is capped at 40 km/s² so phase transitions read as
deliberate adjustments rather than velocity teleports.

---

## v0.14.2 — 2026-05-25

### Changed — auto-landing window tightened to 10m

`AutolandMaxAltitude` lowered from 20m to 10m. ENTER now only arms the
autoland sequence once the ship is in the final approach band, making
the autopilot's "drive every axis to zero" feel like a true landing
assist rather than a takeover from cruise altitude.

---

## v0.14.1 — 2026-05-25

### Changed — landings now start at 500m instead of 200m

Both descent entry paths — cleared landing through `LandingCodeInputSystem`
(Pact stations with ATC) and `CommunicationInputSystem.InitiateManualDescent`
(non-station bodies like asteroids) — used to snap the ship to 200m above
the pad. Bumped to **500m** so the altimeter (`AltimeterScale = 500.0m`)
reads a full gauge at entry and counts down smoothly to zero, giving the
descent the runway it deserves.

---

## v0.14.0 — 2026-05-25

### Added — pilot reputation (v0)

A single global reputation score is now tracked on `Pilot.Reputation`,
starts at 0 for new pilots, persists across saves.

- **+5** on every successfully delivered contract (manifest accepted at
  the Hangar after the Ops report-in flow).
- **-10** when the pilot cancels an accepted contract via the new `[C]`
  prompt in the Ops Center — Dax now mentions the cancel "takes a hit"
  in his dialog.
- Visible in the PDA **Cargo** tab beneath `CREDITS` and `INTEL`,
  signed (`+5`, `-10`, `0`), green when positive, gray at zero, red
  when negative.

This is v0 — a single global score. Future hooks (per-faction buckets,
dialog tone shifts at ±20/±50/±100, gating for higher-tier missions,
grey-market integration with the unverified-prospect-origin loophole)
are deferred until the design pressure surfaces.

---

## v0.13.2 — 2026-05-25

### Added — cancel active contract from Ops Center

Pilots can now bail on a contract they accepted by accident or
changed their mind on. New `[C]` action appears under the left-side
ACTIVE CONTRACT box whenever there's a mission on file. Pressing it
opens a two-key confirmation (`[Y]` drops it, `[N]`/Esc keeps it) so a
fat-finger can't blow away an active haul. Dax handles the dialog —
including a small explanation that the payout is forfeit. No
reputation cost yet; that lands with the upcoming reputation system.

---

## v0.13.1 — 2026-05-25

### Fixed — PDA Cargo tab "ITEMit 100 / 100" header collision

The Cargo tab's column headers (`ITEM` / `QTY`) were drawn at row
`y+3`, which is the exact row `DrawLifeSupportRows` had already used
for the `Suit 100 / 100` readout. "ITEM" overwrote the first four
chars, producing the garbled `ITEMit 100 / 100`, and the separator
at `y+4` then erased the `Ship` row entirely. The cargo table now
starts at `y+6` (`cargoHeaderRow`) so it sits cleanly below the
life-support block; the empty-hold messages shift down by the same
amount.

---

## v0.13.0 — 2026-05-25

### Added — in-flight compass strip + pitch ladder

Until now, the pilot's only attitude readout in flight was the small
text line at the bottom-left of the radar (`P:053/+26 | M:053/+26`).
Two new always-on widgets overlay the SpatialScanner panel:

- **Compass strip** across the top of the radar gutter (cols 17-40):
  a sliding tape of 30° markers. Pointer `▲` is fixed at center, the
  N/E/S/W cardinals slide as the ship yaws. Cardinals in yellow,
  numeric headings in gray, dark-gray ticks underneath.
- **Pitch ladder** down the left edge (col 4): a 9-row vertical
  ladder centered on the horizon row. Diamond marker `◇` slides up
  for nose-up pitch, down for nose-down. Cyan until ±60°, red beyond.
  Live numeric polar readout sits at the horizon row.

Both widgets diff-paint — they only redraw when the underlying yaw
or polar actually changes enough to move a tape label or shift the
marker row, so the per-frame cost is trivial.

---

## v0.12.5 — 2026-05-25

### Changed — Asteroid A-1138 is now findable via the radio waterfall

Asteroid A-1138 used to ship with `frequency: 0.0` (radio-silent),
which meant a prospector at Puerto San Julián had no way to discover
it through the telescope's spectrum/waterfall — you had to already
know its coordinates. Set its emission to **0.4 MHz** (faint thermal
signature) so you can now sweep azimuth/polar with the telescope ON
and watch for a vertical stripe in the waterfall at the 0.4 MHz bin,
then lock with `L`.

---

## v0.12.4 — 2026-05-25

### Added — mining-loop sound effects

The shaft and Workshop are no longer silent:

- **Dig thud** — every successful pick swing plays a short impact whose
  pitch tracks the tile's hardness (90 Hz for soft regolith up to 240 Hz
  for hard rock).
- **Mineral chord** — extracting a mineral plays an ascending chord whose
  voicing scales with rarity: rarity-1 is a single warm tone, rarity-5
  layers four notes with octave + major-third on top. Within a rarity
  tier, the mineral's id-hash shifts the root by up to ±6 semitones so
  iron and nickel sound related but not identical.
- **Pick bounce** — sharp two-note clang when the pick can't break the
  target tile (TooHard result).
- **Tool broken** — descending three-note disappointment when the pick,
  scanner, or detector hits zero durability.
- **Scanner sweep** — fast rising 600/900/1300 Hz sweep on `[R]`.
- **Detector ping** — two evenly-spaced 500 Hz pulses on `[F]` — distinct
  from the scanner so the pilot hears which knowledge tool fired.
- **Workshop repair / upgrade** — Greta plays a crisp two-note chime on a
  successful repair and a triumphant rising A-C#-E-A chord on a tier
  upgrade.

---

## v0.12.3 — 2026-05-25

### Added — per-system on/off chirps + active-systems pad

The four ship systems (Electrical / Security / Life Support / Hull
Seals) now have audible state, matching the engines work from v0.12.1:

- **On / off chirps** — each system has a distinct base pitch so the
  pilot can hear *which* system they flipped: Electrical 720 Hz (crisp),
  Security 440 Hz (alarm-panel cue), Life Support 330 Hz (warm),
  Hull Seals 200 Hz (deep). Rising on-chirp, falling off-chirp.
- **Active-systems pad** — new `SystemsHumProvider` holds a quiet
  three-layer tonal pad (55 / 110 / 220 Hz) whose volume scales with
  how many systems are powered on. Four on → audible "ship is alive"
  hum; zero on → silent. Capped well below the engine hum so the
  motor work stays the dominant sound.

---

## v0.12.2 — 2026-05-25

### Fixed — `[4] Report cargo` no longer looks like a fourth mission

The "Report cargo for active contract" prompt used to render at the
bottom of the right-side mission list, immediately after `[3]`, which
made it read like a fourth dispatch listing the pilot could accept.
Moved it out of the missions block entirely — it now sits directly
below the left-side **ACTIVE CONTRACT** box, in yellow, and only
appears when the pilot has an active contract at its destination
station awaiting manifest approval. The `[4]` key handler is
unchanged.

---

## v0.12.1 — 2026-05-25

### Added — engine ignition / shutdown chirps + idle hum

Each of the six engines now has audible state:

- **Ignition chirp** — a short rising note (180-355 Hz depending on engine
  number) plays when an engine is powered on. The pilot can audibly tell
  *which* engine they just toggled.
- **Shutdown chirp** — mirror of ignition. Same pitch, falling note.
- **Idle hum** — `EngineProvider` now accepts the live active-engine count;
  with engines powered on but no thrust + no velocity (sitting on the launch
  pad post-checklist), a quiet rumble holds proportional to how many are
  on. Six engines = a subtle background presence; one engine = a whisper.

---

## v0.12.0 — 2026-05-25

### Changed — asteroid surface is now a zone picker, not a 5×5 grid

The original asteroid surface had five identical rows of five identical
rectangles — `[R]` to scan, `[X]` to extract per cell, `[G]` to drop a
shaft. Now that the shaft mini-game owns mining proper, the surface has
been rebuilt as a free-walk **dig-site picker**:

- **3-10 irregular zones** are generated from the asteroid's name
  (deterministic per body), each rendered as a colored disc with a
  numbered badge (`[1]`..`[10]`). Three size classes (Small / Medium /
  Large) in dark-yellow / yellow / cyan respectively.
- Pilot walks the canvas with **WASD/arrows**, jumps directly to a
  zone with **1-9** (or **0** for the tenth). The status line under
  the canvas shows which zone the pilot is currently inside.
- **`[G]` descends** into the shaft for whichever zone the pilot is
  standing in — and the shaft is now **sized to that zone**:
  - Small zones → 9-wide × 30-deep shaft (quick run, sparse veins)
  - Medium zones → 15 × 50 (the previous baseline)
  - Large zones → 21 × 80 (deep, dense, more loot per visit)

`R` (scan) and `X` (extract) are gone — those cell-level actions were
superseded by Phase 1's mine-shaft loop.

### Fixed — pilot avatar trail on the surface

`웃` is two console cells wide on most terminals; the old per-cell grid
hid the issue because cells were 9 cols apart, but the new free canvas
exposed it. `RefreshAfterMove` now wipes both cells of the previous
footprint and restores the underlying zone tile (or badge character)
before painting the avatar at its new position.

---

## v0.11.2 — 2026-05-17

### Added — two-press confirmation before quitting with END

Single accidental `END` used to quit immediately and silently lose any
unsaved progress. Now the first press arms a 5-second window — shows a
yellow toast (`✕ PRESS END AGAIN TO QUIT`) and a message-log line
reminding the pilot they can save first with `F2`. A second `END`
inside the window actually exits. The prompt expires silently if the
pilot does anything else.

---

## v0.11.1 — 2026-05-17

### Added — life-support oxygen (suit + ship)

Two new resources on the pilot:

- **Suit oxygen** (`OxygenPortable`, 100 max) — drains 1 unit / second
  while EVA on the AsteroidSurface. The asteroid panel shows
  `SUIT O2  N/100` in the top-right corner, green/yellow/red. Below
  25% the message log surfaces a throttled warning; at zero the hull
  begins to stress at 2%/sec, giving the pilot roughly 50 seconds of
  leeway to make it back inside.
- **Ship reserve** (`OxygenShip`, 1000 max) — the cabin tank. Refills
  the suit automatically every time the pilot re-boards from the
  surface. Being inside the cockpit costs nothing; the only way the
  ship reserve drops is via that transfer.

The PDA Cargo tab gets a **LIFE SUPPORT** section showing both
levels with the same color thresholds.

The Fuel Depot has a new action:

  `[3] Top off ship O2 (50¤)`

Mara tops both the ship reserve and the suit for a flat 50 credits.
Information sub-menu slid down to `[4]`.

`Pilot.OxygenPortable` and `Pilot.OxygenShip` are persisted; older
saves without the fields default to full so existing pilots don't
load into a suffocating state.

---

## v0.11.0 — 2026-05-17

Two new pilot conveniences.

### Added — cargo hold capacity (default 50, upgradable)

Pilots now have a hard cargo limit. Default starting capacity is 50
units. The Trading Post and asteroid extraction both refuse the
overflow with a clear message ("Hold full — N/M units, sell or
upgrade first" or "Hold capped — N units left behind"). The PDA Cargo
tab shows a `X / N units` gauge in the header, colored red when full,
yellow when ≤5 free, gray otherwise.

Repair Bay has a new action:

  `[3] Upgrade cargo hold (+10 units / 500¤)`

Greta rewelds the inner bulkhead, capacity grows by 10, credits drop
by 500. Information sub-menu slid down to `[4]`.

`Pilot.CargoCapacity` is persisted; older saves without the field
default to 50.

### Added — board the ship while it's parked in the hangar bay

The Hangar dialog grows a new action when the ship is in the bay:

  `[B] Board the ship (charts, cargo, systems)`

Pressing it switches into the new `BayCockpit` mode — same panel
layout as the other cockpit-landing views, so the pilot can:

- Open the **PDA** with `[P]` and read Charts / Cargo / Logbook.
- Toggle engines `1-6` and systems `7-0` for inspection.
- Watch the Shutdown / LandingHud / StationInfo readouts.

There's no launch path from the bay — the bay isn't a pad. To actually
take off the pilot still has to ask Vance to bring the ship to the
atrium first. `[E]` or `[Esc]` disembarks back to the Hangar dialog,
with Vance's existing dialog log preserved.

---

## v0.10.2 — 2026-05-17

Asteroid-landing polish — the disembark/launch loop on a non-station
body had several rough edges that conspired to strand a pilot.

### Fixed — disembarking from the cockpit on an asteroid no longer drops you into a phantom station map

`LaunchPadInputSystem.Disembark` was unconditionally setting Mode to
StationMenu — which on an asteroid took the pilot to a fake station
menu with Refuel / Repair / Hangar etc. for a body that had none of
those. Now branches on `LockedTarget.Type`: non-Station resets and
re-enters the AsteroidSurface grid; Station keeps the existing
station-map flow.

### Fixed — save recovery for the bogus-station-menu state

Saves taken in the broken state (Mode = StationMenu but LastStation
null) used to load into Flight at zero altitude on the asteroid pad.
SaveSystem.Apply now has a third Mode-restore branch: if LastStation
is null but LockedTarget is a non-Station body and the ship is within
100m of it, restore to LaunchPad. You can then step back onto the
surface or run startup and launch normally.

### Fixed — `[E]` no longer accidentally boards the ship from the asteroid grid

`E` is already a directional letter in everyone's WASD/QE muscle
memory and pilots kept accidentally entering the cockpit while
trying to walk east on the surface grid. Only `[Esc]` returns to the
ship now. The bottom hint says so.

### Fixed — right-column info panel showed station services on asteroids

`StationInfoPanel` hardcoded the six station services (Refuel / Repair
Bay / Hangar / Mission Board / Trading Post / Data Library) regardless
of target type. Now branches: stations show services + wind / pad
conditions; non-stations show "SURFACE / no services / uninhabited
body", scan/extract action hints, and "Atmosphere: none / Surface:
rock/dust" conditions.

### Fixed — Launch / Landing menu hint was advertising the wrong keys

MenuPanel was falling through to the Flight-mode hint
(`[T-TELESCOPE] [ARROWS-FLY] [SPACE-THRUST] ...`) during
LandingShutdown / LaunchPad / Launch / LandingCodeEntry — so pilots
pressed arrow keys based on the bottom hint and got nothing. Added
mode-specific hints for each phase.

### Added — arrow-key aliases during Landing and Launch

In addition to `IJKL`, the arrow keys now strafe (`←/→`) and move
forward / backward (`↑/↓`) during Landing and Launch. Vertical (`U/O`)
and attitude (`W/S` `A/D` `Q/E`) stay on letters.

### Changed — Launch's vertical thrust per tap is now 5 m/s (was 1)

Against gravity (0.08 m/s²), a 1 m/s tap only nets ~6m of altitude
before the impulse dies — the ship felt like it wasn't accelerating.
Bumped to 5 m/s so each tap visibly moves the altimeter. Still
gravity-limited (the rocket idiom), the pilot is just no longer
fighting at the noise floor.

---

## v0.10.1 — 2026-05-17

Polish pass on top of v0.10.0 — city layout, conversation cleanup, and
a new way into Glaciar's city.

### Changed — Glaciar's City Gate replaces the Trading Post slot

The Glaciar station map no longer has a Trading Post. Its bottom-right
tile is now a **City Gate** — a small mini-map of the buildings beyond
it (Hospital / Market / Plaza / Artisans / Ice Cream / Entertainment),
laid out the same way the city itself renders. The pilot walks their
avatar to the City Gate tile and presses Enter to step into the city,
matching the spatial idiom every other module on the station uses.
Other stations keep their Trading Post unchanged.

The `[C]` global hotkey from v0.10.0 is gone — diegetically, the city
is reached by walking through the gate, not pressed into from anywhere.

### Added — Data Library hints at Glaciar's location

Two new Lore entries:

- **"Glaciar Perito Moreno — Bearing"** — gives direction + rough
  distance ("about half a thousand kilometers out, east of home,
  upward of the orbital plane") without publishing exact coordinates.
- **"Glaciar City"** — explains that the city is built into the
  bedrock around Glaciar's landing pad and how to enter it via the
  City Gate tile.

The exact coordinates still aren't in the chart until you accept a
contract bound for Glaciar (dispatcher hands over the bearing) or fly
close enough to chart it by proximity.

### Fixed — Glaciar city layout overlap

The dialog log was overlapping the Entertainment Hall, wiping the
building's content and burying the avatar at Entertainment's coords
under dialog text. Reflowed into three clean building rows + a
dedicated dialog band (rows 20-23) so every location shows the pilot
correctly.

### Fixed — Glaciar city NPC dialog used to truncate with "…"

Lines longer than 60 chars were getting clipped with an ellipsis.
Switched to the same word-wrap pattern the other detail panels use
(split on spaces, indented continuation under the speaker prefix).

### Fixed — comm "Waiting for response..." after departure clearance

A terminal dialog node clears AvailableResponses but doesn't change
CurrentPermission (which stays InConversation). The CommPanel was
printing "Waiting for response..." even though the conversation had
actually ended — most visible right after the tower granted
departure clearance, where the next move is to launch, not to keep
talking. The panel now shows "▶ CLEARED FOR DEPARTURE ◀ / [ESC]
Back to launch pad" when DepartureCleared is true, and just the
[ESC] hint otherwise.

### Fixed — duplicate `[I]/[E]` hint in Hangar / Ops / Trading

The Hangar, Ops Center, and Trading Post action lists printed
"`[I] Information · [E] Say goodbye`" right above the bottom-of-panel
hint that already showed the same line. Removed the duplicate; the
bottom hint is the right place for it. (Fuel Depot and Repair Bay
were never duplicated.)

---

## v0.10.0 — 2026-05-16

A big release — months of cockpit feel, world building, and architecture
work landing in one bundle.

### Added — NPC conversations (info + goodbye)

Every station NPC now has a real conversation shape instead of an
action-list-and-out flow:

- **`[I] Information`** opens a sub-menu with three questions:
  `[a]` the NPC's backstory, `[b]` flavor about the building, and
  `[c]` an open question tied to your active mission (or a generic
  topical line if you're between contracts).
- **`[E] Say goodbye`** prints a player + NPC farewell exchange in
  the dialog log, then a second `[E]` confirms exit. `[Esc]` lets you
  stay if you had one more thing to ask.

Applied to Fuel Depot (Mara), Repair Bay (Greta), Ops Center (Dax),
Trading Post (Quill), and Hangar (Vance). Each NPC has per-station
flavor lines (San Julián / Glaciar / Madryn) so the room feels
grounded.

### Added — Fuel Depot and Repair Bay custom amounts

The old "top up to 25/50/75/100%" buttons are gone. Both NPCs now
offer:

- **`[1] Choose an amount`** — type the unit (or point) count, Enter
  buys it. Capped at tank capacity / remaining damage and your credits.
- **`[2] Fill her up` / Full repair** — the existing 100% path.

### Added — Glaciar Perito Moreno gets a city

From the Glaciar station menu, `[C]` opens a walkable city map: a
central Plaza hub with five buildings around it — **Hospital**,
**Market**, **Artisans' Guild**, **Entertainment Hall**, and the
**Ice Cream Shop**. Arrow keys walk between buildings; Enter triggers
an NPC line at each. Mechanics (buying meds, shopping, commissioning
artisans, ordering ice cream, etc.) land in follow-up releases — for
this drop the world building is in place so the city feels alive
before the systems that drive it are wired up.

### Added — PDA opens from anywhere

`[P]` was Flight-only. Now it works from any non-text-entry mode
(StationMenu, Hangar, Fuel, Ops, Trading, LaunchPad, etc.). PDA Exit
restores the mode you were in, so PDA acts like a true toggle. Active
flight phases (Landing, Launch) are excluded — opening the PDA mid-
descent would silently keep the ship moving with no HUD.

### Added — Charts are now a rotatable 3D cube

The Charts tab was a flat X/Y plot that lost the Z axis entirely.
Rebuilt as a hybrid:

- **Left** — isometric pane. World (X, Y, Z) rotated by yaw + pitch
  and projected to screen. A wireframe cube traces the ±5000 km world
  bounds so you have an orientation reference.
- **Right** — top-down minimap (X / Z), no rotation, smaller. Good
  for precise picking when the iso angle is awkward.
- **Arrow keys / WASD** rotate the iso view by 10° increments.
  Pitch clamps to ±75° so the cube never flips upside down.

### Fixed — Hangar `[3] Unload` was missing a ship-location gate

You could collect mission payment with the ship still parked on the
atrium. Now `[3]` only renders when `ShipLocation == Bay`, and Vance
pushes back if you trigger it from somewhere weird ("Bring your ship
into the bay first — can't crack the cargo doors on the atrium pad.").

### Changed — View refactor step 9 complete (architecture)

`EngineUpdateSystem` and `Program.cs` no longer pick which panels to
paint each frame. Per-frame draw is now owned entirely by the active
view's `Update` method:

- Left column (Engines / ShipStatus / Shutdown) → in each view's Update
- Telescope + Menu updates → gated by `View.WantsBottomCockpitUpdate`
- Right column (RadioTelescope / StationInfo) → in each view's Update
- Scanner radar sweep → `FlightView.Update`
- Landing HUD → `CockpitLandingView.Update`

The ad-hoc mode-switch lists scattered around the main loop are gone.
Adding a new mode in the future means writing one View class; the
main loop never has to learn about it. `EngineUpdateSystem` now does
pure simulation work (thrust gating, fuel/heat, audio).

---

## v0.9.9 — 2026-05-16

### Fixed — toast leaves a blank rectangle after expiring

`EraseToast` wrote spaces over the 3 rows the toast occupied but never
repainted the underlying panel content. Since toasts usually fire from
a non-flight mode (Ops Center, Hangar) where no mode transition is
coming, the wiped border/content cells stayed blank. The toast erase
path now re-`Activate`s the current view, restoring whatever was
beneath.

### Fixed — flash when moving the pilot on the station map

Arrow-key navigation on the StationMenu called `Clear()` + `Draw()` on
every keypress, which wiped the entire inner area to spaces and
caused a visible flash. New `StationMenuPanel.MoveAvatar(oldLocation)`
does a flash-free incremental repaint: wipe the two cells under the
old avatar, redraw the static map idempotently (same glyphs over
themselves — no visible change), repaint the avatar + entrance hint.

### Fixed — Ops Center bottom-right border missing

`DrawActions` was placing the `[4] Report cargo` slot at `reportY = 28`,
which is the panel's bottom-border row, and unconditionally space-wiping
that row plus one row below. The bottom-right border disappeared
whether or not the `[4]` action was visible. Moved the slot up to the
trailing blank after mission 3, dropped the description sub-line
(manual covers the two-step flow), and the wipe now stays inside the
inner area.

---

## v0.9.8 — 2026-05-16

### Fixed — empty engine / system rows on the shutdown panel

Reported from a screenshot: the **POST-LANDING SHUTDOWN** panel showed
its `ENGINES (1-6):` and `SYSTEMS (7-0):` section headers but every
status row underneath was blank. Same class of bug as the v0.9.7
direction-readout bleed: the panel's differential-paint trackers
(`lastEngineStates`, `lastElectrical`, etc.) survived the
`Console.Clear` that runs during the mode-transition repaint. With
the trackers still matching current state, `DrawContent` decided
"nothing changed, skip" and left the cleared rows empty.

Added `ShutdownPanel.ForceRedraw()` that invalidates the trackers
then calls `Draw()`. `CockpitLandingView.Activate` now calls
`ctx.Shutdown.ForceRedraw()` instead of plain `ctx.Shutdown.Draw()`
— same pattern already used for `LandingHud`, `RadioTelescope`,
`CommPanel` on transitions into their owning views.

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

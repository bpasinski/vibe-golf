# Vibe Golf — Plan

## Problem

A small, shareable canvas demo for "Vibe Coding". The player golfs a ball toward a
target across a 2D field. The twist: you choose how far to shoot, but the harder you
shoot the worse your aim — near-perfect at low power, fully random at max. Reaching
the target in the fewest shots and least total distance is the game. Progress must
survive a page reload.

**Theme — shipping a feature.** The whole loop is framed as delivering software: a
shot is *shipping an implementation*, power is *scope* (how much you attempt at once),
the accuracy cone is the *uncertainty* of a change landing where intended, and the
target is the *feature delivered*. This is pure flavor over the mechanics below — it
drives labels, copy, and colour, not behaviour — but it makes the dev-workflow skills
(/plan, /plan-review, /code-review, /refactor) read as de-risking a change rather than
arbitrary power-ups.

## Solution

A single self-contained `index.html` at the **repo root** (canvas + vanilla JS, no
build step, opens directly in a browser). Everything — markup, styles, game logic —
lives in that one file so the demo is trivial to run and read top to bottom.

**Deployment (GitHub Pages).** Because it is one static file with only inline assets
and relative paths, GitHub Pages serves it as-is: enable Pages → "Deploy from a
branch" → `main` / root. No workflow, no bundler, no base-path config needed. The
same file that opens locally is what ships.

Structure inside the file (conceptual sections, not separate files):

- **State** — course (seeded start, target, target radius, obstacles), ball position,
  shots taken, total distance travelled, past-shot trail. Single source of truth.
- **Obstacles** — ~10 seeded irregular polygons ("legacy code") the ball **bounces**
  off. Placed clear of the start, the target, and off the direct start→target line so
  every course stays solvable. Part of the course, so they persist and survive reload;
  removing one (see /refactor) persists too.
- **Persistence** — the whole state is serialized to `localStorage` after every shot
  and on completion; restored on load so the player continues where they left off.
  Cleared / re-seeded on "New course". A stored schema version guards against stale
  saves.
- **Input** — drag from the ball: gesture direction sets aim, gesture length sets
  power (labelled *scope*). A live overlay previews the aim line and the accuracy cone.
  Release *ships* the shot.
- **Shot resolution** — the cursor is the intended landing spot: travel distance = the
  drag length, capped at the max reach (180% of the current distance to target). The
  accuracy cone half-angle grows with the **absolute shot distance** (0 for a short
  putt → 180° at a fixed reference distance), so precision depends only on how far you
  shoot, *not* on how close the target is — a tap-in is always accurate. The actual
  flight direction is drawn uniformly within the cone around the aim. The shot then flies a
  fixed distance budget along that direction, **bouncing** (reflecting) off obstacle
  edges and the walls until the budget is spent; the traced polyline is the trail and
  its length is the distance travelled. A shot that passes within the target radius
  holes out mid-flight.
- **Skills** — four power-ups named for the real dev skills, framed as *de-risking a
  change before shipping*, that trade **elapsed time** for **accuracy**. Each is a
  button in the HUD; using one adds penalty seconds to the run clock. They are the
  strategic layer: a skill only pays off when the accuracy it buys saves more
  distance/shots than the time it costs.
  - **/plan** — pre-shot: halves the next shot's cone half-angle. Staple precision. `+25s`
  - **/plan-review** — only after /plan on the same shot: halves the cone again
    (compounding). Cheaper, but requires planning first. `+15s`
  - **/code-review** — pre-shot: doesn't shrink the max cone, but biases the random
    draw toward the aim line (takes the tighter of two draws), so shots land
    statistically closer without capping reach. `+20s`
  - **/refactor** — select the skill to arm it (all blocks flash), then tap a block to
    **delete** that "legacy code" permanently for this course, clearing a bounce hazard.
    Tapping empty space or re-selecting the skill cancels with no cost. `+40s` on delete.
- **Scoring** — one number, higher is better, floored at 0, shown live and at win:
  `points = max(0, round(1000 × directDistance/totalDistance − 25 × shots − 1 × elapsedSeconds))`.
  The ratio rewards an efficient (straight) path, `−25 × shots` rewards fewer strokes,
  `−1 × elapsedSeconds` rewards speed and is where skill penalties land.
- **Clock** — elapsed time accumulates from real play time plus skill penalties, is
  persisted, and resumes on reload.
- **Animation & feel** — eased ball flight, motion trail, landing pulse, glow on
  start/target/ball, subtle grid backdrop. HUD: shots, total distance, elapsed time,
  live power %, live score, and the four skill buttons.
- **Win** — ball lands within the target radius → "Feature delivered" celebration
  pulse, final strokes + total distance + time + score, "New course" button. One
  active course at a time. Copy throughout leans on the theme (ship / scope / commits
  / delivered) without changing any mechanic.

## Decisions

- **Single `index.html`, vanilla JS canvas** — zero deps, instant run, best fit for a
  small demo; rejected a Vite/framework setup as overkill.
- **Angular cone accuracy** (not positional scatter) — makes aiming the core skill;
  cone widens with the absolute shot distance up to a full 180° so long "hero" shots
  are a deliberate gamble while short putts stay precise regardless of target distance.
- **Power as % of remaining distance (0–180%)**, overshoot allowed — ties risk/reward
  directly to the target and keeps controls relative as you close in; rejected
  absolute-distance mapping as less tightly coupled to the target.
- **Drag-to-aim** (direction + power in one gesture) over separate direction + power
  slider — one fluid motion, more satisfying.
- **Full-state localStorage snapshot after each shot**, versioned — simplest reliable
  continue; no per-field migration needed for a demo.
- **Seeded course** — a reload restores the same layout; only "New course" re-randomizes.
- **Root `index.html`, Pages "deploy from branch"** — a single static file needs no
  build or base path; rejected a `/docs` folder or Actions workflow as unnecessary.
- **Skills trade time for accuracy, differentiated by mechanic** — /plan and
  /plan-review shrink the cone (the latter compounding and gated on the former),
  /code-review biases the draw toward center without capping reach, /refactor deletes
  a selected obstacle. Distinct mechanics rather than four copies of one number.
- **Obstacles bounce, irregular polygons, seeded into the course** — bouncing (vs
  blocking/absorbing) makes hazards playful and routing matter; irregular shapes read
  as "legacy code"; seeding keeps them reload-stable and lets placement guarantee a
  clear direct line so no course is unsolvable.
- **/refactor is an armed mode** — select the skill first (blocks flash), then a tap
  picks the block to delete; a tap is distinguished from a shot by pointer movement
  (< 8px). Arming-first makes the destructive action deliberate. Replaces the earlier
  undo-mulligan use of /refactor.
- **Single higher-is-better score folding distance, shots, and time** — one legible
  number the skills must be weighed against; rejected separate leaderboards per metric.
- **"Shipping a feature" theme as flavor only** — labels/copy/colour reframe the loop
  (shot = ship, power = scope, skills = de-risking) so the dev-skill power-ups feel
  earned; deliberately kept out of the mechanics so behaviour stays as specified.

## Player experience after the change

1. Open `index.html`. A glowing start ball and target appear on a dark field (same
   course as last visit if one was in progress).
2. Drag back from the ball: an aim line and a cone show direction and how wild the
   shot will be. Short drag = tight cone, short hop. Full drag = 180% reach but the
   cone spans every direction.
3. Release. The ball flies along a random line inside the cone, leaving a trail; HUD
   updates shots and total distance.
4. Repeat toward the target. Land inside it to win — see final strokes + distance,
   then start a "New course". Close the tab any time; reopening resumes mid-course.

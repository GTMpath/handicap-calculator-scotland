# Session — 2026-07-01 · Rebrand + competition rebuild + persistent memory

Long working session with Justin. Started as a design polish pass, grew into a full rebrand
and a rebuild of the competition format. This log is the source of truth for where we are.

## What shipped (all in `index.html`, one file, no build step)

### Branding → "The 2nd Eagle Father & Son Invitational"
- Renamed from "Fife & Angus Golf Tour". Eyebrow "The 2nd Eagle · St Andrews 2026", H1
  "Father & Son Invitational", trimmed intro to one line.
- New navy/blue palette pulled from Justin's logo: `--ink` #16243f navy, `--gorse` #3f5c8c
  denim/saltire (the accent, replaces the old gorse yellow), cool near-white backgrounds.
  Team colours: Fife = navy #16243f, Angus = claret #8a3b46 (in CSS `--teamA/--teamB` AND
  JS `TEAM_COLORS`, keep them in sync).
- Logo: masthead loads `assets/logo.png`; until that file exists it shows an inline SVG crest
  fallback (F&S badge + saltire). **Justin still needs to save the real logo to
  `assets/logo.png`** (the pasted image was never on disk).

### Layout
- Tabs reordered to **Schedule / Competition / Handicaps**. Active tab now denim with white
  text + a bottom accent bar so it reads clearly as a tab (was too subtle).
- Schedule tab now leads with a **summary block** (`#schedSummary`, `renderSchedSummary()`):
  the two locked teams + the 5-round format with points.
- Dinner names and the "Dinners & bars" chips are now **Google Maps links** (`dinnerLinks()`,
  `mapUrl()`; chips are `<a>` in the static HTML).

### Competition format — REBUILT, config-driven
- Single source of truth: `ROUND_CFG` (near the top of the script). To change a round, edit
  one line. Current locked plan (10 cup points, 5½ to win):
  - R1 Kingsbarns — Pairs match play, Better Ball — 2 pts
  - R2 Dumbarnie — Pairs match play, Combined (aggregate) — 2 pts
  - R3 Carnoustie — Team foursome, best 2 net — 1 pt
  - R4 St Andrews New — Team foursome, best 4 net — 1 pt
  - R5 St Andrews Old — Singles — 4 pts
- Round state is now `rounds[cid] = {scores:{}, results:{}}`. `LS_KEY` bumped to
  `fs-invitational-v9` (old saves won't load, intentional).
- Match play rounds (pairs/singles): **tap the winner** (Fife win / Halved / Angus win),
  `results[matchIdx]`. Scores optional, only feed the individual board. No net-computed result.
- Foursome rounds: enter each gross, `teamFoursomeNet()` adds the best-N nets, low total wins
  the point (`foursomeResult()`).
- `cupTotals()` sums points across all rounds. `roundPoints()` = points a round is worth.
- **Teams are locked** (removed the dropdowns/rename). `ensureShape()` forces duos back to the
  seed every load. Standing father-son duos: Fife = Brett+Bruce, Kyle+Ron; Angus =
  Justin+Robert, Andrew+Mike.
- **Individual net leaderboard** kept, now with a **low-net shout-out** card up top (`champ`).
- **Groupings view** (`#groupings`, `renderGroupings()`, `groupsForRound()`): the playing
  groups each round derived from the format, plus a who-plays-with-whom count matrix. It
  confirms Justin's worry: every father-son pair currently shares all 5 rounds (the "5" cells),
  cross pairings sit at 2-3.

### Voice / copy
- All prose rewritten in Justin's voice, every em dash removed (47 → 0), trophy emoji removed.
  Fixed a factual bug (old copy said New Course was the swap round; there is no swap now).

## Verified
- `node --check` clean. Rendered in browser: masthead/crest, schedule summary, format table,
  locked teams, groupings + matrix, win-toggle match cards, foursome score panel, individual
  board empty state. Win toggle updates the cup. Foursome panel computes team totals.
- Not visually confirmed at mobile width (extension screenshots capture desktop width), but the
  CSS is standard responsive grid with mobile overrides.

## OPEN with Justin (his calls, all cheap to change)
1. **Logo file**: save the real artwork to `assets/logo.png`.
2. **Format still being polled with the group**: he's weighing ending on a team foursome
   best ball instead of singles, and wants to mix up who plays together. Any round change is a
   one-line `ROUND_CFG` edit.
3. **Groupings are derived, not editable.** Next step if he wants real mixing: an editable
   per-day group assigner (drag/select people into groups), with the count matrix updating live.
4. Points weighting (2/2/1/1/4 = 10) is an assumption; easy to rebalance.

## Commit
Checkpoint committed + pushed to `GTMpath/handicap-calculator-scotland` main (push as GTMpath).

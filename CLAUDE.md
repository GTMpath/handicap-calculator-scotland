# The 2nd Eagle Father & Son Invitational — Trip HQ

Single-file web app (`index.html`, no build step, no backend) that runs a Scotland golf
trip: day-by-day schedule, WHS handicap calculator, and a two-team cup competition with live
scoreboard, groupings view, and individual net leaderboard. State persists to `localStorage`
on the device, there is no server. Branded as the Father & Son Invitational (navy/blue logo
palette); the app still lives in the `handicap-calculator-scotland` repo.

- **Live**: served straight from `index.html` (open the file or host statically).
- **Repo**: `GTMpath/handicap-calculator-scotland` (public). Push as `GTMpath` or
  `spartacus3131` — run `gh auth switch` first; the machine defaults to another account.
- **Trip dates**: Jul 8–14 2026. Roster: Brett, Bruce, Kyle, Ron, Andrew, Mike, Justin, Robert.
- **Teams**: Fife (A, navy) vs Angus (B, claret), two father-son duos each. Locked to the seed.

## Architecture (all inside `index.html`)

- `<style>` — design tokens live in `:root` (see Design system below). One mobile media
  query at `max-width:640px`.
- `COURSES` — the five courses + Kingsbarns, realistic tees only, with CR/Slope/Par per tee.
  Carnoustie plays par 70 off the Yellows/Greens; that per-tee `par` override matters.
- `SCHEDULE` — the seven trip days; golf days link to a course by `id`.
- `ROUND_CFG` — the 5-round cup format, ONE source of truth. To change a round, edit one line.
  `team: 'pairs'|'singles'` are match play (tap the winner, own-ball formats vary by round);
  `'foursome'` is stroke (enter gross scores, best-N team net wins the point). A round can
  carry its own `pts` field to weight it differently from the default. LOCKED plan (confirmed
  2026-07-02, supersedes the 2026-07-01 version): R1 Kingsbarns pairs Combined (aggregate,
  2 pts), R2 Dumbarnie pairs High-Low (2 pts, a new format: one point for low ball + one for
  low total each hole), R3 Carnoustie pairs Better Ball (2 pts, the forgiving game on the
  hardest course), R4 St Andrews New Singles mixed/split round (4 pts, the one round where
  fathers and sons split up), R5 St Andrews Old team foursome best 3 nets (2 pts, the one team
  game, softens a blow-up hole). Cup total is 12 points, 6½ to win.
- `ROUND_GROUPS` — playing groups per round (who walks together), locked via a brute-force
  Python optimization run outside the repo (scratch scripts, not committed): proved
  everyone-plays-everyone-twice is impossible across 5 foursome rounds, so the floor is
  father-son together 4 of 5 rounds (split only at the New Course singles), Brett plays
  Andrew 3x / Kyle 2x / Mike 2x, and everyone plays everyone at least once. `matchesForRound`
  (pairs) and `groupsForRound` both read this. UI calls them "pairings," not "duos" (internal
  `duos`/`teamDuos`/`SEED_DUOS` names kept).
- `HCP_ALLOWANCE` (0.80) — the cup is played off 80% of course handicap. This is applied
  directly to `roundCourseHcp` (the number actually used for net scoring), not just to the
  displayed Playing HCP column. There used to be a per-course allowance dropdown that only
  changed the display while nets used full handicap; that dropdown/state is gone, replaced by
  this single constant applied everywhere.
- State: `players`, `teeChoice`, `rounds` where each round is
  `{scores:{pid:gross}, results:{matchIdx:'A'|'B'|'half'}}`, saved under `LS_KEY`
  (`fs-invitational-v9`; bump the suffix when the shape changes). Teams are LOCKED to the seed.
- Render functions are named `render*`; live in-place updaters (`updateMatchLive`,
  `updateIndividualLive`) exist so typing a score doesn't drop input focus.

## Handicap math

`Course HCP = Index × (Slope ÷ 113) + (Course Rating − Par)`, rounded. Playing HCP applies
the comp allowance. Net = gross − rounded Course HCP. Match play: low net wins, 1 pt, ½ tied.
Better Ball takes a duo's lower net; Aggregate adds both. Every format is own-ball.

## Design system

Navy/blue heraldic identity taken from the event logo (was a green newsprint look).
- Palette (`:root`): `--paper` #e7ecf3, `--card` #f7f9fc, `--ink` #16243f (navy),
  `--gorse` #3f5c8c (denim/saltire, the ACCENT, name kept from the old palette),
  `--teamA` #16243f (Fife navy), `--teamB` #8a3b46 (Angus claret). JS `TEAM_COLORS` must
  match `--teamA/--teamB`.
- Type: Space Grotesk (display), Inter (body), Space Mono (data/labels/eyebrows).
- Ruled-paper background, 2px ink borders, dark navy tab bar with a denim active tab (white
  text + bottom accent). Masthead loads the real event logo from `assets/logo.jpeg`
  (circle-clipped), with an inline SVG crest as the `onerror` fallback if the file is missing.
- Keep the boldness in the signature (masthead + the live scoreboard). Everything else quiet.

## Gotchas

- `localStorage` writes are wrapped in try/catch — a sandboxed preview silently no-ops
  persistence; that's expected, not a bug.
- `.card-line .k` labels are **mobile-only** card labels. They must be `display:none` on
  desktop or they duplicate the table header inside every cell.
- Bump `LS_KEY` whenever the saved-state shape changes, or returning users load stale data.
- iOS zooms on focus if inputs are <16px — the mobile query already bumps them to 16px.

## Session logs

One dated file per working session in `sessions/`. Newest wins for "where did we leave off."

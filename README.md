# Marble Jar Game

A kid-friendly iPad app for testing probabilistic reasoning in children. Based on the classical urn task paradigm from Bayesian cognition research. Single HTML file, zero dependencies.

## What it tests

Children see a jar being "randomly chosen" from 13 possible jars (4 to 16 red marbles out of 20). They draw 6 marbles in two rounds and make a guess after each round. The key research signal is the gap between guess 1 and guess 2: does the child update their belief after seeing new evidence? Bayesian-optimal behavior is proportional updating. Anchoring shows as no update. Noise sensitivity shows as wild swings.

## Game flow

1. **Intro screen** - Shows all 13 jars with actual marble fills (4 red through 16 red), title, brief explainer, Let's Play button.
2. **Shuffle screen** - All 13 plain jars (no marble counts visible) with a spotlight animation that randomly lands on one. The jar chosen is theater only; the actual red count is assigned randomly after the animation.
3. **Play screen** - Draw 3 marbles from the big jar (click/tap), then guess. Draw 3 more, then final guess. Marble draws are Bernoulli sampling with replacement.
4. **Reveal screen** - Shows true red count, both guesses, stars earned, and session scoreboard.

## Scoring

| Distance from true count | Stars | Confetti |
|---|---|---|
| 0 off | 3 stars | yes |
| 1 off | 3 stars | yes |
| 2 off | 2 stars | yes |
| 3-4 off | 1 star | no |
| 5+ off | emoji only | no |

## Persistent counter (top right)

Appears after the first completed round and stays for the session:
- Games played
- Total stars
- Bullseyes (rounds with 3 stars, i.e. within 1 of the true count)

Resets on page refresh. No backend or localStorage used.

## Layout

Designed for iPad. Responds to orientation:

- **Landscape**: 2 rows of jars (7 + 6), jars sized at 11vh height
- **Portrait**: 3 rows of jars (5 + 5 + 3), jars sized at 10vh height

Both orientations use the same flex-wrap layout with `display: contents` on the row divs so orientation media queries control items-per-row without JS.

## File structure

```
urn-game-ipad/
  index.html    - entire game (HTML + CSS + JS, no build step)
  README.md     - this file
```

## Running locally

```bash
cd urn-game-ipad
python3 -m http.server 8081 --bind 0.0.0.0
```

Open `http://localhost:8081` on laptop or `http://<your-local-ip>:8081` on iPad (same WiFi).

On Windows you need a firewall rule to allow the iPad through:
```
netsh advfirewall firewall add rule name="MarbleGame8081" protocol=TCP dir=in localport=8081 action=allow
```

## Deployment

Deployed on Railway via GitHub repo link. Push to `main` triggers auto-deploy.

Repo: `karkipra/urn-game-ipad`

## Game parameters

| Constant | Value | Meaning |
|---|---|---|
| `NUM_JARS` | 13 | Jar types shown (4 red to 16 red) |
| `TOTAL` | 20 | Total marbles per jar |
| `MIN_RED` | 4 | Minimum red marbles |
| `MAX_RED` | 16 | Maximum red marbles |
| `DRAWS_TOTAL` | 6 | Total draws per round |
| `DRAWS_R1` | 3 | Draws before first guess |

## Key implementation notes

- Marble positions use a seeded hex-grid (Park-Miller LCG shuffle) so the same jar always shows the same visual layout
- SVG viewBox `0 0 100 120` with marble positions centered at x=50
- Orientation layout uses `display: contents` on `.jars-row` divs so all 13 `.jar-item` elements participate directly in the parent flex container. CSS `@media (orientation: landscape/portrait)` sets `width: calc(100%/7)` vs `width: 20%` to control wrapping
- Tap highlight suppressed globally with `-webkit-tap-highlight-color: transparent` and `user-select: none`
- No em dashes anywhere (house rule)

## Not yet built

- Data logging (no backend, no localStorage)
- Participant ID or session labels
- Researcher admin view
- Configurable difficulty
- Kiosk / forced fullscreen mode

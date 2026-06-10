# Marble Jar Game (Urn Task)

A kid-friendly (~age 7+) iPad-ready implementation of the classical urn task from Bayesian reasoning research. Single self-contained HTML file, zero dependencies.

## What it is

The **urn task** is a well-studied paradigm in cognitive science for measuring probabilistic reasoning and belief updating. This version wraps it in a playful "marble jar" metaphor suitable for children.

### Game mechanics

- 13 possible jars, each containing 20 marbles with a different number of red ones (4 to 16 red)
- One jar is secretly selected at random each round
- The player draws 6 marbles total, split into two rounds:
  - Draw 3 marbles, then make a **first guess** (how many red out of 20)
  - Draw 3 more marbles, then make a **final guess**
- Marbles are drawn **with replacement** (Bernoulli sampling)
- Scoring is based on the final guess only:
  - 0-1 off: 3 stars + confetti
  - 2 off: 2 stars + confetti
  - 3-4 off: 1 star
  - 5+: encouragement emoji, no stars
- Session score (games + stars) persists in-tab across rounds

### What the two-guess structure tests

The gap between guess1 and guess2 is the primary research signal: it measures how much a child updates their belief after seeing new evidence. A child who never updates (guess2 always equals guess1) shows anchoring or conservatism. A child who wildly swings shows noise sensitivity. Bayesian-optimal behavior is gradual, proportional updating.

## File structure

```
urn-game-ipad/
  index.html    - the entire game (HTML + CSS + JS, no build step)
  README.md     - this file
```

## Running locally

Just open `index.html` in any browser. No server needed.

For iPad testing over a local network (e.g. Mac running the file, iPad as client):

```bash
# From the project directory
python3 -m http.server 8080
# Then open http://<your-mac-ip>:8080 on the iPad
```

## Hosting

The game is a single static HTML file, so any static host works:

- **GitHub Pages**: push to a repo, enable Pages on `main` branch root
- **Netlify**: drag-and-drop the folder at netlify.com/drop
- **Vercel**: `vercel --prod` from the directory

All three give an HTTPS URL suitable for iPad Safari with no further configuration.

## iPad-specific notes

- Touch targets: the big jar (260x320px) and stepper buttons (56x56px) are large enough for finger taps
- No hover states are required for any core interaction
- `viewport` meta tag is set for device-width scaling
- Designed at 880px max-width, fits well in both portrait and landscape on iPad
- Comic Sans renders natively on iOS

## Current game parameters (all in JS constants at top of script)

| Constant | Value | Meaning |
|---|---|---|
| `NUM_JARS` | 13 | Number of distinct jar types (4 red to 16 red) |
| `TOTAL` | 20 | Total marbles per jar |
| `MIN_RED` | 4 | Minimum red marbles in any jar |
| `MAX_RED` | 16 | Maximum red marbles in any jar |
| `DRAWS_TOTAL` | 6 | Total draws per round |
| `DRAWS_R1` | 3 | Draws before the first guess |

## What is NOT yet implemented

- Data collection / response logging (no backend)
- Participant ID or session labeling
- Researcher / admin view
- Configurable difficulty (draw count, jar range)
- Forced fullscreen / kiosk mode for unsupervised testing
- Persistent scores across page refreshes (no localStorage yet)

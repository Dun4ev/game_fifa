# Sakura Brick 11v11 Football

Single-file Three.js 11v11 football prototype with a Sakura Brick visual theme.

## What It Is

`index.html` contains the whole game: HTML, CSS, and vanilla JavaScript.

Features:
- Three.js r160 WebGL scene.
- 11v11 players: blue home team vs red away team.
- 3-minute quick match.
- Standard 110m x 68m pitch with penalty boxes, center circle, goals, and striped grass.
- Sakura-themed surroundings: torii gate, pagoda, lotus pond, sakura garden, lanterns.
- Keyboard and basic gamepad input.
- Passing, through balls, charged shots, sprinting, player switching, tackles, goals, pause.

## Run

The game can be opened directly as a file:

```text
file:///Users/j15/Documents/New%20project%202/index.html
```

Recommended local server:

```bash
cd "/Users/j15/Documents/New project 2"
python3 -m http.server 4173
```

Then open:

```text
http://127.0.0.1:4173/index.html
```

## Controls

Keyboard:
- `WASD` - move selected home player
- `Shift` - sprint
- `J` - pass
- `K` - through ball
- `Space` - charged shot
- `Q` - switch selected player
- `P` - pause / resume
- `R` - restart kickoff

Xbox-style controller:
- Left stick - move
- Right trigger - sprint
- `A` - switch
- `Y` - pass
- `B` - through ball
- `LB` - shoot

## Gameplay Notes

- The player controls the home team.
- Away team is AI-controlled.
- Home teammates also press and tackle in their zones when away has possession.
- When a home teammate wins the ball, control switches to that player.
- Tackles can either win possession or deflect the ball loose.

## Files

- `index.html` - playable game.
- `DEV_PROMPT_AND_BUGS.md` - original prompt plus bugs found and fixes applied.
- `README.md` - this file.

## Known Limits

- This is a prototype, not a physics-accurate football simulation.
- Players are simple generated 3D shapes, not rigged character models.
- AI is heuristic-based and intentionally lightweight.
- Three.js is loaded from a CDN, so internet access is needed for first load unless cached.


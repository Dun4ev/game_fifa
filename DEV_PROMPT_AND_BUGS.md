# Sakura Brick 11v11 Football - Dev Prompt and Bug Log

## Original Dev Prompt

### Three.js 11v11 Football Game - Dev Prompt

#### Project Overview
- Name: Sakura Brick 11v11 Football
- Stack: Three.js r160 + Vanilla JavaScript
- Format: Single HTML file (2228 lines)
- Match Duration: 3-minute quick match

#### Pitch Design
- Dimensions: 110m x 68m (standard football pitch)
- Penalty area depth: 16.5m, width: 40m
- Goal: 14m wide x 4.5m deep
- Striped grass surface (12 alternating color bands)
- Surroundings: torii gate, pagoda, lotus pond, sakura garden

#### Player System
- 22 players total (11v11)
- Home team blue vs Away team red, goalkeeper in yellow
- Player radius: 1.05 units
- Player controls home team, AI controls away team

#### Controls
- WASD: Move
- Shift: Sprint
- J: Pass
- K: Through ball
- Space: Charged shot
- Q: Switch player
- R: Restart kickoff
- Xbox controller: LS move / RT sprint / A switch / Y pass / B through ball / LB shoot

#### Technical Implementation
- Renderer: WebGL, antialiasing enabled, shadow map 2048 x 2048
- Lighting: HemisphereLight + DirectionalLight
- Materials: MeshStandardMaterial (PBR)
- Animation: requestAnimationFrame at 60fps
- Input: Keyboard Events + Gamepad API

#### Visual Theme
- Theme name: Sakura Brick
- Primary colors: Home #2f7df6 / Away #f04c45 / Accent #f3c944
- Dark semi-transparent UI (backdrop-filter blur)
- Goal toast animation

## Bugs and Issues Found During Review

### 1. Player overlap near ball carrier
- Symptom: a red player and a blue player could visually merge into one stacked character.
- Cause: AI defender moved directly into the ball carrier's coordinates, and there was no player spacing resolution.
- Fix: added side marking behavior and `resolvePlayerSpacing()` to softly separate players.

### 2. Pass did not seem to work
- Symptom: pressing `J` looked like no pass happened.
- Causes:
  - Short key taps could be missed between animation frames.
  - The passer could immediately reclaim the ball after kicking it.
  - A nearby opponent could instantly intercept a pass before the intended teammate had a chance.
- Fix:
  - Added pending keyboard action events.
  - Added ball pickup cooldown.
  - Added intended receiver protection for a short pass window.

### 3. Opponent followed the player but did not tackle
- Symptom: red defenders shadowed the blue ball carrier without trying to win the ball.
- Cause: ball ownership logic only worked for loose balls; no explicit tackle mechanic existed while a player owned the ball.
- Fix: added `resolveTackles()` for active tackle attempts, tackle distance, tackle cooldown, and short lockout for the tackled player.

### 4. Tackle checked distance to ball instead of owner
- Symptom: red defender stood close to the player but still failed to tackle.
- Cause: the ball is rendered slightly in front of the owner, so distance to the ball was larger than distance to the owner's body.
- Fix: changed tackle detection to use distance to the current ball owner's body position.

### 5. Home AI did not try to recover the ball
- Symptom: when red controlled the ball, blue teammates did not actively defend in their zones.
- Cause: tackle logic initially worked only for red defenders against blue possession.
- Fix: made tackling bidirectional and added zone-based home pressers with `getZonePressers()`.

### 6. No automatic player switch after home AI recovery
- Symptom: if a home teammate won the ball, control could remain on the previous selected player.
- Cause: switching was only manual unless ownership changed through the existing `claimBall()` path.
- Fix: `claimBall()` now switches control to the home player who gains possession.

### 7. Normal pickup could steal from an owned ball
- Symptom: the selected blue player could reclaim possession just by being close, bypassing tackle logic.
- Cause: manual pickup checked proximity to the ball even when another player already owned it.
- Fix: manual pickup now only applies to loose balls.

### 8. Pause was missing
- Symptom: there was no way to stop the quick match once running.
- Fix: added `P` pause/resume. While paused, timer, AI, ball, and player updates stop, and HUD shows `Paused`.

## Current Implementation Notes

- Main file: `index.html`
- Runtime: browser, no build step.
- External dependency: Three.js r160 from CDN.
- Recommended local run:

```bash
python3 -m http.server 4173
```

Then open:

```text
http://127.0.0.1:4173/index.html
```


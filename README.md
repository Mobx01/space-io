# Space.io Enhanced

Space.io Enhanced is a fast-paced, single-lane-dodging arcade game built with pure HTML, CSS, and JavaScript.  
You dodge obstacles, collect coins, and grab power-ups while the difficulty ramps up over time. The game includes juicy visual feedback like screen shake and particle trails, plus a settings panel for fine-tuning audio.

---

## Features

- **Endless lane-based runner**
  - 4 vertical lanes with smooth lane switching.
  - Increasing difficulty over time: faster obstacles and tighter spawn intervals.
- **Randomized obstacle system**
  - Obstacles spawn at randomized intervals between a configurable minimum and maximum.
  - Burst spawns: occasional groups of obstacles for intense moments.
  - Lane clustering: new obstacles sometimes spawn near the previous lane to create patterns.

- **Collectibles and power-ups**
  - **Coins**:
    - Spinning 3D-style coin visual.
    - Each coin increases your score by a fixed amount.
  - **Shield power-up**:
    - Temporarily lets you survive one collision with an obstacle.
    - HUD pill indicates active shield duration.
  - **Slow-motion power-up**:
    - Temporarily slows down obstacle movement.
    - HUD pill indicates slow effect duration.

- **Dynamic difficulty**
  - Base obstacle speed increases at regular time intervals.
  - Spawn intervals shrink over time to increase pressure.
  - Difficulty parameters are configurable via a central `CONFIG` object in the script.

- **Visual & feedback effects**
  - **Parallax-scrolling city background**:
    - Main background moves horizontally based on game speed.
    - Three additional parallax layers add depth with subtle, independent motion.
  - **Global screen shake**:
    - Triggered on:
      - Direct collisions with obstacles.
      - Shield-breaking collisions.
      - Occasional near-misses (close passes without impact).
    - Uses a CSS keyframe animation applied to the game container.
  - **Player trail particles**:
    - Soft, orange glow particles spawn behind the player at short intervals while moving.
    - Each particle fades and disappears over a brief period for a smooth trail effect.
  - **Score HUD & best score**:
    - Live score display increments with survival time and coin pickups.
    - Best score is stored in `localStorage` and persists across sessions.

- **Audio & Settings**
  - **Background music**:
    - Looped `<audio>` element controlled via JavaScript.
    - Respectful of mute state and user interaction (to satisfy browser autoplay policies).
  - **SFX system using Web Audio API**:
    - Beep-based sound effects for:
      - Game start.
      - Coin collection.
      - Power-up pickup.
      - Hit / game over.
      - Difficulty level-up.
  - **Settings panel (overlay)**:
    - Accessible from:
      - Topbar ⚙️ button.
      - Pause menu “Settings” button.
    - Includes:
      - **Music volume slider** (0–100%).
      - **SFX volume slider** (0–100%).
    - Values are saved to `localStorage` and restored on reload.
    - Background music volume is directly controlled; SFX gain scales with the SFX slider.

- **Responsive controls & input**
  - **Keyboard**:
    - `Arrow Up` / `W` → move up one lane.
    - `Arrow Down` / `S` → move down one lane.
  - **Mouse wheel**:
    - Scroll up → move up.
    - Scroll down → move down.
  - **Touch / mobile**:
    - Tap top half of the game → move up.
    - Tap bottom half → move down.
    - Swipe gestures also supported via touchstart/touchend handling.
  - **Pause / resume**:
    - Dedicated Pause button in the topbar.
    - Auto-pause on window blur (when user switches tabs / windows).
    - Overlays for:
      - Main menu.
      - Pause screen.
      - Game over screen.

---

## Tech Stack

- **HTML5** for structure and `<audio>` integration.
- **CSS3** for:
  - Layout and responsive design.
  - Parallax background layers and blurred page background.
  - Screen shake keyframe animation.
  - Coin spin animation and trail particle styling.
- **Vanilla JavaScript** for:
  - Game loop and timing using `requestAnimationFrame`.
  - Entity management (obstacles, coins, power-ups).
  - Collision detection using DOM `getBoundingClientRect()`.
  - Audio using `<audio>` and Web Audio API (for SFX).
  - State management and persistent settings via `localStorage`.

---

## How to Play

1. **Start**
   - Open `index.html` in a modern browser.
   - Click **“Start Game”** on the main menu.

2. **Controls**
   - Keyboard:
     - `Arrow Up` or `W` → Move up a lane.
     - `Arrow Down` or `S` → Move down a lane.
   - Mouse:
     - Scroll wheel up/down to move between lanes.
   - Touch:
     - Tap top half of the game area → Move up.
     - Tap bottom half → Move down.
   - Pause:
     - Click the **Pause** button in the top bar.
     - Game auto pauses when the tab/window loses focus.

3. **Objective**
   - Dodge obstacles as long as possible.
   - Collect coins to increase score.
   - Use power-ups (shield and slow) to survive longer.
   - Try to beat your **Best** score stored locally.

---

## Settings & Audio

- Click the **⚙️ Settings** button in the topbar or **Settings** from the pause menu.
- Adjust:
  - **Music Volume** (0–100%): Controls background music volume.
  - **SFX Volume** (0–100%): Controls all beep-based sound effects.
- Changes are:
  - Applied immediately.
  - Saved to `localStorage` under a settings key and restored at reload.

Use the **🔊 / 🔇** button to quickly mute/unmute all audio (music + SFX).

---

## Project Structure

Basic structure for deployment (e.g., GitHub Pages):


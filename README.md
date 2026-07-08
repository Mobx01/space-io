# Space.io

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)](#) [![Version](https://img.shields.io/badge/version-1.0.0-blue)](#) [![License](https://img.shields.io/badge/license-MIT-blue)](#)

Space.io is a fast-paced, single-lane-dodging arcade game built entirely with pure HTML, CSS, and Vanilla JavaScript. Players dodge obstacles, collect coins, and utilize power-ups while the game engine dynamically ramps up difficulty over time. The project features advanced visual feedback, including screen shake, particle trails, parallax background scrolling, and a custom Web Audio API sound system.

## Table of Contents
- [Core Features and Gameplay](#core-features-and-gameplay)
- [Tech Stack](#tech-stack)
- [Visuals](#visuals)
- [Installation and Setup](#installation-and-setup)
- [Usage Examples](#usage-examples)
- [Time Complexity and Performance](#time-complexity-and-performance)
- [Game Engine Pipeline](#game-engine-pipeline)
- [Project Architecture](#project-architecture)
- [Contributing](#contributing)
- [Changelog](#changelog)
- [License](#license)
- [Contact and Support](#contact-and-support)

## Core Features and Gameplay

### Endless Lane-Based Runner
* Features 4 vertical lanes with smooth transitioning.
* Increasing difficulty over time via faster obstacles and tighter spawn intervals.

### Randomized Obstacle System
* Obstacles spawn at randomized intervals between a configurable minimum and maximum.
* **Burst spawns:** Occasional groups of obstacles for intense moments.
* **Lane clustering:** New obstacles sometimes spawn near the previous lane to create recognizable patterns.

### Collectibles and Power-ups
* **Coins:** Spinning 3D-style visual. Each coin increases the score by a fixed amount.
* **Shield:** Temporarily allows survival of one collision with an obstacle. Represented by a HUD pill.
* **Slow-motion:** Temporarily slows down obstacle movement.

### Visual and Feedback Effects
* **Parallax-Scrolling City:** Main background moves horizontally, supported by three additional parallax layers for depth.
* **Global Screen Shake:** Triggered on direct collisions, shield-breaking, and near-misses.
* **Player Trails:** Soft, glowing particles spawn behind the player and smoothly fade out.
* **Score HUD:** Live score display increments with survival time and coin pickups. Best score persists via local storage.

### Audio and Settings
* **Background Music:** Looped audio element respectful of browser autoplay policies.
* **SFX System:** Utilizes the Web Audio API for custom beep-based effects (start, coin collect, power-up, hit, level-up).
* **Settings Panel:** Allows independent volume control for music and SFX. Values are saved persistently.

### Responsive Controls
* **Keyboard:** Arrow Up/W to move up, Arrow Down/S to move down.
* **Mouse:** Scroll wheel up/down to switch lanes.
* **Touch:** Tap top/bottom half of the game area, or use swipe gestures.

## Tech Stack
* **Markup & Structure:** HTML5 (including native Audio integration).
* **Styling:** CSS3 (Flexbox layout, Keyframe animations, Parallax layering).
* **Logic & Engine:** Vanilla JavaScript (ES6+).
* **Browser APIs:** DOM Manipulation, Web Audio API, LocalStorage API, `requestAnimationFrame`.

## Visuals
[Insert screenshot, GIF, or gameplay footage demonstrating the parallax scrolling, obstacle dodging, and particle trails.]
*(Placeholder for gameplay GIF)*

## Installation and Setup

### Prerequisites
Because the game runs entirely in the browser using Vanilla web technologies, no build tools or package managers (like npm) are required. You only need a modern web browser.

### Step-by-Step Installation

1. Clone the repository to your local machine:
   ```bash
   git clone [https://github.com/Mobx01/space-io.git](https://github.com/Mobx01/space-io.git)
   cd space-io
   ```

2. Launch the game:
   Simply double-click the `index.html` file to open it in your default web browser.

   *(Note: If your browser restricts local file audio playback due to CORS policies, you can run a quick local server instead:)*
   ```bash
   # If using Python
   python -m http.server 8000
   
   # Or using Node.js
   npx http-server
   ```
   Then navigate to `http://localhost:8000` in your browser.

## Usage Examples

Below are snippets demonstrating how the custom game engine handles core logic.

**Collision Detection Logic (AABB):**
```javascript
// Example of the Axis-Aligned Bounding Box (AABB) intersection check used in the game loop
function rectsIntersect(a, b) {
  return !(a.right < b.left || 
           a.left > b.right || 
           a.bottom < b.top || 
           a.top > b.bottom);
}
```

**Web Audio API Synthesizer (SFX):**
```javascript
// Example of how the engine dynamically generates sound effects without audio files
function beep(freq = 440, time = 0.08, gain = 0.12) {
  if (state.muted || state.sfxVol === 0) return;
  initAudio();
  const oscillator = audioCtx.createOscillator();
  const gainNode = audioCtx.createGain();
  
  oscillator.type = 'sine';
  oscillator.frequency.value = freq;
  gainNode.gain.value = gain * state.sfxVol;
  
  oscillator.connect(gainNode);
  gainNode.connect(audioCtx.destination);
  oscillator.start();
  oscillator.stop(audioCtx.currentTime + time);
}
```

## Time Complexity and Performance
* **Collision Detection:** O(N) per frame, where N is the number of active entities on screen. The engine optimizes this by strictly checking intersections only against the player's bounding box.
* **Garbage Collection (DOM Recycling):** O(K) where K is the number of off-screen entities purged per frame, keeping memory footprint low and stable.
* **Rendering Update:** O(N) per frame for DOM translation updates (adjusting `style.left`).

## Game Engine Pipeline
The application utilizes a custom `requestAnimationFrame` loop that acts as the primary data pipeline, executing the following phases every frame:
1. **Time Delta Calculation:** Computes the delta time (dt) between frames to ensure movement speeds are mathematically decoupled from the monitor's refresh rate.
2. **State & Environment Update:** Translates the parallax background layers and checks dynamic difficulty timers to scale base speeds.
3. **Entity Processing:** Iterates through all spawned entities (obstacles, coins, power-ups), applying velocity and calculating new screen coordinates.
4. **Collision Phase:** Fetches DOM bounding boxes and executes AABB overlap checks to handle shield breaks, near-miss screen shakes, or game-over states.
5. **DOM Pruning:** Detects entities that have crossed the left boundary threshold and removes their corresponding DOM nodes to prevent memory leaks.

## Project Architecture
The project is structured as a lightweight, single-page application to maximize load speed and accessibility:
* `index.html` - Contains the entire application. It is divided into three distinct blocks:
  * **HTML Structure:** Defines the game container, UI overlays, scoreboards, and touch zones.
  * **CSS Styles:** Handles all visual design, keyframe animations (coin spins, screen shakes), and z-index layering.
  * **JavaScript Engine:** Contains the core configuration object (`CONFIG`), state management, procedural generation logic, and the central `gameLoop`.
* `/assets` - Contains associated media such as `bg-city.png`, `music.mp3`, and entity sprites (`obs1.png`, `shield.png`, etc.).

## Contributing

We welcome contributions to improve this project. To contribute:

1. Fork the repository.
2. Create a new branch (`git checkout -b feature/your-feature-name`).
3. Ensure your code follows the established conventions.
4. Commit your changes (`git commit -m 'Add some feature'`).
5. Push to the branch (`git push origin feature/your-feature-name`).
6. Open a Pull Request.

Please report bugs or request features by opening an issue in the GitHub issue tracker.

## Changelog

* **v1.0.0** - Initial release featuring dynamic lane clustering, parallax scrolling, Web Audio API sound effects, and persistent local storage saving.

## License

This project is licensed under the [MIT License](LICENSE) - see the LICENSE file for details.

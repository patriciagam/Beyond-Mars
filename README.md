# Beyond Mars

A Space Invaders-style game written in assembly for the PEPE-16 processor. You pilot a spaceship, fire probes at incoming asteroids, and mine the right ones to keep your energy up.

Project for *Introdução à Arquitetura de Computadores* (Introduction to Computer Architecture), Instituto Superior Técnico.

## Gameplay

Asteroids fall toward your ship. Fire probes to destroy them before they hit you.

- **Energy:** you start with 100, and it drains steadily during play. Shooting a *minable* asteroid restores 25.
- **Two asteroid types:** minable asteroids give energy when destroyed. Non-minable ones are just a threat.
- **Three probes:** left, front and right, each fired with its own key.
- **Game over:** you run out of energy, an asteroid hits the ship, or you quit.

The energy level is shown on the display in decimal. The game has background scenes, a different end screen for each way of losing, and sound effects for firing and exploding.

## Controls

The game uses the PEPE-16 hex keypad.

| Key | Action |
| --- | --- |
| `C` | Start the game |
| `0` | Fire left probe |
| `1` | Fire front probe |
| `2` | Fire right probe |
| `D` | Pause / resume |
| `E` | End the game |
| `B` | Return to the start screen |

## How it works

The game is built as several cooperating processes, each with its own stack, synchronized with `LOCK`s and driven by timer interrupts:

| Process | Role |
| --- | --- |
| Keyboard | Scans the keypad and sends the pressed key to the game |
| Ship | Draws the ship and its animated lights |
| Display | Converts energy to decimal and updates the display |
| Energy | Drains energy on each tick |
| Probe (x3) | Moves a probe and checks collisions with asteroids |
| Asteroid (x4) | Spawns, moves and draws an asteroid, and handles its collisions |

Asteroid spawn column and direction come from a table of combinations picked pseudo-randomly. Sprites are defined as pixel tables and drawn through the MediaCenter commands.

## Running

Open `grupo39.asm` in the PEPE-16 simulator, assemble it, and run it with the keypad, display and MediaCenter peripherals connected. Press `C` to start.

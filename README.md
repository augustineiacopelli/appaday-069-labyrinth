# AppADay 069 &mdash; Labyrinth

**Live app:** https://augustineiacopelli.github.io/appaday-069-labyrinth/
**Portfolio:** https://augustineiacopelli.github.io/appaday/

Tilt your phone and a steel ball rolls across a wooden board with real gravity behind it. Six hand-cut mazes, each drilled with holes that pull before they swallow. Prise the brass tacks out of the wood, find the maker's marks, and rebuild the toy at the workbench.

| Field | Value |
| --- | --- |
| App number | 069 |
| Category | Games (G) |
| Shipped | 2026-07-15 |
| Stack | Single-file HTML, CSS, vanilla JS, Canvas 2D, DeviceOrientation |
| Dependencies | Google Fonts (Saira Stencil One, Barlow Semi Condensed) |

## What it does

The board is the whole app. On a phone it reads real device tilt and converts pitch and roll into acceleration on a ball. On a desktop, dragging the board or holding the arrow keys tilts it the same way. The board itself pitches in 3D under your hands and the ball's shadow slides with it, so the physical metaphor holds from the first frame.

Holes are not a hit test. Within one hole radius the ball feels a pull toward the centre that grows as it closes, and only commits to falling inside 1.5 units. That gap between *pulled* and *lost* is where the whole game lives: you can feel a hole taking the ball and still tilt out of it.

## Collectables

Eight **brass tacks** are hammered into every board, and one **maker's mark** is hidden on each. Roll over them to prise them out.

The catch is that tacks only bank when you reach the ring. Drop down a hole and you keep them; walk away from the board and you lose the lot. Every tack is a decision about whether to extend a run you are currently winning.

They are also, deliberately, in bad places. Placement was not done by eye. A script floods the board's reachable space, computes the shortest path from start to ring, and then measures every reachable cell by how far it sits off that racing line and how close it sits to a hole. Tacks are drawn from cells that are a real detour *and* near a hole, spread apart so they never cluster. The maker's mark goes somewhere nastier: tightness is a hard gate, so every mark sits between 2.5 and 4.7 units of a hole centre, with the detour used only to break ties. On The Comb the mark ends up jammed against a hole directly on the racing line &mdash; you pass within a ball's width of it on every single run and you still cannot take it.

Everything is verified before it ships. A checker confirms the ring, all 48 tacks, and all 6 marks are reachable on the shipped data, and that nothing is stranded inside a fall radius where it could never be collected.

Find all five marks on boards one through five and **The Maker's Own** unlocks: a sixth board with no walls on it at all, just a 72-hole Archimedean spiral you have to follow inward to the eye.

## The workbench

Tacks buy modifications to the toy, not power-ups. There are 48 in the world and 43 buys everything, so a completionist earns the full set &mdash; but nothing here is free of consequence.

**The rack** holds three balls of identical diameter and different metal. *Steel* is stock. *Brass* is heavy enough to skim a hole's lip and keep going, and heavy enough that you will not stop it when you need to. *Boxwood* is light, so every hole grabs at it, but it sheds speed the instant you level the board, which is the whole game on a tight run. There is no best ball, only a best ball for the board in front of you.

**Fittings** are three. *Felt lining* glues baize along every wall, so the ball stops rattling off them and starts dying against them. *Beeswax plugs* give you three per run, sealing whichever hole is nearest the ball &mdash; including one that is already dragging it under, which turns a lost run into a save. *Magnet knob* is a lodestone on a slide beneath the board: hold the board, or the space bar, to drag the ball to a halt, watching the grip meter drain and waiting for it to recover.

**Melt it all down** refunds everything, because a finite budget you cannot respec is a trap rather than a decision.

## Controls

Tilt on a phone. Drag the board or use the arrow keys or WASD on a desktop.

Because tilt comes from the sensors on a phone, the board itself is free for touch: **hold anywhere on the board** to run the magnet. On a desktop the board is already the steering wheel, so the magnet moves to the **space bar**. `P` or the Seal button places a plug. `R` restarts. **Center** re-zeroes the tilt baseline to however you are holding the phone right now, which is the fix for playing lying down or in a passenger seat.

## Motion permission

iOS 13 and later require an explicit user gesture and HTTPS before a page may read device orientation. The start screen's **Allow motion** button calls `DeviceOrientationEvent.requestPermission()` inside that gesture. If permission is denied, or the device has no sensors, the pointer and keyboard fallbacks take over automatically with no mode to select. Screen rotation is handled by reading `screen.orientation.angle` and remapping beta and gamma, so landscape play steers correctly rather than sideways.

## Notes on the build

The physics runs six substeps per frame with a capped delta, which keeps a fast ball from tunnelling through a two-unit wall. Collision is circle-versus-AABB with a closest-point normal and restitution, so the ball rattles down a corridor instead of sticking to it. The ball's material scales hole pull, damping, and bounce; felt scales bounce again; the magnet spikes damping while the grip lasts.

The wooden board, its grain, knots, walls, holes, and brass ring are rendered once to an offscreen canvas per board and blitted each frame, so the only per-frame drawing is the ball, the uncollected tacks, and the marks.

Progress lives in localStorage, wrapped in try/catch so it degrades to a single session rather than throwing in a sandboxed frame. Everything is one file. No build step, no framework, nothing leaves the device.

---

Part of [AppADay](https://augustineiacopelli.github.io/appaday/), one complete web app shipped every day.

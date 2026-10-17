# SidelineReel Product Case

Interactive product-case prototype for a proposed **Player Identity Confidence Gate**.

## What it demonstrates

SidelineReel automatically generates weekly player highlight reels. The prototype adds a safety layer:

- High-confidence identity matches continue automatically.
- Low-confidence clips are blocked.
- A human reviewer can **confirm**, **reassign**, or **exclude** ambiguous clips.
- Only verified clips can be published or used as a reel thumbnail.

## Run the prototype

Open:

`./sidelinereel_prototype.html`

No build step or external dependency is required.

## Demo path

1. Preview Lucas, Emma, or Diego to see normal generated reels.
2. Preview Maya #14 to see a generated draft with uncertain clips blocked.
3. Open **Review identity**.
4. Confirm/reassign/exclude the uncertain clips.
5. Preview the verified final reel.
6. Publish.

## Included files

- `sidelinereel_prototype.html` — clickable prototype.
- `sidelinereel_case.md` — roadmap call, spec, acceptance criteria, failure modes, AI workflow note.
- `SidelineReel_motion_graphics.mp4` — motion-graphics walkthrough of the system.

All names, metrics shown inside the prototype, and reel visuals are simulated for demonstration.

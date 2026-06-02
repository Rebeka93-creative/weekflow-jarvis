# WEEKFLOW // S.Y.S.T.E.M. CORE

> A tactical week planner with a Jarvis-style HUD interface — built as a single, self-contained HTML file. No build step, no dependencies, no server required.

![WeekFlow Preview](preview.png)

## Features

- **Week View** — 7-column grid with real dates, drag-and-drop rescheduling, per-day item counts
- **Project View** — track progress per project with completion bars
- **Alerts Centre** — overdue items, deadlines, high priority and upcoming tasks in one place
- **User Panel** — profile, productivity stats, Pomodoro timer, today's agenda, scratch pad
- **J.A.R.V.I.S Voice** — Web Speech API with smart queuing, dedup, and voice selection
- **Ambient Music** — 7-layer procedural cyberpunk synthesizer via Web Audio API (no files needed)
- **Persistent storage** — everything saved to `localStorage`, survives browser restarts
- **Zero dependencies** — one `.html` file, works offline

## Quick Start

```bash
# Option 1 — just open the file
open index.html

# Option 2 — serve locally (avoids any browser file:// quirks)
npx serve .
# or
python3 -m http.server 8080
```

Then open `http://localhost:8080` in your browser.

## Usage

| Action | How |
|---|---|
| Add item | Hover a day column → click `+ ADD ITEM`, or tap ADD in bottom nav |
| Move item | Drag a card to another day column |
| Quick actions | Right-click any item card |
| Complete task | Click the checkbox on any item |
| Jarvis voice | Toggle `🎤 J.A.R.V.I.S` in the top bar |
| Ambient music | Click `▶ AMBIENT` in the top bar |
| Alerts | Tap ALERTS in bottom nav — click any alert to jump to it |
| User profile | Tap USER in bottom nav |

## Item Fields

Each item supports:
- **Type** — Meeting, Task, Deadline, Prep Work
- **Start & end date** — spans multiple days with `RANGE` badge
- **Start time / Finish by**
- **Duration** — free text (e.g. `1h 30m`)
- **Priority** — Low / Medium / High
- **Daily reminder** — shows item every day until end date
- **Prep notes** — what to prepare beforehand
- **Notes** — full description, links, attendees

## J.A.R.V.I.S Speaks When

- Opening the app (personalised greeting with week summary)
- Completing a task
- Creating a new item
- Starting a Pomodoro session
- Overdue items detected on load

## Data & Privacy

All data is stored in your browser's `localStorage` under these keys:

| Key | Contents |
|---|---|
| `wfsys_i` | All items |
| `wfsys_p` | All projects |
| `wfsys_user` | User profile |
| `wfsys_pom` | Pomodoro settings |
| `wfsys_notes` | Scratch pad |

No data ever leaves your device. No analytics, no telemetry, no server.

## Migrating from Previous Versions

The app automatically migrates data from older localStorage keys (`wf_items_v2`, `wfj2_i`, `wfhud_i`) on first load.

## Browser Support

| Browser | Voice | Music | Notes |
|---|---|---|---|
| Chrome / Edge | ✅ Best | ✅ | Recommended |
| Firefox | ✅ Good | ✅ | — |
| Safari | ✅ Good | ✅ | May prompt for audio permission |

> Audio requires a user interaction (click) before it starts — this is a browser security requirement, not a bug.

## Customisation

All design tokens are CSS variables at the top of `index.html`:

```css
:root {
  --primary: #4cd7f6;   /* HUD cyan */
  --surface: #0e1416;   /* OLED background */
  --error:   #ffb4ab;   /* alert red */
  /* ... */
}
```

## License

MIT — do whatever you want with it.

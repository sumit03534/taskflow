# TaskFlow Lite

A small, fast task ledger that runs entirely in the browser — vanilla
HTML/CSS/JS, no frameworks, no build step, no backend.

## Running it

Because `app.js` uses native ES modules (`import`/`export`), most
browsers won't execute it directly from a `file://` URL (a CORS
restriction on module scripts). Serve the folder locally instead:

```bash
cd taskflow-lite
python3 -m http.server 8000
# then open http://localhost:8000
```

Any static server works — `npx serve`, VS Code's "Live Server"
extension, etc.

## Features

- **Create** — type a task and press Enter or hit the + button
- **Read** — tasks render in a ruled-paper list, newest at the bottom
- **Update** — double-click a task (or use the pencil icon) to edit it
  in place; toggle the hand-drawn checkbox to mark it done
- **Delete** — click the trash icon once to arm it, click again within
  3 seconds to confirm (click elsewhere to cancel)
- **Filters** — All / Active / Completed, plus a live "N items left"
  counter and a "Clear completed" action
- **Persistence** — the task list is saved to `localStorage` on every
  change and reloaded on page load
- **Validation** — empty submissions are blocked, a 140-character
  limit is enforced with live feedback, and invalid input shakes
  instead of silently failing

## File structure

```
taskflow-lite/
├── index.html            Main application shell
├── styles/
│   ├── main.css           Core visual design
│   └── utilities.css      Small generic helper classes
├── app.js                 Entry point — state, events, wiring
├── modules/
│   ├── storage.js          localStorage read/write
│   ├── render.js           DOM rendering (list, stats, filter tabs)
│   └── validation.js       Input validation rules
└── README.md
```

## Design notes

The visual concept is a page from a running notebook: warm paper,
a ruled background, and a punch-hole margin. Completing a task draws
a hand-tracked checkmark and an ink strike-through rather than
snapping to a solid state, so the interaction feels closer to marking
up a real page than toggling a UI control.

## Data model

Each task is a plain object:

```js
{
  id: 1731000000000,       // Date.now() at creation
  text: "Learn JavaScript",
  completed: false,
  createdAt: "2026-07-20T09:00:00.000Z"
}
```

## Browser support

Any evergreen browser (Chrome, Firefox, Safari, Edge). Relies on ES
modules, `localStorage`, and standard DOM APIs — no polyfills
included.

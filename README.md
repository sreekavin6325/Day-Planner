# Day Planner

Day Planner is a focused, mobile-friendly daily task list built as a single HTML file. It helps you decide what today needs, carry unfinished work forward, create repeating daily routines, and recover recent changes—all without accounts, a backend, or a build step.

## Features

- Add tasks for today with a keyboard-friendly quick-entry field
- Mark tasks complete or reopen them
- Push tasks to tomorrow
- Edit and delete tasks
- Swipe left to complete and right to move a task forward on pointer-enabled devices
- Separate overdue, today, upcoming, and completed work
- Show a live completed-versus-open progress indicator
- Create reusable “Every day” tasks that are added once per calendar day
- Edit or stop repeating routines
- Undo the 15 most recent changes
- Save tasks automatically in the browser
- Report when browser storage is unavailable
- Adapt automatically to light and dark system themes
- Support mobile safe areas and responsive layouts

## Tech stack

| Layer | Technology |
| --- | --- |
| Interface | Semantic HTML5 |
| Styling | Vanilla CSS with responsive and color-scheme-aware styles |
| Logic | Vanilla JavaScript in an immediately invoked function |
| Persistence | Browser `localStorage` |
| Typography | Archivo, Karla, and IBM Plex Mono from Google Fonts |

## Project structure

```text
day-planner/
└── Day Planner.html   # Complete application: markup, styles, and logic
```

## Getting started

### Quick start

1. Download `Day Planner.html`.
2. Open it in a modern browser.
3. Type a task into **Add a task for today** and press **Enter** or **Add**.

No package installation, framework, development server, or build command is required.

The application itself has no JavaScript dependencies. Its fonts are loaded from Google Fonts, so the intended typography requires an internet connection; the browser falls back to system fonts if they are unavailable.

### Optional local server

You can serve the file over HTTP while developing:

```bash
python -m http.server 8000
```

Then open [http://localhost:8000/Day%20Planner.html](http://localhost:8000/Day%20Planner.html).

## Scripts and build process

There is no `package.json`, dependency installation, compilation, bundling, or automated test command. All behavior is implemented by the inline script in `Day Planner.html`.

The script handles:

- State initialization and browser persistence
- Task creation, editing, completion, reopening, rescheduling, and deletion
- Daily-routine generation
- Recent-change snapshots and undo
- Date classification for overdue, current, and upcoming work
- Progress calculation and interface rendering
- Pointer gestures and keyboard interactions
- Storage-status feedback

## Using the planner

### Manage today's work

Add tasks from the field at the top of the page. Open tasks appear under **Today**, while unfinished tasks from earlier dates appear under **Carried over**.

Each open task provides controls to:

- Mark it done
- Move it to tomorrow
- Edit its text
- Delete it

On pointer-enabled devices, swipe a task left to complete it or right to move it to tomorrow. A swipe must travel more than 80 pixels before the action is applied.

Completed tasks are shown in **Done today** and can be reopened. Tasks scheduled beyond today appear under **Coming up**.

### Create daily routines

Use the **Every day** section for recurring actions such as checking email. Each routine creates one open task for the current date when the planner is first opened that day.

Changing a routine updates its future generated text but does not rewrite task copies that were already created. Stopping a routine prevents new copies; it does not remove earlier entries.

### Undo changes

The planner stores a snapshot before each meaningful change and keeps up to 15 snapshots. The undo bar identifies the latest action that can be reversed.

Undo history is saved with the rest of the planner state, so it can survive a reload when browser storage is available.

## Data storage

All state is stored in `localStorage` under:

```text
dayplanner.v1
```

The saved state contains:

```json
{
  "seq": 3,
  "tasks": [
    {
      "id": "t2",
      "t": "Prepare tomorrow's agenda",
      "date": "2026-10-06",
      "done": false,
      "doneAt": null
    }
  ],
  "hist": [],
  "daily": [
    {
      "id": "d1",
      "t": "Check email",
      "last": "2026-10-06"
    }
  ]
}
```

Data belongs to the browser profile and origin where the page is opened. It is not synced between devices. Clearing site data removes the planner state.

If `localStorage` is blocked, the interface continues working for the current page session and displays a warning that changes will not survive a reload.

## Deployment

Upload `Day Planner.html` to any static host or ordinary web server. No environment variables, API keys, server-side routes, or supporting assets are required.

To make typography fully self-hosted, download the font files and replace the Google Fonts stylesheet URL with local declarations.

The page includes mobile web-app metadata, but it does not include a manifest or service worker and is therefore not an offline-installable PWA.

## Customization

Edit `Day Planner.html` directly:

- CSS variables and media queries control colors, spacing, breakpoints, and light/dark appearance.
- `KEY` controls the browser-storage namespace.
- `HIST_MAX` controls the maximum number of undo snapshots.
- The initial `state.daily` value defines the default daily routine for new users.
- `seedDaily()` controls when repeating tasks are generated.
- `render()` defines the page sections and status text.
- `bindSwipe()` controls gesture thresholds and behavior.

If the saved data structure changes, use a new storage key or add migration logic before publishing the update.

## Current limitations

- Data stays in one browser profile and origin; there is no account or cloud sync.
- There is no built-in import, export, or backup feature.
- Tasks support text and dates but not priorities, times, tags, subtasks, or reminders.
- Daily routines repeat every day only; custom schedules are not supported.
- Completed tasks are surfaced only when they were completed today.
- The current date is determined when the page loads; leave-and-return behavior should use a reload after midnight.
- There is no automated test suite or formal build pipeline.

## License

No license is declared in the source file. Add a license before redistribution or outside contributions.

# Gantt Chart Viewer

A self-contained, zero-dependency Gantt chart application built with plain HTML, CSS, and JavaScript. Opens directly in any modern browser from the file system -- no server, no build step, no framework.

---

## Files

| File         | Purpose                                                                    |
|--------------|----------------------------------------------------------------------------|
| `index.html` | Application (markup, styles, and all logic in a single file)              |
| `data.js`    | Demo dataset -- a 155-task sample project embedded as a CSV string        |
| `data.csv`   | Reference copy of the same data in standard CSV format                    |

`data.js` is included for demonstration purposes so the application has something to render out of the box. Replace its contents with your own project data to use the viewer for real work.

---

## Quick Start

1. Replace the CSV content in `data.js` with your own project data, or use the included sample as-is.
2. Open `index.html` in a browser.

No HTTP server is required. The application runs entirely from the `file://` protocol. Data is loaded via a `<script>` tag, which is not subject to browser CORS restrictions.

---

## Data Format

`data.js` exports a single global variable `window.__GANTT_CSV__` containing a CSV string inside a template literal. The first row must be the header. Supported columns:

| Column         | Required | Description                                                                              |
|----------------|----------|------------------------------------------------------------------------------------------|
| `Task`         | Yes      | Task name (also accepts `Name`, `Title`)                                                 |
| `Start`        | Yes      | Start date in `YYYY-MM-DD` format                                                        |
| `End`          | Yes      | End date in `YYYY-MM-DD` format                                                          |
| `Group`        | No       | Grouping label (also accepts `Category`, `Phase`). Defaults to "Ungrouped"               |
| `Assignee`     | No       | Person assigned (also accepts `Owner`, `Resource`)                                       |
| `Progress`     | No       | Completion percentage, 0-100                                                             |
| `Dependencies` | No       | Semicolon-separated list of predecessor task names (also accepts `Deps`, `Predecessors`) |
| `Color`        | No       | Hex color for the task bar. Auto-assigned by group if omitted                            |

Example row:

```
Core Framework Build,Development,Fiona,2024-11-18,2024-12-20,100,Dev Environment Setup,#F44336
```

---

## Demo Dataset

The included `data.js` contains a sample project with 155 tasks across 10 groups (Initiation, Requirements, Architecture, Design, Development, Testing, Data Migration, Training, Deployment, Post-Launch, and Documentation), 8 assignees, and a timeline spanning 2024 through 2026. Task dates are automatically shifted at load time so the project always appears partially complete relative to the current date. This shifting only applies to the bundled demo data and can be removed by deleting the date-shifting block at the bottom of the `<script>` tag in `index.html`.

---

## Features

### Views

Five zoom levels, selectable from the toolbar:

- **Day** -- One column per day. Shows day-of-month numbers.
- **Week** -- One column per day at reduced width. Labels every Monday.
- **Month** -- One column per week. Header shows month and week-start dates.
- **Quarter** -- One column per month. Header shows quarters and month abbreviations.
- **Year** -- One column per quarter. Header shows years and quarter labels.

The timeline is trimmed to the bounds of the actual project data in every view.

### Filtering

The filter panel (toolbar button) supports three dimensions:

- **Groups** -- Show or hide task groups.
- **Assignees** -- Show or hide tasks by assignee.
- **Dependencies** -- Show or hide tasks by dependency name.

Each section has All/None bulk toggles. A badge on the filter button indicates how many items are currently filtered out. The Reset All button restores defaults.

### Search

The search box filters tasks in real time by name, group, or assignee.

### Theme

A light/dark mode toggle is in the toolbar. The selection is persisted to `localStorage`.

- **Dark mode** -- Navy blue palette.
- **Light mode** -- White and grey palette with blue accents.

### Task Detail Panel

Clicking any task bar or task row opens a side panel showing:

- Task name, group, assignee
- Start date, end date, duration in days
- Progress bar
- Dependency list

### Other

- **Today line** -- A vertical red line marks the current date on the chart.
- **Today button** -- Scrolls the chart to center on the current date.
- **Collapse/Expand** -- Toggle button collapses or expands all task groups at once.
- **Drag and drop** -- Drop a `.csv` file onto the page to load different data at runtime.
- **Resizable panels** -- Drag the handle between the task list and chart to resize.
- **Weekend highlighting** -- In Day and Week views, weekend columns have a subtle background tint.
- **Progress column** -- The progress column in the task list is hidden automatically when no task has progress data.
- **Grouped rows** -- Tasks are grouped by their `Group` field with collapsible section headers.
- **Color-coded bars** -- Each group gets a distinct bar color from a built-in palette, or uses the `Color` column if provided.
- **Scroll sync** -- The task list and chart body scroll vertically in lockstep.

---

## Browser Support

Tested in Chromium-based browsers (Chrome, Edge). Should work in Firefox and Safari. Requires ES6 support (template literals, Sets, arrow functions).

---

## Customization

### Changing the color palette

Edit the `palette` array inside the `loadTasks` function in `index.html`.

### Adjusting column widths per view

Edit the `cellWidth` values returned by `getViewConfig()` in `index.html`.

### Modifying theme colors

Edit the CSS custom properties in the `:root` / `[data-theme="dark"]` and `[data-theme="light"]` blocks at the top of the `<style>` tag.

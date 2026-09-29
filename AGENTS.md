# AGENTS.md

Guidance for AI coding agents (and humans) working on this project. Read this whole file before making changes.

## 1. Project overview

A todo list web app built for HNG Internship Stage 1.

- Tasks live in three sections: **Today**, **Tomorrow** and **Later**.
- Users can add, complete or uncomplete, edit, delete and filter tasks (All / To do / Done).
- Each task can have one optional tag: Work, Personal, Learning or Errands.
- Data persists in the browser via `localStorage`.
- It is deployed as a static site on Vercel.

The owner is a UI/UX designer who is new to coding. **Design fidelity matters as much as working code**, and explanations should be simple and jargon-free.

## 2. File structure

```
todo-app/
├── index.html   # The entire app: HTML + <style> (CSS) + <script> (JS)
├── README.md    # What the app is and its features (for users/reviewers)
└── AGENTS.md    # This file: rules and workflow for agents
```

Inside `index.html`, code is split into clearly numbered, commented sections:

| Part | Sections |
|------|----------|
| CSS  | 1. Design tokens · 2. Base · 3. Page layout · 4. Sections · 5. Task card · 6. Buttons · 7. Add-task form |
| HTML | Header (date, title, "tasks left", filter) · three `<section class="column">` blocks · `<template id="composer-template">` for the add-task form |
| JS   | 1. Settings · 2. State · 3. Storage · 4. Task actions · 5. Helpers · 6. Render · 7. Composer · 8. Editing · 9. Focus helpers · 10. Events · 11. Start |

### Data model

Tasks are stored under the `localStorage` key `hng-todo.tasks.v1` as a JSON array:

```js
{
  id: "uuid-string",
  text: "Buy groceries",
  done: false,
  section: "today" | "tomorrow" | "later",
  tag: "work" | "personal" | "learning" | "errands" | null,
  createdAt: 1727600000000
}
```

The selected filter is stored under `hng-todo.filter.v1` (`"all" | "todo" | "done"`).

If you change the task shape, bump the key version (e.g. `.v2`) and migrate old data in `loadTasks()`.

## 3. Coding rules

### Stack

1. **Single file only.** All HTML, CSS and JS stays in `index.html`. No frameworks, libraries, npm packages or build tools.
2. **Only one external resource:** the Google Fonts stylesheet for Plus Jakarta Sans. Don't add CDNs or trackers.

### How the JavaScript works

3. **State → render.** The variables `tasks`, `filter` and `editingId` are the source of truth. To change the UI:
   - update the state with a task-action function (`addTask`, `toggleTask`, `updateTaskText`, `deleteTask`), which saves to storage;
   - then call `render()`.
   
   Don't edit task cards in the DOM directly.
4. **Security.** Never put user-typed text into `innerHTML`. Use `textContent` (the `el()` helper does this). `innerHTML` is only allowed for the fixed `ICONS` strings.
5. **Storage is fragile.** Every `localStorage` read and write is wrapped in `try/catch`. Validate loaded data (see `loadTasks()`), and the app must still work if storage is blocked.
6. **Event delegation.** Card interactions are handled by the listeners on `#board` using `data-action` attributes. To add a new button, give it a `data-action` and handle it in the `click` switch. Don't attach listeners to each card.

### Design

7. **Design tokens.** Every colour and font size comes from the CSS variables in `:root`. Don't hard-code new colours elsewhere; add a token first.
8. **One accent colour.** `--accent` (muted green) is used **only** for actions: "+ Add task", the Add/Save buttons and ticked checkboxes. Filters, selected tags, focus rings and everything else stay neutral grey or near-black.
9. **Calm visuals.**
   - Cards are white with a 1px subtle border.
   - No heavy shadows, gradients or decorative background shapes.
   - Keep generous white space.
10. **Typography.** One font (Plus Jakarta Sans). Keep the scale: title 32px, section 18px, task 15px, meta 13px.
11. **Tags** always show as a small coloured dot plus the text label. The dot is decorative (`aria-hidden`); the label carries the meaning.
12. **Completed tasks** get a strikethrough, a lighter (but still AA-compliant) text colour and sort to the bottom of their section. Don't use `opacity` on text, because it breaks contrast.

### Accessibility (required, not optional)

13. **Contrast.** All text must meet WCAG AA contrast: 4.5:1 for text, 3:1 for UI outlines. Check any new colour against both `--surface` (white) and `--bg` (grey).
14. **Keyboard.** Everything must work with the keyboard:
    - Enter adds or saves; Escape cancels or closes.
    - Focus must never be lost after an action (see `focusTaskControl`).
15. **Labels.** Every control needs an accessible name (`<label>`, `aria-label` or visible text). Use `announce()` to tell screen readers about adds, edits and deletes.
16. **Touch targets.** Keep tap targets at least 40px, or 44px on touch screens (`--tap`).
17. **Reduced motion.** Respect `prefers-reduced-motion`.

### Responsive

18. **Mobile first.** Sections stack by default and become 3 columns at `min-width: 900px`. There must be no horizontal scrolling at 320px width.

### Style

19. **Write for a beginner.**
    - Use clear names and short functions.
    - Add a brief comment explaining *why* for anything non-obvious.
    - Match the existing comment style and section numbering.

## 4. Workflow for agents

Follow these steps every time you work on this project:

1. **Read first.** Read this file, `README.md` and all of `index.html` before editing. Understand the state → render flow.
2. **Restate the task.** Confirm what the user wants in one or two sentences. If a request conflicts with the rules above (e.g. adding a framework or a second accent colour), point it out and ask before proceeding.
3. **Plan small.** Decide which numbered sections of `index.html` you'll touch. Prefer the smallest change that works.
4. **Edit in the right place.**
   - New colours or sizes go in design tokens.
   - New behaviour goes in the right JS section: actions → render → events.
5. **Keep data safe.** If you change the task shape, bump the storage key version and migrate old data. Never silently wipe users' tasks.
6. **Test in a browser.** Go through this checklist:
   - [ ] Add a task with Enter and with the Add button, in each section, with and without a tag
   - [ ] Empty input shows "Type a task first." and adds nothing
   - [ ] Tick and untick: done tasks strike through and move to the bottom
   - [ ] Edit via the pencil and via double-click; Enter saves, Escape cancels, clicking away saves
   - [ ] Delete; focus moves to the next task or the "Add task" button
   - [ ] Filters All / To do / Done, and the counts and empty states update
   - [ ] "X tasks left" is correct
   - [ ] Refresh the page: tasks and filter are still there
   - [ ] Keyboard only (Tab / Enter / Space / Escape) works with visible focus
   - [ ] Phone width (375px): stacked layout, no sideways scroll, easy tapping
   - [ ] Desktop (≥ 900px): three columns side by side
   - [ ] No errors in the browser console
7. **Check the design** against section 3: one accent colour, contrast, spacing and no new visual noise.
8. **Update the docs.** If features change, update `README.md`. If structure, rules or the data model change, update this file.
9. **Explain simply.** Summarise what changed and why, in plain language, for a beginner.
10. **Git.** Only commit or push when the user asks. Use clear commit messages (e.g. `Add due-date field to tasks`).

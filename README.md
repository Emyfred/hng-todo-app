# Planner: Today, Tomorrow, Later

A calm, minimal todo list web app built for the **HNG Internship, Stage 1** task.

**Live demo:** [hng-todo-app-psi.vercel.app](https://hng-todo-app-psi.vercel.app/)

Tasks are organised into three sections, **Today**, **Tomorrow** and **Later**. They sit side by side on desktop and stack on mobile. Everything is saved in your browser, so your list is still there after you refresh.

## Features

- **Add tasks.** Click **+ Add task** under any section, type, and press **Enter** or click **Add**. You can:
  - choose which section the task goes into
  - optionally pick one tag: **Work**, **Personal**, **Learning** or **Errands**
- **Mark as done.** Tick the checkbox. Done tasks fade, get a strikethrough and move to the bottom of their section. Untick to undo.
- **Edit.** Click the pencil icon, or double-click the task text. Press **Enter** to save or **Escape** to cancel.
- **Delete.** Click the bin icon.
- **Filter.** Switch between **All**, **To do** and **Done**. Each filter shows how many tasks it has.
- **Tasks left.** The header shows how many unfinished tasks remain.
- **Section counts.** Each section header shows how many tasks it is currently showing.
- **Saved automatically.** Tasks (and your chosen filter) are stored in `localStorage`.
- **Friendly empty states**, e.g. "Nothing planned for tomorrow yet."
- **Mobile friendly.** Large tap targets, stacked layout and no sideways scrolling.
- **Accessible.**
  - All text meets WCAG AA colour contrast.
  - Everything works with the keyboard, with visible focus rings.
  - Screen-reader labels and announcements are included.
  - Animations are turned off for people who prefer reduced motion.

## Design

- Soft light-grey background with white task cards and a very subtle border (no heavy shadows).
- One accent colour, a muted green (`#3A7456`), used **only** for actions: the add buttons and ticked checkboxes.
- Neutral greys and near-black text everywhere else.
- Font: [Plus Jakarta Sans](https://fonts.google.com/specimen/Plus+Jakarta+Sans) from Google Fonts.

## Tech

- Plain **HTML, CSS and JavaScript** in a single `index.html` file.
- No frameworks, no build tools and no install step.

## Run it locally

Double-click `index.html` to open it in your browser. That's it.

## Deploy

The app is a static site. On [Vercel](https://vercel.com), import the GitHub repository and click **Deploy**. No settings need changing.

## Notes

- "Today", "Tomorrow" and "Later" are simple buckets. Tasks don't move between them automatically when the date changes.
- Data is stored per browser and per device. Clearing your browser data clears your tasks.

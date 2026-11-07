# Approved V1 plan

Approved on 2026-11-07. This document records the decisions from the product interview. The implementation has not started.

## Purpose

Build a Windows GUI memo app that stays available in the background and keeps short-term and long-term items in one manually ordered list. The user reviews the list deliberately. Long-term items do not resurface automatically.

An item can represent a task or hold reference text, a URL, or a short code snippet. There is one item type. Completing an item means it is no longer needed in the active list.

## Main window and text

- Use a normal resizable window that remembers its size and position.
- Show one scrolling list of active items, with each item's full text visible.
- Use multiline plain text. Preserve line breaks and indentation.
- Use Courier New for item text and the standard Windows font for buttons and menus.
- Use one restrained light theme with a neutral background, subtle borders, and compact spacing.
- Treat URLs and code as plain text. V1 requires no special link, preview, or code-rendering behavior.
- Keep completion history controls out of the main window.

## Adding, editing, and ordering

- Put a new-item text box at the top of the window.
- Enter inserts a newline. Ctrl+Enter or the Add button saves a new item.
- Put new items at the top of the active list.
- Double-click an existing item to edit its text. Provide a clear save action for the edit.
- Provide a small drag handle for rearranging items.
- Persist the resulting order across hiding, quitting, and restarting.
- Automatically preserve unfinished editor text across hiding the window and restarting the app. A draft is not a committed new item.

## Completion and restoration

- Prompt for confirmation every time the user marks an item complete.
- Cancelling leaves the item active and unchanged.
- Confirming completion removes the item from the active list and retains its data.
- A `Completed items` tray-menu entry opens a separate view.
- Show completed items with the most recently completed first.
- Limit the completed view to reading and restoring items.
- Restore puts an item at the top of the active list. The user can edit it after restoration.
- Retain completed items without automatic expiry. V1 requires no permanent-delete interface.

## Windows lifecycle

- Start automatically at Windows sign-in, hidden in the notification area. This is per-user sign-in startup, not a service before login.
- Taskbar and tray entry points show and focus the main window.
- Closing or minimizing hides the window while the app continues running.
- An explicit `Quit` tray-menu command exits the app.
- Normal Windows shutdown must also be able to exit the process.
- Use a single running instance. A subsequent launch activates the existing instance.
- Provide a shortcut suitable for taskbar pinning.
- Keep the tray menu small: show the main window, open completed items, and quit.

## Technology and persistence

Selected stack:

- C# with WPF on .NET 10.
- `System.Windows.Forms.NotifyIcon` for the tray component.
- SQLite through `Microsoft.Data.Sqlite` for local data.

Use the current user's local application-data directory for persistent data, separate from the installer and repository. Store item text, active order, completion state and time, unfinished editor text, and window preferences.

Use SQLite transactions for changes that must commit together, including ordering and item-state changes. A server, ORM, and database administration UI are unnecessary for V1. Keep persistence code small and version the schema so later releases can preserve existing data.

No backups are required for V1. SQLite transactions address normal save consistency; they do not introduce a backup feature.

## Distribution and resource use

- Deliver a per-user Windows installer.
- Include the required .NET runtime so a separate runtime installation is unnecessary.
- Configure sign-in startup and provide the application shortcut.
- Determine the Windows architecture from the target environment when preparing the first build. Architecture and exact installer tooling were not product decisions in the interview.
- Prefer quick opening, responsive interaction, and low idle activity over minimum installer size.
- Measure startup time, reopening time, idle CPU, and idle memory on the target Windows machine.
- Numeric resource limits remain unset. Do not invent benchmark results or claim that a framework guarantees a particular footprint.

## Excluded from V1

- Search and filtering.
- Deadlines, reminders, and automatic resurfacing.
- Projects, folders, categories, urgency, priorities, and task-size fields.
- Separate short-term and long-term lists.
- Rich text, Markdown rendering, syntax highlighting, and URL previews.
- Global capture shortcuts.
- Dark mode, theme settings, and a theme picker.
- Synchronization, cloud storage, and backups.
- An archive or completed-items control in the main window.

## Acceptance checks

1. Add multiline text containing a URL and an indented snippet. Confirm that the saved text remains intact after restart.
2. Edit an existing item. Confirm that its saved text persists and its position does not change unexpectedly.
3. Rearrange several items. Confirm that the exact order survives hiding, quitting, and restarting.
4. Leave unfinished new-item or edit text. Confirm that hiding and restarting preserve it without creating a duplicate item.
5. Cancel a completion confirmation. Confirm that the item remains active and unchanged.
6. Confirm completion. Confirm that the item leaves the active list and appears in the completed view.
7. Complete several items. Confirm newest-completed-first ordering. Restore an item and confirm that it returns to the top of the active list.
8. Close and minimize the main window. Confirm that each action hides it while the tray remains usable. Confirm that Quit exits.
9. Use the tray and pinned taskbar shortcut to reopen the app. Confirm that they focus one instance and preserve window geometry.
10. Install without relying on a separately installed .NET runtime. Sign out and back in, or use an equivalent real sign-in test, and confirm that the app starts hidden.
11. Measure and record startup, reopening, idle CPU, and idle memory. Review whether the result meets the user's lightweight expectation.

Implementation may resolve routine control, layout, schema, and packaging details within this scope. Revisit product decisions only when a concrete blocker or new user request requires it.

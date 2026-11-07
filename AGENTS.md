# Repository instructions

## Read before implementation

1. Read `docs/V1_PLAN.md`, the canonical approved product specification.
2. Read `docs/HANDOFF.md` for the current state, build order, and verification checklist.
3. Inspect the checkout and toolchain before assuming that the handoff is still current.

The user completed the design interview and approved C# + WPF on .NET 10, SQLite, and a self-contained per-user installer. Continue from those decisions. Do not repeat the interview or replace the stack without a concrete blocker or a new user instruction.

## Scope and implementation

- Target Windows. Keep data local to the current user.
- Keep one active list, plain text, manual order, and Courier New item text.
- Hide on close and minimize. Exit only through the explicit Quit action or normal system shutdown.
- Confirm each completion. Retain completed items and expose restoration through the tray menu, outside the main window.
- Preserve unfinished editor text across hiding and restarting.
- Do not add search, deadlines, reminders, projects, categories, urgency, task sizes, Markdown rendering, syntax highlighting, global capture shortcuts, theme settings, sync, or backups to V1.
- Prefer a small application structure and direct `Microsoft.Data.Sqlite` access. An ORM, dependency-injection framework, or third-party UI toolkit is not required.
- Avoid background polling for an event-driven list utility. Measure actual startup and idle resource use before making performance claims.
- Keep saved data outside the installation and source directories. Never commit personal items, local databases, installer output, or credentials.

## Verification and handoff

- Run builds and meaningful tests appropriate to the changes. Record the exact commands and outcomes.
- Validate tray, taskbar, focus, close/minimize, sign-in startup, and installation behavior on Windows. A successful build alone does not validate these flows.
- Test persistence with an isolated data directory or temporary database, not personal data.
- Do not mark a milestone complete without verifying its behavior. State unavailable checks clearly.
- Update `docs/HANDOFF.md` with completed work, actual commands, remaining work, and blockers before ending an implementation session.

At the initial documentation handoff, the .NET CLI was unavailable on PATH. Recheck before treating this as a current blocker. No application or tests exist at this initial stage.

# xc-memo

A Windows-only memo app for keeping short-term and long-term items in one manually ordered list. Items contain plain text and remain active until completed.

**Status:** V1 planning is complete. This repository contains the approved specification and implementation handoff. Application code, builds, tests, and an installer do not exist yet.

## Start here

- [Approved V1 plan](docs/V1_PLAN.md): product behavior, scope, technical decisions, and acceptance checks.
- [Implementation handoff](docs/HANDOFF.md): current state, build order, development setup, and validation work.
- [Agent instructions](AGENTS.md): repository guidance for the next Codex session.

## Selected stack

- C# and WPF on .NET 10.
- SQLite through `Microsoft.Data.Sqlite` for local persistence.
- Windows notification-area integration through `System.Windows.Forms.NotifyIcon`.
- A per-user installer that includes the required runtime and starts the app hidden at Windows sign-in.

The main window provides adding, editing, rearranging, and completing items. Closing or minimizing hides the window while the app remains in the tray. Completed items are accessible for restoration through the tray menu.

V1 excludes search, deadlines, reminders, projects, categories, priorities, rich text, synchronization, and backups. Read the plan before changing scope.

## Continue in Codex

Open this repository on Windows and use this prompt:

> Read AGENTS.md, docs/V1_PLAN.md, and docs/HANDOFF.md. Implement the approved V1 in the documented order. Preserve the agreed scope, validate the Windows window/tray lifecycle and persistence behavior, and update the handoff with actual progress and verification results.

There is no application command to run yet. The implementation session must first provision or locate the .NET 10 SDK and scaffold the solution.

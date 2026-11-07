# Implementation handoff

## Current state

- The product interview is complete and the user approved `V1_PLAN.md`.
- The selected stack is C# + WPF on .NET 10, with SQLite.
- This is a documentation-only repository. There is no application scaffold, solution, test project, CI workflow, or installer yet.
- During handoff preparation on 2026-11-07, `dotnet` was not available on PATH. Recheck the next session's environment.
- No application build, runtime test, or installation test has run. Documentation review is not application verification.
- No architecture target, installer tool, package versions, or numeric performance limits are fixed yet.

Documentation handoff checks passed: all three local Markdown links resolved, `git diff --cached --check` reported no whitespace errors, and ignore rules covered build output, SQLite data/sidecars, and local secrets. These checks do not validate application behavior.

## First session

Read `../AGENTS.md` and `V1_PLAN.md`. Inspect the current checkout and Windows toolchain. Then begin implementation rather than repeating product discovery.

Use the .NET 10 SDK for development. The SDK is a development prerequisite; the delivered app will include its runtime. Visual Studio is optional if the CLI and suitable editing tools are available.

Suggested initial layout after scaffolding:

```text
src/XcMemo/              WPF application
tests/                  Meaningful persistence and behavior tests
installer/              Packaging scripts and installer configuration
docs/V1_PLAN.md          Approved product specification
docs/HANDOFF.md          Current progress and verification notes
```

Keep the architecture small: views and view models for the interface, a persistence layer for SQLite, and Windows integration for tray/lifecycle behavior. Do not introduce additional frameworks without a concrete need.

Start by locating or provisioning the SDK and checking `dotnet --info`. After scaffolding, record the actual solution path and restore, build, test, run, and publish commands here. Do not present nonexistent project commands as working setup instructions.

## Build order

### 1. Window and background lifecycle

- Scaffold the WPF application.
- Add tray show/completed/quit actions and a single-instance activation path.
- Implement hide on close/minimize, explicit quit, and shutdown handling.
- Persist window size and position, handling unavailable monitor positions sensibly.
- Validate the window/tray lifecycle on Windows before building more features on it.

### 2. Items and persistence

- Add the SQLite schema and version handling with direct `Microsoft.Data.Sqlite` access.
- Store data under the current user's local application-data directory.
- Implement adding, editing, new-items-first order, and automatic draft preservation.
- Verify multiline text, indentation, Unicode, and restart persistence with isolated test data.
- Preserve unfinished edit text without silently committing or losing it when the window hides.

### 3. Ordering and completion

- Implement drag handles and persisted manual ordering.
- Confirm each completion and retain completed records.
- Add the separate completed view with newest-completed-first order and Restore.
- Verify cancelling, completing, restoring to the top, and repeated restarts.

### 4. Visual polish

- Apply a restrained light appearance, using the built-in WPF Fluent theme where suitable.
- Use Courier New for item text and the Windows UI font for controls.
- Check multiline items, long lines, scrolling, drag targets, resizing, display scaling, keyboard focus, and empty states.
- Keep the main window free of archive/history controls and excluded features.

### 5. Installation and validation

- Choose a Windows architecture and per-user installer tool based on the target environment.
- Publish with the required runtime included.
- Add shortcuts and startup registration that launches hidden.
- Validate the installed build, pinned taskbar activation, and actual sign-in startup.
- Confirm that a normal upgrade preserves the local database and drafts. Do not make an unrequested destructive data-removal choice during uninstall.
- Measure startup, reopening, idle CPU, idle memory, and distribution size. Record the measurement method and machine details.

## Verification approach

Automated tests should cover meaningful persistence risks: text round trips, transactional ordering changes, completion/restoration, schema upgrades once applicable, and draft recovery. Use temporary data directories or databases. Avoid tests that only mirror a property assignment or UI implementation detail.

Windows interaction checks are also required. Build success and unit tests cannot establish taskbar focus, tray behavior, startup registration, or installer behavior. Record each manual check as passed, failed, or not run, with a reason.

The full acceptance checklist is in `V1_PLAN.md`. No acceptance item is complete at this documentation-only stage.

## Update before handing off again

Record:

- Implemented milestones and any remaining behavior gaps.
- Actual development and packaging commands.
- Build/test results and Windows interaction checks.
- Data location and schema version once selected.
- Installer tooling, Windows architecture, and tested Windows version.
- Resource measurements, unresolved issues, and the next concrete task.

Keep the approved plan stable unless the user changes scope. Update this file as implementation progresses.

---
name: safe-ios-disk-cleanup
description: Inspect disk use from iOS development on macOS. Remove verified unused build output and temporary test simulators when the user requests cleanup.
disable-model-invocation: true
---

# Safe iOS disk cleanup

Identify generated iOS development data that can be removed without losing source work, active task data, or user data. An inspection request authorizes inspection only. Delete data when cleanup is requested and the checks below pass.

Use ASD-STE100 simplified technical English for findings: short sentences, active voice, and consistent technical terms.

## 1. Identify disk use

Record free space on the volume that contains the candidate data. On macOS, `df -h /System/Volumes/Data` can measure the Data volume. Treat folder sizes and free-space changes as estimates. APFS snapshots, shared blocks, and concurrent writes can affect them.

Inspect the requested roots first. Relevant locations can include:

- Xcode DerivedData, usually `~/Library/Developer/Xcode/DerivedData`.
- XcodeBuildMCP workspaces, for example `~/Library/Developer/XcodeBuildMCP/workspaces`.
- Workspaces or worktrees used by Codex or other coding agents, e.g. `~/.codex/worktrees`. Inspect only their generated build output; retain the source worktrees.
- Build output paths found in project or build tool settings.
- Temporary folders identified by build or test logs.
- Simulator devices listed by `xcrun simctl list devices --json`.

These paths are examples, not required locations. Confirm that each relevant path exists. Use tool settings or build logs to find custom locations. Inspect agent workspaces only when they are relevant to the request. A workspace can contain source, build output, and task evidence together.

Measure each exact candidate. Inspect its contents, dates, repository markers, and links to tasks. A folder name is a clue, not proof that its contents are generated or unused.

This step is complete when each candidate has a known path, estimated size, content type, and activity status.

## 2. Protect active work and retained data

Check build, compiler, simulator, and app processes. Inspect process arguments and open files for the candidate path. A build coordinator's presence alone does not show which workspace it uses. Missing access to process information is not proof that a path is unused.

When data belongs to a task, fetch its status from an issue tracker available in context, e.g. Linear or GitHub Issues. If no tracker is available, use other reliable task evidence. Treat open, in-review, blocked, or unknown tasks as active. Protect tasks updated within the past 24 hours. If the user requires task completion, require evidence of completion before deletion.

Protect all of the following:

- Source repositories and worktrees, including uncommitted and untracked files.
- Data used by a running build, app, or active task.
- Task logs, screenshots, and test results needed as evidence.
- Agent session or history data, archives, credentials, and other user data.
- Booted simulators, retained test devices, and installed simulator runtimes.

File modification time alone does not establish last use. If activity or ownership remains uncertain, retain the candidate and explain the missing evidence.

## 3. Select eligible data

### Generated build output

All protection checks must pass. Confirm that the selected contents are generated and can be rebuilt. In a mixed folder, select only the verified generated child.

Then require at least one of these conditions:

- The output is verified unused for at least 24 hours and exceeds 5 GiB.
- It is a redundant older copy for the same task, and the newest copy is retained.
- It is a temporary test-products folder verified unused for at least 24 hours.
- The user requests the exact generated output, and any required task-completion condition is met.

Dependency checkout folders can contain local edits. Classify their contents before considering deletion. A build-output label does not make an entire dependency directory disposable.

### Temporary test simulators

Confirm that the exact device was created as a temporary duplicate for testing. Require every condition below:

- Its task is complete, or reliable evidence shows no use for more than 2 days.
- It is not booted or used by a process or active task.
- It is not the newest or retained device for the task.
- It contains no user data or task evidence that must be retained.

Use `xcrun simctl delete <validated-UDID>` for an eligible device. Keep installed runtimes and ordinary development devices.

The age and size limits above are conservative defaults. Apply stricter project or user retention rules when present.

## 4. Delete exact candidates and verify

List each selected absolute path or device ID with its size, eligibility reason, and protection checks. Keep this list within the user's requested scope.

For filesystem candidates, validate the resolved path against the inspected roots. Reject symlinks, roots themselves, and unexpected path components. Recheck path identity, activity, task status, and retention requirements immediately before deletion.

Delete only validated entries. Avoid wildcard deletion and broad cache or simulator cleanup commands. If a check fails or becomes uncertain, skip that entry. Record successful removals and errors separately.

Measure free space again. Report the before and after values separately from estimated folder sizes. State what was removed, what was retained, and any missing checks. For an explicit inspection request, report candidates without deleting them.

If the user requests quiet scheduled cleanup, use their reporting thresholds. Otherwise, give a short result even when nothing is removed.

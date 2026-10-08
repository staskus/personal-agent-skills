---
name: snapshot-driven-ui-iteration
description: Implement or refine UI changes by rendering snapshots, comparing them with design references or stated goals, and correcting visible differences.
---

# Snapshot-driven UI iteration

Use rendered images to check UI changes during implementation. Repeat the cycle until the selected states meet the design goal or a specific limit prevents further verification.

Use ASD-STE100 simplified technical English for findings: short sentences, active voice, and consistent technical terms.

## 1. Define the visual goal

Read the supplied design images and requirements. Identify the view, visible states, and target viewport sizes. If no image is available, turn the user's description into concrete checks and state any layout assumptions.

Select cases that can expose errors in the changed UI. These can include long text, empty content, errors, narrow widths, dark mode, or larger text. Use cases that matter to the change.

This step is complete when each selected state has a target appearance and a known viewport.

## 2. Use the project's rendering tools

Inspect repository instructions, test targets, snapshot tools, and available runtimes. Reuse existing snapshot tests or preview fixtures when they cover the target view.

Choose the smallest target that can render the real view with its required dependencies. Use fixed data and controlled state. Keep time, network responses, and animation progress stable when they affect the image.

For SwiftUI or UIKit projects that need a snapshot fixture, read [Swift snapshot setup](references/swift-snapshot-testing.md).

If a new dependency or temporary test is needed, follow project rules. Record the existing file changes before editing. Keep a list of the files and settings added for this task, so cleanup can preserve other work.

Keep captures in a task-specific location. Use filenames that identify the state, viewport, and iteration.

## 3. Render, inspect, and correct

For each implementation change:

1. Render the selected states at their target viewport sizes.
2. Check the run result and locate the generated images.
3. Open each image with an available image viewer. Compare it with the reference or stated goal.
4. Identify specific differences in layout, spacing, text wrapping, colors, or visible content.
5. Correct the cause in the view or fixture, then render the affected states again.

Match the reference's viewport, content, and display settings before judging differences. Use the project's design tokens when it provides them.

Keep viewport captures at the intended size. For scrollable content, capture additional scroll positions or a separate full-content image. A taller image does not prove that content fits in the target viewport.

If rendering fails, separate build or environment failures from snapshot comparison failures. Correct a known setup problem before retrying. If the required runtime or tool is unavailable, report the missing check.

This step is complete when every selected state has a reviewed capture and each important difference is corrected or reported.

## 4. Keep evidence claims precise

A recorded image shows what the fixture rendered. A comparison test checks that image against a stored baseline. Accept a new baseline only after visual review; recording an image is not a passing comparison.

State which view, states, viewport sizes, and runtime were checked. Link to useful captures when they are available.

Static snapshots do not establish navigation, touch behavior, keyboard behavior, or live data flow. If those affect the change, verify them through the app's interaction tools and report the result separately.

## 5. Remove temporary setup

Remove only the temporary files and edits listed for this task. Preserve existing tests, baselines, dependencies, lockfile changes, and user edits. Restore individual edits instead of resetting whole files or deleting shared directories.

Keep permanent snapshot coverage when the task or project requires it. Follow the user's instructions for commits and publication.

Inspect the final diff. Confirm that the intended UI changes remain and each temporary addition is removed or deliberately retained.

Finish with a short report: what changed, which captures were reviewed, the visible result, and any remaining verification gaps.

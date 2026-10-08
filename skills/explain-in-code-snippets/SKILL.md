---
name: explain-in-code-snippets
description: Explain a code change with annotated code snippets when the user asks for an explanation in code snippets. Cover its purpose, runtime behavior, evidence, and limits.
---

# Explain in Code Snippets

Help the reader understand what the final code does and why each important change is necessary. Explain the implementation that exists.

Use ASD-STE100 simplified technical English: short sentences, active voice, direct instructions, and one meaning for each term. Keep technical names when they identify code or API behavior. Use the same term for the same concept.

## 1. Identify the change

Use the scope from the request: working-tree changes, staged changes, a commit, a branch comparison, a pull request, or named files. If the scope is unclear, use the conversation and repository state to identify it. Ask only when different choices would change the explanation.

Inspect the change, its callers, and relevant failure or fallback paths. State the comparison basis when it affects the result. Distinguish changed code from unchanged context.

This step is complete when you can identify each important behavior change and the code path that uses it.

## 2. Explain the overall behavior

Start with a short paragraph that states:

- What behavior changes.
- Why the change was made and what outcome it should produce.
- How the code supports that outcome.
- The main uncertainty or limit, if one remains.

Use a short before/after sequence when order, state, or ownership explains the change:

```text
Before: event A -> state changes -> operation completes
After:  event A -> operation completes -> state changes
```

Separate the stated reason for the change from a reason you infer from the code.

## 3. Explain each important change

Explain production code first, then tests and supporting code. Cover each changed section that affects behavior, an interface, or validation. Group renames, mock setup, and other repeated changes by purpose.

For each change:

1. Use a short heading that names its purpose.
2. Show the smallest source excerpt that preserves the relevant logic. Use before/after snippets when they help explain the difference.
3. Add short comments to explain the code. Label comments that you add as explanation annotations, so the reader can distinguish them from source comments.
4. Explain why the change is necessary and what happens when the code runs.
5. Explain failure, cancellation, repeated actions, or fallback behavior when the change affects them.

Keep code excerpts faithful to the source. Identify omitted code as an excerpt. Show a proposed alternative separately if the user requests one.

Link to local files with exact line numbers when they are available.

## 4. Explain what the tests prove

For each important test, state its initial state, the action it simulates, the result it checks, and the regression it can detect. Group tests that check the same contract.

State whether a test was run or only inspected. Report relevant results from available evidence.

Keep the claim within the test's scope. A helper test can prove the helper's behavior. Evidence from the actual UI is required to show that the UI calls the helper at the correct time. A test of an intermediate state does not establish that a crash was reproduced or fixed.

Explain fixed mock values when the tested code uses them. Identify other required mock values as interface placeholders.

## 5. Check claims and coverage

Use these evidence levels when the distinction matters:

- **Observed:** shown directly in code, logs, test results, or recorded manual checks.
- **Inferred:** an explanation that fits the evidence and the inspected code path.
- **Unknown:** behavior or impact that the available evidence does not establish.

Check the API contract before claiming that a callback waits for layout, keyboard, rendering, or cleanup work. If the contract is unavailable, state that ordering is unknown.

If the user asks for a success percentage, label it as a subjective engineering estimate. Explain the evidence for and against it. Distinguish confidence in local code behavior from confidence that the change fixes the reported issue.

Correct earlier claims when new evidence contradicts them.

Before you finish, check that each important changed section has an explanation and that each runtime or validation claim has a stated basis. End with a short list or table of the affected user flows, their resulting behavior, and any remaining validation gaps. Include only details that add to the opening explanation.

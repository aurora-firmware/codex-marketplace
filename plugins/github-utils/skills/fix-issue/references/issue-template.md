# Issue body template

Reshape the issue's existing content into this structure. Preserve all
reporter-provided details, including reproduction steps, logs, and suggested
fixes. If a detail does not map neatly to a heading, retain it under the closest
heading rather than dropping it.

```markdown
## Description

<One or two sentences explaining what is wrong and why it matters.>

## Expected Behavior

<What should happen.>

## Actual Behavior

<What happens instead. Note related but out-of-scope problems explicitly.>

## Steps to Reproduce

<Numbered steps, or the reporter's original reproduction details.>

## Logs / Evidence

<Command output, stack traces, diffs, or other evidence.>

## Diagnosis

- **Root cause:** <from the fix contract>
- **Isolated fault:** `<file:line>` — <what is wrong and why>
- **Planned fix:** <the minimal change>
- **Planned verification:** <exact commands or manual steps>

## Status

<One line, updated as the workflow progresses:>
- `Diagnosis in progress.`
- `Fix in progress on branch \`fix/<N>-<short-description>\`.`
- `Fixed on branch \`fix/<N>-<short-description>\` — see PR #<M>.`
- `Merged in #<M>.`
```

## Diagnosis

Write Diagnosis only after completing diagnosis. When the fault is a
hypothesis because reproduction was not possible, write `**Root cause
(hypothesis):**` rather than presenting it as confirmed.

## Status

After the first approved issue edit, keep the report stable. Later, update
only Diagnosis when evidence changes and the single Status line as the work
progresses.

---
name: fix-issue
description: >-
  End-to-end workflow for turning a specific open GitHub issue into a verified
  fix: diagnose the root cause before changing code, record the diagnosis in a
  structured issue body, branch and implement the minimal fix, open a linked
  PR, then use pr-review and receive-pr-review until clean or blocked. Use
  when the user asks to fix, resolve, pick up, or ship one specific GitHub
  issue. Not for selecting an issue to work on, feature requests, or
  open-ended design work. Defers to gh-cli for exact GitHub CLI syntax.
---

# fix-issue

Take one specific open GitHub issue from report to a reviewed fix. The issue
holds the diagnosis, and the branch and pull request hold the implementation;
do not create a separate ticketing or documentation trail.

This skill defines the workflow, issue-body structure, and confirmation points.
For exact `gh` syntax, read the `gh-cli` skill. For the PR review loop, use
`pr-review` and `receive-pr-review`.

First check scope. Use this workflow for a bug, small well-scoped defect, or
clearly specified small task. If the issue is a feature request or needs design
exploration, explain that and route it through the project's planning or spec
process instead.

## 1. Fetch the issue

Accept a URL, `owner/repo#N`, or a bare issue number for the current repository.
Use `gh issue view` to retrieve its number, title, body, labels, state, and URL.

If it is closed or already has a linked PR, report that and ask whether to
proceed rather than assuming the work is fresh.

## 2. Diagnose before changing code

Do not edit implementation code during diagnosis.

If the project provides a diagnosis or debugging skill, use it and adapt its
output to the following fix contract. Otherwise:

1. Reproduce or confirm the defect. If a live reproduction is impractical,
   trace the implicated code path and state that this is what you did.
2. Isolate the fault to the responsible file, function, or line.
3. Identify the root cause rather than merely its symptom. Label an
   evidence-backed but unconfirmed conclusion as a hypothesis.
4. Form a fix contract containing root cause, isolated fault (`file:line`),
   minimal planned fix, and planned verification.

Keep the work scoped to that fault. Record adjacent problems as separate issue
candidates instead of folding them into the change.

## 3. Structure the issue body

Read [the issue template](references/issue-template.md). Reorganize the
existing report into that structure without dropping reporter-provided details,
then add the diagnosis and an appropriate status line.

Show the user the complete draft first. Updating an issue is GitHub-visible, so
do not make that edit without explicit confirmation in the current conversation.
If the user requests changes, revise and show the draft again. After approval,
use `gh-cli` guidance to update the issue.

## 4. Create the branch

Check the repository's documented branch model in `AGENTS.md`, `CLAUDE.md`,
`CONTRIBUTING.md`, or an applicable git-conventions skill. Follow it when it
exists. Otherwise, branch from the default branch as
`fix/<issue-number>-<short-description>`.

Ensure the working tree is clean before branching.

## 5. Implement and verify

Implement only the minimal fix described by the fix contract. Then run the
project's documented commands, preferring the specific regression test before a
broader suite. When automation cannot fully verify the behavior, record the
precise manual steps and outcome.

## 6. Commit

Use the repository's commit convention. If none exists, use
`<type>(<scope>): <description>`: imperative, lowercase, and without a trailing
period. The issue number need not be repeated because it is already in the
branch name.

## 7. Push and open the PR

Draft the PR title and body, then show them to the user and obtain explicit
confirmation before pushing or opening the PR. The PR body should:

- Summarize the fix in a few bullets.
- Link to the issue for its full diagnosis.
- List the verification performed.
- Include `Fixes #<N>` unless the repository closes issues manually.

After confirmation, push the branch and open the PR against the integration
branch identified earlier. Update only the issue's status line to include the
PR after the user has also confirmed that visible edit.

## 8. Review and close the loop

Iterate rather than treating review as one pass:

1. Run `pr-review` for the PR.
2. If it has no open findings, report the clean result and stop.
3. Otherwise use `receive-pr-review` to triage each finding.
4. Address valid, small findings; record valid larger work, questions, and
   reasoned disagreements as that skill directs. Commit and push approved
   fixes.
5. Re-run `pr-review`.

Stop when review is clean or all remaining items require user input. If three
rounds do not converge, stop and report what remains and why. Do not merge on
the user's behalf.

## Quality criteria

- Root cause is identified and recorded before implementation changes.
- Original reporter content is preserved when the issue is structured.
- No GitHub-visible write happens without user confirmation in the current
  conversation.
- The diff remains limited to the isolated fault.
- Verification records exact commands or manual steps and their outcome.

## Common pitfalls

- Do not force feature requests or design work through this fix workflow.
- Do not patch a visible symptom before isolating its root cause.
- Do not assume the default branch is the integration branch.
- Do not silently edit the issue, push a branch, or create a PR.
- Do not stop after a single review pass when findings remain.

## Reference

- [Issue-body template](references/issue-template.md) — required structure for
  preserving the report while recording diagnosis and status.

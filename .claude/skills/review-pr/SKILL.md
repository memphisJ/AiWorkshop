---
name: review-pr
description: Review a GitHub pull request and post structured, actionable feedback. Use when the user asks to review a PR, look over a pull request, give feedback on a PR, or check a PR before merging (e.g. "review PR 42", "can you look over this pull request", "review the open PR on this branch").
---

# Review a Pull Request

Review a GitHub pull request thoroughly and leave clear, prioritized, and
respectful feedback. The goal is a review that helps the author ship a correct,
maintainable change — not a nitpick dump.

## 1. Identify the PR

Work out which PR to review:

- If the user gives a number or URL, use that.
- Otherwise, find the PR associated with the current branch.

Gather the essentials before reading any code:

- **Title and description** — what is this change *supposed* to do?
- **Linked issue(s)** — the requirement the change must satisfy.
- **Changed files, diff, and commit history.**
- **CI status** — note any failing checks; don't re-review what CI already caught.

If the stated intent and the diff disagree, that gap is itself a finding.

## 2. Read the diff with intent

Go file by file. For each change, ask:

- **Correctness** — Does it do what the description claims? Edge cases, null/empty
  inputs, off-by-one, error handling, race conditions.
- **Tests** — Is the new behavior covered? Do existing tests still make sense?
  A change to behavior with no test change is suspicious.
- **Security** — Untrusted input, injection, secrets committed, authz/authn gaps,
  unsafe deserialization. Flag anything that touches a trust boundary.
- **Readability & maintainability** — Naming, dead code, duplicated logic,
  functions that do too much. Does it match the surrounding style?
- **Scope** — Unrelated changes mixed in? Debug prints or commented-out code left
  behind?

Read enough surrounding context to judge each change — don't review the diff in
isolation when the rest of the file changes the answer.

## 3. Prioritize findings

Sort every finding into one of three buckets and label it:

- **🔴 Blocking** — bugs, security issues, broken behavior, missing critical tests.
  These should be fixed before merge.
- **🟡 Should-fix** — real problems that aren't release-blocking: weak naming,
  missing edge-case handling, gaps in test coverage.
- **🟢 Nit / optional** — style preferences and minor suggestions. Mark these
  clearly so the author can skip them without guilt.

If there are no blocking issues, say so plainly. Don't manufacture problems to
look thorough, and don't pad the review with restated obvious facts.

## 4. Write the review

Each comment should:

- Point at a specific file and line.
- Explain **why** it matters, not just what to change.
- Suggest a concrete fix where you can (a code snippet beats a vague request).
- Stay respectful — critique the code, never the author.

End with a short summary that gives the overall verdict (approve / needs changes /
comment) and lists the blocking items, if any.

## 5. Deliver

Default to summarizing the review in chat so the user can decide what to post.
Only post comments directly to GitHub when the user explicitly asks you to.
When you do post, use inline comments anchored to the relevant lines for specific
findings, and a single top-level comment for the overall summary.

Be frugal with GitHub comments: consolidate related nits into one comment rather
than spraying the diff.

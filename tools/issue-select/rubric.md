# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| viable-repo | repo-facts: last push date, latest release date, default-branch commits, and contribution policy | The repository is not archived, has a commit or push within the last 180 days (or a release within the last 12 months), and its contribution policy does not explicitly prohibit outside PRs, ban AI assistance, or declare an active pull request freeze. | required |
| not-claimed | repo-facts: assignees, ALL linked PRs (open, closed, merged), and comment thread | The issue is unassigned, has no currently open PR addressing it, no linked PR that was closed unmerged after a claimed attempt, and no maintainer comment reserving the issue for someone else or indicating it's already implemented. | required |
| clear-actionable-ask | issue body, title, labels, author | The issue names a concrete, nameable problem or deliverable (a bug, a missing feature, a described visual/UX defect) — this can be as short as one sentence, especially when opened by a maintainer/collaborator or labeled 'good first issue'/'bug'/'help wanted'. Fails only on open-ended asks with no concrete deliverable, where the fix's shape still needs to be designed. | required |

## Verdict rule

- Accept if every `required` check passes (`P`).
- Reject if any `required` check fails (`F`) or is `unclear` (`?`).
- An `unclear` (`?`) on any required check counts as a fail (`F`).
# Issue tracker: GitHub

Tickets and specs live in GitHub Issues in JoranBergfeld/procurement-automation.
Use the gh CLI for tracker operations.

## Conventions

- Confirm the target with `gh repo view --json nameWithOwner` before creating issues.
- Create: `gh issue create --title "..." --body "..."`.
  For multiline bodies in PowerShell, use a here-string with `--body-file -`.
- Read: `gh issue view <number> --json number,title,body,labels,comments`.
- List: `gh issue list --state open --json number,title,body,labels,comments`.
  Add label and state filters as needed.
- Comment: `gh issue comment <number> --body "..."`.
- Labels: `gh issue edit <number> --add-label "..."` or `--remove-label "..."`.
- Close: post the explanation as a comment, then run `gh issue close <number>`.

## Parent tickets and blockers

Use GitHub native sub-issues and issue dependencies when supported.
Check `gh issue create --help` and `gh issue edit --help` for available flags.
For API operations, use numeric database IDs, not issue numbers or node IDs.

- Sub-issue: POST `repos/{owner}/{repo}/issues/<parent>/sub_issues`
  with `sub_issue_id=<child database ID>`.
- Blocker: POST `repos/{owner}/{repo}/issues/<child>/dependencies/blocked_by`
  with `issue_id=<blocker database ID>`.
- Obtain an ID: `gh api repos/{owner}/{repo}/issues/<number> --jq .id`.
- Count open blockers: `gh api repos/{owner}/{repo}/issues/<number> --jq .issue_dependencies_summary.blocked_by`.

If native relationships are unavailable, add `Part of #<parent>` or
`Blocked by: #<number>` at the top of the child body, and list children
in the parent's task list. A ticket is startable when all blockers are closed.

## Pull requests as a triage surface

**PRs as a request surface: no.**

## Skill operations

- "Publish to the issue tracker" means create a GitHub issue.
- "Fetch the relevant ticket" means read its GitHub issue.
- `/wayfinder`: use one `wayfinder:map` issue and child tickets labeled
  `wayfinder:research`, `wayfinder:prototype`, `wayfinder:grilling`, or
  `wayfinder:task`. Create those labels when that workflow needs them.
- Frontier: choose the first open child in map order with no open blockers
  and no assignee.
- Claim: `gh issue edit <number> --add-assignee "@me"`.
- Resolve: comment with the answer, close the child, then append a context
  pointer to the map's Decisions-so-far.

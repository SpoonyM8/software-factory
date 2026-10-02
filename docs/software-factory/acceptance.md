# Acceptance scenarios

Status: specification scenarios, not executable tests. Confirmed outcomes reference requirement IDs; detailed mechanisms remain proposed until related decisions are resolved.

| Scenario | Expected outcome | Basis |
| --- | --- | --- |
| Issue remains Triage or Backlog | No implementation agent starts. | R04 |
| Issue is Ready and capacity/instructions are available | Implementation starts in its own worktree using configured agent. | R03–R05, R08 |
| README has no required build/test instructions | Affected ticket is blocked with the missing information identified; implementation does not proceed. | R10 |
| Two Ready issues execute | Separate worktrees; no second implementation claim for the same issue. | R03, R08; claim constraint proposed |
| Different target language/framework | Factory delegates to repository instructions/toolchain, without assuming its own stack applies. | Repository-independent goal; support matrix open |
| Agent implements successfully | One active PR and evidence are ready for Jev classification. | R06, R11; exact Git operations open |
| Jev selects a route | Apply the agreed direct-merge/agent/human/rework policy; preserve classification result. | R06; policy open |
| Agent or human review fails | Implementation resumes with existing changes, failure reason, findings, and command evidence. | R07 |
| Rework generates a new commit | Same active PR; earlier approval/classification cannot approve the new revision. | R11; revision rules proposed |
| Previously merged issue is reopened | A new PR is permitted, while maintaining the one-active-PR constraint and the agreed readiness gate. | R04, R11 |
| Local app stops | No factory execution continues beyond its agreed shutdown behaviour. | R02; exact shutdown open |

## Proposed reliability scenarios

- Restart after a PR creation request whose response was lost: reconcile the existing PR instead of creating a duplicate.
- Crash while an agent has uncommitted changes: retain the worktree and show an interrupted run.
- Human pushes during review: invalidate stale results and reconcile the new revision.
- Concurrent PR merges change the target branch: follow the agreed refresh/check/reclassification rules.
- GitHub/TypeSafe outage: display actionable status and follow the agreed retry/escalation policy without treating errors as approval.
- CLI is missing, unauthenticated, or incompatible: setup/run preflight explains what to fix.
- README instructions change: use the agreed command revision/confirmation rules, and do not silently invent replacements.
- External second PR, issue closure, label conflict, cancellation, or scope edit: follow the decided conflict policy.
- Classifier receives contradictory or adversarial evidence: route according to the agreed evaluation/policy rules.

## Proposed validation plan after specification approval

Validate adapter compatibility with the installed Codex version; test state transitions, feedback handoffs, PR uniqueness, revision invalidation, process shutdown, and recovery. Exercise GitHub behaviour against a designated disposable repository with explicit permission. Evaluate Jev routing on an agreed set of representative and difficult PRs before enabling automatic merge.

Test repository selection, agents/models, budgets, threshold targets, and the definition of a successful end-to-end MVP are still open. No credentials or live agent runs are needed for this documentation phase.

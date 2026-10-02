# Product specification

## Purpose — confirmed

Provide a local web application that can apply a software factory to GitHub repositories owned by the user. Humans define and refine feature and bug issues. The factory starts work only when issues become Ready, delegates implementation and review to user-configured agents, and uses Jev to determine the PR review route.

Repository independence means the target repository need not use the factory's own implementation language or framework. Execution still requires a compatible local toolchain, usable build/test instructions, and sufficient GitHub access. Supported hosts, repository visibility, operating systems, and organisation ownership are open decisions.

## MVP scope — confirmed

| ID | Requirement |
| --- | --- |
| R01 | Produce the specification and architecture before application implementation; keep them in a dedicated docs directory. |
| R02 | Expose a local web application. The factory operates only while that application is running. |
| R03 | Support one connected repository in the MVP, with multiple issues executing concurrently. |
| R04 | Leave triage and refinement to humans; begin new work only from Ready. |
| R05 | Let the user configure the agent used for implementation and PR review. The initial user's agent is the local Codex CLI. |
| R06 | Use TypeSafe AI's Jev to classify ready-for-review PRs and determine their review/merge route; the rubric and policy remain to be defined. |
| R07 | Return failed reviews to implementation automatically, supplying what was done, why review failed, and actionable additional information. |
| R08 | Run each issue in its own Git worktree. |
| R09 | Allow agents to install dependencies, access the network, and run repository commands, scoped to the task's needs. Enforcement and execution permissions remain open. |
| R10 | Obtain repository-specific build/test commands from that repository's README. Missing required instructions block affected tickets; agents must not invent commands to proceed. |
| R11 | Allow only one active PR per issue. Reopening after a merged PR may create a subsequent PR. |
| R12 | The factory may modify the target repository as needed in this first pass. The actual onboarding changes remain to be agreed. |

The user delegated the choice of state representation and blocked handling. The proposed choices are documented in [workflow](workflow.md); they are not inferred requirements.

## Proposed web experience

1. Setup: identify local GitHub authentication, select one repository, configure agent profiles and Jev access, inspect build/test instructions.
2. Dashboard: view issue stages, queued/active runs, PR links, blocked reasons, and repository health.
3. Issue detail: inspect acceptance criteria, implementation/review attempts, commands and results, classification answers, and the current PR revision.
4. Human review: inspect changes and findings, approve or request changes, with an explicit link to the reviewed revision.
5. Controls: pause new work, stop an issue run, resolve a block, retry an interrupted run, and disconnect the repository.

The exact screens, actions, review location, and startup behaviour require confirmation. Whether closing the browser also stops the application is explicitly unresolved.

## README command contract — partially confirmed

Confirmed: commands must come from the target repository's README; missing instructions block tickets.

Open: filename/location, free-form extraction versus a standard section, supported monorepo instructions, required setup/build/test categories, whether documented scripts referenced by the README count, and how repositories with no applicable build or test are represented.

Proposed: a clearly named README section with explicit setup/build/test commands, working directories, environment prerequisites, and explicit explanations for inapplicable commands. The factory records the README revision and approved commands. It blocks absent or ambiguous instructions with a precise explanation; humans repair the README. Agents do not unblock themselves by writing their own command instructions.

## Not yet specified

Authentication, language/framework, state synchronization, concurrency limit, queue ordering, GitHub polling cadence, target branch, merge method, required checks, retry budgets, shutdown, crash recovery, retention, credential storage, supported agents, and Jev's inputs/thresholds are recorded in the [decision register](decisions.md).

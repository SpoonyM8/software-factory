# Conversation handoff

Last updated: 2026-10-02. Current phase: specification and architecture discovery.

## Resume instruction

Read this document and the linked specification documents before continuing. Resume the unresolved design discussion; do not begin application implementation or treat recommendations as approved defaults. The user asked to save the conversation context for a later session. They have not yet answered the seven questions below.

Suggested prompt for a future session:

> Read docs/software-factory/handoff.md and its linked documents. Resume specification and architecture work from the pending questions. Clarify implementation choices with me before implementing anything.

## Goal and conversation history

The user wants a software factory that can be applied to any GitHub repository they own. They initially asked about issue lifecycle states, then suggested a local workflow: Triage, Backlog, Ready, In Progress, In Agent Review, In Human Review, Done.

The subsequent request was to review the root overview.md and develop the factory. The user explicitly said: “Ask me for any further clarifying information, do not make any assumptions about the implementation. Everything should be clarified.” When asked about the deliverable, they chose a complete specification and architecture first, organised separately from implementation in a dedicated docs directory.

The original overview additionally includes Ready for Review, Needs Agent Review, Needs Human Review, and Blocked. It permits risk-based automatic merge, agent review, or human review. These differences were raised with the user. The user then delegated sensible state/blocked representation and said Jev should determine the review route, with the policy to be defined after researching Jev. The exact final lifecycle and routing policy are still proposals.

## Confirmed requirements

1. Complete the specification and architecture before application implementation.
2. Use a local web app. For the first version, factory execution exists only while the local app is running.
3. MVP supports one connected repository and multiple concurrent issues.
4. Humans own triage and refinement. Factory implementation begins only once an issue is Ready.
5. Implementation and PR review use whichever agent the user configures. This user's intended tool is their local Codex CLI. Integration work may be necessary.
6. Jev is TypeSafe AI's classification model; it determines review/merge routing for ready-for-review PRs. Risk policy is not yet defined.
7. Failed review automatically sends the issue back for implementation, providing what has been done, why it was returned, and additional actionable information.
8. Each issue runs in a separate Git worktree.
9. Agents may install dependencies, access the network, and run repository commands, scoped to the task's needs. Specific permission configuration is not yet chosen.
10. Repository-specific build/test instructions must come from that repository's README. Missing instructions block tickets; agents cannot guess commands and proceed.
11. Only one active PR is allowed per issue. Reopening after a PR was merged may create a subsequent PR.
12. The factory may make repository modifications needed for the first pass. No live GitHub repository modification was requested for this specification session.
13. The user requested recommendations for GitHub connection and stack alternatives for discussion. Neither has been selected.

## Work completed

- Read root [overview.md](../../overview.md) and [README.md](../../README.md). At review time, this repository contained those two documents and Git metadata; there was no application implementation or applicable AGENTS.md found.
- Researched primary documentation for GitHub authentication/App alternatives, Git worktrees, Codex automation/app-server, and TypeSafe Jev.
- Created the dedicated [docs/software-factory index](README.md), [specification](specification.md), [workflow proposal](workflow.md), [architecture proposal](architecture.md), [integration research](integrations.md), [decision register](decisions.md), and [acceptance scenarios](acceptance.md).
- Added a pointer to the documentation in the root README. The original overview remains intact.
- Checked local Markdown links and ran `git diff --check`; both passed before this handoff was added.
- No app code, dependency installation, credentials, repository connection, live API calls, agent execution, or executable application tests were performed. Browsing vendor documentation was the only external research activity.
- No commit or push was made in this conversation. Recheck Git status when resuming rather than assuming the recorded working-tree state persists.

## Research findings and proposals

These are recommendations or factual research, not user-selected implementation decisions.

- **GitHub:** proposed reuse of local GitHub CLI authentication, explicit owner/repository selection, and polling while the backend runs. Token or GitHub App remain alternatives. [GitHub CLI login](https://cli.github.com/manual/gh_auth_login), [GitHub App guidance](https://docs.github.com/en/apps/creating-github-apps/about-creating-github-apps/deciding-when-to-build-a-github-app).
- **Stack:** proposed TypeScript/Node.js + Fastify + React/Vite + SQLite; alternatives are Python/FastAPI + React + SQLite, or Python/FastAPI with server-rendered pages + SQLite.
- **Workflow:** proposed namespaced GitHub lifecycle labels plus a local execution record, Blocked as an additional flag retaining the lifecycle stage, and a visible Ready for Review classification stage. Review queue/activity can be metadata rather than extra labels.
- **Agents:** proposed configurable profiles and adapter contracts, initially testing Codex. CLI process invocation and app-server remain alternatives. Codex documents structured events, output schemas, saved CLI authentication, and interactive app-server exchanges. [Non-interactive mode](https://learn.chatgpt.com/docs/non-interactive-mode), [app-server](https://learn.chatgpt.com/docs/app-server).
- **Jev:** returns typed decisions; it does not generate review feedback. TypeSafe recommends atomic judgments composed in code. Proposed classification assesses criteria alignment, scope changes, assumptions, complexity, and consequences, then uses a versioned policy to route. Confidence thresholds are not selected. [TypeSafe introduction](https://docs.typesafe.ai/introduction), [confidence](https://docs.typesafe.ai/confidence), [limitations](https://docs.typesafe.ai/model-jaggedness/jev-1.13).
- **Recovery:** proposed preserving worktrees and durable attempt history, reconciling remote writes before retrying, and binding reviews/classification to the PR revision. Exact shutdown/resume behaviour is unanswered.

Research was performed on 2026-10-02. Verify current vendor behaviour when settling integrations; none has been validated against a real account or installed agent during this work.

## Exact pending questions

The following questions were presented at the end of the previous work session. They map to D01–D07 in the decision register. All remain unanswered.

1. **GitHub connection:** I recommend reusing your local `gh` login, selecting a repository in the UI, and polling GitHub while the app runs. This minimizes custom authentication work. A token or GitHub App offers more explicit access configuration. Which approach do you prefer?

2. **Stack:** My recommendation is **TypeScript + Fastify + React + SQLite**, keeping frontend and backend in one language. Alternatives are **Python + FastAPI + React + SQLite**, or **Python + FastAPI with server-rendered pages + SQLite** for less frontend tooling. Which would you like to explore?

3. **Supported repositories and computers:** Should the MVP support macOS only, or also Linux/Windows? Should it include private repositories and organisation-owned repositories? Is `github.com` sufficient?

4. **Agent integration:** Should the MVP include only a Codex adapter with an interface for future agents, or support other agents immediately? Should agents run unattended, or surface command approvals and clarification requests in the web app?

5. **Application lifetime:** Should closing the browser tab leave the backend running? I propose stopping agent processes when the backend stops, preserving worktrees, and requiring manual resumption after restart. Is that the behaviour you want?

6. **README commands:** Should we require a standard README section, or interpret existing instructions? How should repositories with no applicable build or automated tests declare that? I recommend explicit commands—or explicit “not applicable” explanations—to avoid guessing.

7. **Jev access:** Do you already have TypeSafe API access? May the factory send relevant repository code, diffs, and issue content to its hosted API, including private repository content?

## Next steps

1. Recheck repository guidance and working-tree changes; read the document index and existing drafts.
2. Resume with the seven pending questions, incorporating any answers the user supplies. Do not repeat questions already answered in the new session.
3. Record accepted choices, rejected proposals, and remaining questions in the decision register. Update the affected documents to agree with those decisions.
4. Resolve D08–D26 in manageable rounds: lifecycle authority, Jev policy/rejection, human review, merging, execution permissions, storage, concurrency, budgets, external changes, reopening, dependencies, credentials, startup, synchronization, and UI/packaging.
5. Resolve D27 by specifying the selected component interfaces, data/configuration schemas, event contracts, transitions/recovery, and implementation milestones. Align acceptance scenarios with the final choices.
6. Present a complete, internally consistent specification and architecture for review. Application implementation is a later phase, not the next action implied by this handoff.

## Outstanding boundaries

The current documentation is a discovery draft, not a complete implementation contract. No framework, database, authentication method, agent permission flags, model version, classification threshold, merge method, concurrency limit, README format, or shutdown behaviour has been approved merely because it appears in a proposal. State/blocked design was delegated, but its proposed details remain visible for review. Do not reinterpret the user's request to save context as a request to start implementation.

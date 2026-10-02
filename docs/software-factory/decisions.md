# Decision register

## Confirmed by the user

| ID | Decision |
| --- | --- |
| C01 | Specification and architecture first; application implementation follows later. |
| C02 | Local web app; factory operates only while the app runs. |
| C03 | One repository in MVP; concurrent issues. |
| C04 | User-configured implementation/review agents; initial user's tool is local Codex CLI. |
| C05 | Jev is TypeSafe AI's model and determines the review route; policy follows research. |
| C06 | Human triage/refinement; factory begins from Ready. |
| C07 | Failed review automatically returns to implementation with history and actionable feedback. |
| C08 | Separate worktree for each issue. |
| C09 | Task-scoped dependency installation, network, and repository commands are permitted. |
| C10 | README supplies repository build/test instructions; missing instructions block tickets. |
| C11 | One active PR per issue; reopening after merging can create another PR. |
| C12 | Target repository modifications are permitted for the first pass. |
| C13 | State representation and blocked handling are delegated to a sensible design; proposals are documented. |

## Next discussion

The exact questions presented to the user and the session context are preserved in the [conversation handoff](handoff.md). D01–D07 are unanswered as of 2026-10-02.

| ID | Question | Recommendation / alternatives |
| --- | --- | --- |
| D01 | How do we connect GitHub? | Existing `gh` login plus owner/repo selection and polling; alternatively token or GitHub App. |
| D02 | Which application stack? | A: TypeScript/Fastify/React/SQLite; B: Python/FastAPI/React/SQLite; C: Python with server-rendered UI/SQLite. |
| D03 | Which OS, GitHub hosts, visibility, and ownership must MVP support? | Ask explicitly about macOS/Linux/Windows, github.com/Enterprise, private/public, and personal/org repositories. |
| D04 | Which agents and Codex interaction mode must MVP support? | Codex adapter first with a pluggable interface; finite CLI attempts or app-server with interactive approvals. |
| D05 | What exactly means app running, and what happens at restart? | Propose backend lifetime; closing tab does not stop it; backend shutdown stops agents; retained work and manual recovery after restart. |
| D06 | What is the README command format and validation contract? | Standard section versus free-form extraction; required categories, monorepo paths, inapplicable build/test, environment prerequisites. |
| D07 | Are Jev API credentials available, and may relevant code/issue content leave the machine? | User account/key and data scope must be confirmed; research does not establish access. |

## Remaining questions to resolve before implementation

| ID | Decision needed | Proposal / clarification |
| --- | --- | --- |
| D08 | Lifecycle labels and authority | Proposed stages in workflow; determine exact labels and human-versus-factory edit precedence. |
| D09 | Review policy | Define risk dimensions, route choices, confidence thresholds, mandatory escalation, and evaluation criteria. |
| D10 | Rejection semantics | Rework versus terminal rejection; difference between coding defects and missing human requirements. |
| D11 | Human review channel/authorization | Local UI, GitHub submitted reviews, or both; which actors count; owner reviewing own PR; comments versus formal reviews. |
| D12 | Merge/completion | Target branch, merge method, required checks, whether factory merges human-approved PRs, issue closure and no-change completion. |
| D13 | Agent versus factory GitHub writes | Propose factory owns commit/push/PR/merge/state mutations; define agent credential exposure and permissions. |
| D14 | Local execution permissions | Select Codex permission settings; handle dependency/network approvals; review read-only behaviour; task scope enforcement versus guidance. |
| D15 | Worktree/clone storage | Existing or factory-owned clone, paths, branch naming, retention/cleanup, reviewer workspace, cancelled work. |
| D16 | Capacity and queue | Maximum concurrent implementation/review processes, FIFO versus priority, slot use by blocked/waiting tasks. |
| D17 | Retries and budgets | Agent failure/rework limits, time/cost budgets, classifier/API retry policy, escalation after exhaustion. |
| D18 | Issue edits and external PRs | Scope changes, human cancellation, existing/multiple PR adoption, closed-unmerged PR handling. |
| D19 | Reopened issue | Explicit Ready gate; new cycle/branch; previous-cycle feedback; scope snapshot rules. |
| D20 | Dependencies and conflicts | How dependencies are declared/resolved; merge conflict/base-update handling; invalidation of earlier checks/reviews. |
| D21 | Local persistence and secrets | SQLite confirmation, config format/location, credential/key storage, log redaction, retention/export. |
| D22 | Local application access | Loopback binding, browser session protection, single user, port choice, network access to UI. |
| D23 | Startup and setup changes | Automatically begin Ready work on start or require Start; created labels/config/files; README amendments remain human-owned. |
| D24 | Synchronization | Polling cadence, rate limits/outages, refresh control, missed events/reconciliation, reconnect/account switching. |
| D25 | Jev evidence/model lifecycle | Full diff versus scoped evidence, oversized/binary content, version pinning, reevaluation, classifier outage/override. |
| D26 | Packaging and web controls | Installation/start/stop method; UI actions; live logs; notification requirements. |
| D27 | Architecture/API/schema completion | Define selected component interfaces, persistence schemas, events, transitions, and implementation milestones. |

No unanswered decision is approved merely because a recommendation is listed. Resolve these in manageable rounds, updating the specification after each answer. The user's prior instruction requires clarification of implementation choices.

# Integration research and proposals

Research date: 2026-10-02. Only primary vendor documentation is used below. API access has not been exercised, and no real repository has been connected.

## GitHub connection

| Option | How connection works | Tradeoff |
| --- | --- | --- |
| Existing GitHub CLI login | User authenticates `gh`, then selects/pastes an owner/repository in the local UI; backend uses CLI operations/API access. | Least custom login work; requires `gh` installation and depends on that account's granted access. |
| Fine-grained personal access token | User supplies a token scoped to selected repositories and required permissions. | Explicit repository scope; token setup, storage, renewal, and organisation approval need handling. |
| GitHub App | User installs an app on selected repositories; factory obtains installation credentials. | Dedicated identity and scoped permissions; more registration/key/token lifecycle work. |

Recommendation for the MVP: reuse GitHub CLI login and use polling while the backend runs. Show the authenticated account, require explicit selection of one repository, check capabilities before execution, and create the agreed factory labels. GitHub CLI documents browser authentication and credential storage behaviour. [GitHub CLI login](https://cli.github.com/manual/gh_auth_login).

An App is a viable alternative when dedicated identity and repository-scoped access are priorities. [GitHub App guidance](https://docs.github.com/en/apps/creating-github-apps/about-creating-github-apps/deciding-when-to-build-a-github-app).

Open: CLI versus token versus App; read/write permissions; clone transport; personal/org/private repositories; existing clone versus factory-owned clone; sync interval; token storage; onboarding artifacts; branch protection; target branch; merge strategy. Polling is a recommendation, not a requirement.

## User-configured agent tools

Confirmed: user-selected agents perform implementation and review; initial user's tool is local Codex CLI. Supporting arbitrary executables does not automatically make every agent compatible.

Proposed adapter contract:

- Discover executable/version and validate authentication/capabilities without exposing credentials.
- Accept role, worktree, issue criteria, README commands, current revision, and rework history.
- Emit progress/events and a final structured outcome: completed, changes requested, blocked, failed, or interrupted.
- Return changed-file summary, commands/results, acceptance evidence, findings, and missing information.
- Support cancellation and preserve conversation/run identifiers where supported.
- Distinguish process success from task success; validate repository and check evidence independently.

Recommendation: configurable agent profiles and a pluggable adapter boundary, with only a tested Codex adapter included initially. Other tools can be added explicitly. Whether the MVP needs a generic command adapter or more named adapters is open.

### Codex approaches

**Process invocation:** `codex exec` supports JSONL events, final output schemas, and reuse of saved CLI authentication. This is a candidate for finite implementation/review attempts using the installed user's CLI. The factory must still handle interruption and unavailable permissions. [Codex non-interactive mode](https://learn.chatgpt.com/docs/non-interactive-mode).

**App-server:** a richer local integration offers structured conversation lifecycle and approval/input exchanges. This fits an app that needs to surface command approval or clarification requests while a run is active, with more protocol work. [Codex app-server](https://learn.chatgpt.com/docs/app-server).

Recommendation is pending the desired unattended/interactive permission behaviour. Exact flags, minimum version, permission profiles, network access, authentication mode, model selection, and resume/review commands must be verified for the chosen adapter before implementation. Do not treat an illustrative CLI invocation as a settled execution policy.

Proposed review isolation: start a fresh review context using the selected reviewer profile and inspect the actual PR revision. The same agent tool can fill both roles. Whether review may modify files or execute tests, and whether it needs another worktree, are open.

## Jev: established capabilities

Jev evaluates a supplied state against typed questions rather than generating review prose. Its primitives are Choice (options), Score (ordered rubric), and Noul (yes probability). TypeSafe recommends atomic questions composed in code. [Introduction](https://docs.typesafe.ai/introduction).

The documented endpoint is `POST https://api.typesafe.ai/v1/systemone` with bearer authentication and `state`, `model`, and `questions`. Results include the evaluated model and typed answers. [API reference](https://docs.typesafe.ai/api).

Choice/Score confidence describes the answer distribution; Noul has no separate confidence field. Numeric thresholds depend on use-case evaluation. Confidence must not be treated as a universal proof of correctness. [Confidence](https://docs.typesafe.ai/confidence).

The vendor identifies weaknesses involving indirection, unrelated context, numeric precision, and adversarial content. Jev does not generate explanatory text. [Jev limitations](https://docs.typesafe.ai/model-jaggedness/jev-1.13).

Official [JavaScript](https://docs.typesafe.ai/sdk/javascript) and [Python](https://docs.typesafe.ai/introduction/quickstart) client documentation exists, so Jev does not force the factory's language choice.

## Proposed PR classification design

1. Collect an evidence bundle tied to the actual PR head/base: criteria, diff, changed paths, command/check results, unresolved findings, and scope/assumption notes. Record incomplete or omitted context.
2. Ask Jev bounded questions about criteria alignment, out-of-scope changes, unsupported assumptions, code complexity, and consequences. The exact rubric is open.
3. Combine answers in an explicit versioned policy to choose direct merge eligibility, agent review, human review, or rework. Alternatively, use a direct route Choice; these alternatives require discussion/evaluation.
4. Record model/rubric/policy versions, input references, raw answers, and the routing rule that fired.
5. Before merge, check that classification/reviews still apply to the current PR revision and that agreed deterministic checks and GitHub rules pass.

Jev supplies judgments; the application's selected policy converts them into actions. Human feedback comes from humans; agent findings come from the review/implementation tool. Jev cannot supply invented prose explaining why a PR was rejected.

Proposed defaults for discussion: uncertain or incomplete evidence routes to human review; failed mandatory checks prevent merge; unavailable classification prevents automatic merge; changed PR revisions require new classification. Exact outage behaviour remains open.

Open: TypeSafe account/key availability, whether private code may be sent to the API, input coverage/size limits, model pinning, rubric, confidence/probability thresholds, route composition, rejection semantics, protected change types, overrides, and evaluation dataset. No threshold has been selected or tested.

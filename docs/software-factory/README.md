# Software factory specification and architecture

Status: discovery draft, awaiting decisions. Updated: 2026-10-02.

This directory contains product requirements, architecture proposals, integration research, and acceptance scenarios. It contains no application implementation. Implementation must not begin until the relevant decisions are resolved; an unanswered question is not an approved default.

The original [overview](../../overview.md) is the starting brief. The documents here capture subsequent clarification. Where they differ, explicitly confirmed user requirements take precedence; proposals remain proposals.

## Reading order

For a later session, start with the [conversation handoff](handoff.md), which preserves the confirmed context, exact pending questions, and next steps.

1. [Specification](specification.md): confirmed scope and product requirements.
2. [Workflow](workflow.md): proposed lifecycle and feedback handling.
3. [Architecture](architecture.md): proposed components, data model, and stack alternatives.
4. [Integrations](integrations.md): GitHub, configurable agents, and Jev research.
5. [Decisions](decisions.md): confirmed decisions and questions still requiring answers.
6. [Acceptance scenarios](acceptance.md): expected behaviour and proposed validation.

## Decision convention

- **Confirmed:** explicitly specified by the user.
- **Proposed:** recommendation for discussion, not permission to implement it.
- **Open:** no implementation choice has been made.

Permission to modify connected repositories is confirmed for the first pass. This does not settle which modifications the factory should make, and does not authorize connecting or changing a real GitHub repository during this specification phase.

## Completion of specification phase

The specification becomes ready for implementation when the decision register is resolved, component contracts and persistence/API schemas are specified for the selected stack, state transitions and recovery rules are agreed, and acceptance scenarios reflect those decisions. This draft is not yet a complete implementation contract.

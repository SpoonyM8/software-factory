This repository will become the implementation of a software factory.

The software factory will behave as follows: watches github issues, acts on them according to labels and adds labels back to them based on the state they are in. Jev will be used to classify the risk on a PR and if it can be auto-merged, rejected, or if human review is required.

## Github Labels

Github issues will drive both the feature and bug reports. The label on a github issue will determine what the next step is for either a human or agent. The following labels cover the lifecycle of a github issue:

- Triage
- Backlog
- Ready
- In Progress
- Ready for Review
- Needs Agent Review
- Needs Human Review
- In Agent Review
- In Human Review
- Blocked
- Done

Triage - the issue is being defined and is not ready to be worked on.

Backlog - the issue has been defined but should not yet be worked on.

Ready - the issue is ready for an agent to pick up.

In Progress - the issue is actively being worked on by an agent.

Ready for Review - the issue has open pull requests. Classification is required to determine if an agent review, human review, or automatic merging is needed.

Needs Agent Review - the issue has been classified as safe enough to exclude human review and requires an agent approval before merging.

Needs Human Review - the issue has been classified as higher risk or the classification lacked enough information to accurately categorise the risk. Human review is required before merging.

In Agent Review - An agent is actively reviewing the pull request.

In Human Review - A human is actively reviewing the pull request.

Blocked - A human or agent has marked the issue as blocked due to either a lack of information or waiting on dependencies.

Done - The issue has been completed and merged successfully.


### Review Classification
Jev will be used to classify if an agent generated pull request needs either agent review, human review or can just immediately be merged. This will be a risk decision about the complexity of the code change, if assumptions were made where the code does not completely match the acceptance criteria, or if additional changes were made outside the scope of the ticket.
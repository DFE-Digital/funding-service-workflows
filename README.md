# funding-service-workflows

This repository is used as a central base for all workflows that are maintained and used by funding service repositories.

## For Workflow Developers

- Most workflows should aim to be implementation generic; for example, our `build-and-publish.yml` flow works regardless of what the underlying environment is
- If a workflow has different implementations (e.g. auditing dependencies can be done with multiple types of software), then a folder should be created with a 'purpose' name, with workflows within it being named after the specific implementation. Folders may also contain subfolders to narrow down purpose; For example, `audit/dependencies/npm.yml` is a workflow with the purpose of auditing dependencies, using npm as its implementation; `audit/containers/trivy.yml` is a workflow with the purpose of auditing container images, using trivy as its implementation
- This repository is for bricks, not houses; workflows in this repository should be independent from each other. This helps to avoid multiple layers of nesting which can be difficult for teams to manage

## For Workflow Users

- To use these workflows, [follow the guidance](https://docs.github.com/en/actions/how-tos/reuse-automations/reuse-workflows) provided by Github. 
- We mandate that all teams pin workflows to a given SHA commit; this is for both stability and security practice to avoid being caught out by workflow changes.
- We also recommend:
    - Using `secrets: inherit` for simplifying passing secrets where appropriate
    - Naming each job after its purpose rather than implementation; this makes life simpler if you want to change implementation in the future
    - Where relevant, aiming to use matrix strategies to run multiple jobs across many environments
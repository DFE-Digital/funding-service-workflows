# funding-service-workflows

This repository is used as a central base for all workflows that are maintained and used by funding service repositories.

## Currently Provided Flows

### Audit

- `audit-container-trivy.yml`: Analyses container images for security issues
- `audit-dependency-npm.yml`: Analyses NPM Packages

### Linting

- `lint-yaml.yml`: Lints any YAML files to the spec found in .yamllint.yml

### Testing

- `test-dotnet.yml`: Runs a dotnet testrunner for a given solution
- `publish-test-results.yml`: Compiles .trx test file outputs into an easy-read summary for github actions

## For Workflow Developers

### Standards

- Most workflows should aim to be implementation generic; for example, our `build-and-publish.yml` flow works regardless of what the underlying environment is
- If a workflow has different implementations (e.g. auditing dependencies can be done with multiple types of software), then the name of the workflow should be in the form of <function-name>-<implementation>; for example, `audit-dependencies-npm` is a workflow for auditing dependencies, using npm as its core technology
- This repository is for bricks, not houses; workflows in this repository should be independent from each other. This helps to avoid multiple layers of nesting which can be difficult for teams to manage. Workflows in this repository should only use `workflow_call` triggers
- The `meta-` workflows are for use in **this** repository only and are used to lint workflows. Workflows must pass these actions to be accepted, and preferably developers should resolve all warnings flagged too
- It is acceptable to disable the truthy rule for github action triggers, as this is considered standard workflow syntax; this rule is kept in as it is useful for other types of YAML file that are linted by `lint-yaml`
- Always pin actions against SHA hashes; PRs using any other tags will be rejected

### Submission Process

- [ Optional ] - Create an Issue
    - This helps us to stay organised, particularly when handling requests from other teams, but we will accept PRs without an associated issue, so long as the PR is well explained
    - DevOps engineers can instead create a JIRA ticket on their own board
- Create a Pull Request
    - If an issue is available that is relevant, please link to it, or provide a link to a JIRA ticket
    - A template is provided that should be filled out for each PR 
    - A linting run will automatically be triggered on every push; if this fails due to linting errors, then resolution is mandatory
- Obtain a review from DevOps Engineers and Technical Architects
    - Only DevOps Engineers and Technical Architects have permission to merge into main


## For Workflow Users

- To use these workflows, [follow the guidance](https://docs.github.com/en/actions/how-tos/reuse-automations/reuse-workflows) provided by Github. 
- We mandate that all teams pin workflows to a given SHA commit; this is for both stability and security practice to avoid being caught out by workflow changes.
- We also recommend:
    - Using `secrets: inherit` for simplifying passing secrets where appropriate
    - Naming each job after its purpose rather than implementation; this makes life simpler if you want to change implementation in the future
    - Use singular nouns for file names instead of plurals (e.g. dependency instead of dependencies)
    - Where relevant, aiming to use matrix strategies to run multiple jobs across many environments
    - It is acceptable to disable the truthy rule for github action triggers, as this is considered standard workflow syntax; this rule is kept in as it is useful for other types of YAML file that are linted by `lint-yaml`
## Purpose

This repository follows a specification-first development workflow for AI-assisted software engineering.

Specifications define observable system behaviour. Tests are derived from specifications, and implementation is derived from both.

Agents SHALL minimise unrelated changes and SHALL not invent unverified APIs, types, selectors, commands, or project conventions.

## Requirements writing

Acceptance criteria SHALL use EARS notation for observable system behaviour.

Use the following EARS patterns:

* Ubiquitous:

  * THE System SHALL [response].
* Event-driven:

  * WHEN [event] THE System SHALL [response].
* State-driven:

  * WHILE [state] THE System SHALL [response].
* Unwanted behaviour:

  * IF [undesired condition] THEN THE System SHALL [response].
* Optional feature:

  * WHERE [feature exists] THE System SHALL [response].
* Complex:

  * WHILE [state], WHEN [event], THE System SHALL [response].

EARS requirements SHALL describe externally observable behaviour, not implementation steps.

Each acceptance criterion SHALL contain one primary system response.

Shared constraints SHALL be written once in the most appropriate common section rather than repeated across multiple acceptance criteria, test cases, or documents.

Use complex EARS clauses only when the condition, state, and event are all necessary.

When a requirement contains several simultaneous conditions, prefer a short list of conditions or a decision table instead of a long, nested EARS sentence.

Requirements SHALL use consistent terminology for actors, system components, states, events, and responses.

Implementation details SHALL not be included in acceptance criteria unless they are externally observable contractual requirements.

## Specification-first development workflow

For any non-trivial feature, bug fix, workflow change, API integration, data-model change, navigation change, persistence change, security change, or user-visible behaviour change:

1. Read the relevant existing specification before modifying code.
2. Identify the current expected behaviour and the proposed behaviour.
3. If behaviour changes, update the specification before implementation.
4. If no suitable specification exists, create one before implementation.
5. Treat the specification as the source of truth for implementation and tests.
6. Derive tests from the specification’s acceptance criteria.
7. Write or update tests before modifying the implementation.
8. Confirm that new or changed tests fail for the expected reason.
9. Implement the minimum change required to satisfy the specification.
10. Run the relevant tests and verification commands.
11. Update the specification, tests, and implementation in the same pull request.

If an Issue, task description, design document, test, or implementation conflicts with an existing specification, do not silently choose one source.

Document the conflict and request or propose a source-of-truth resolution before implementing conflicting behaviour.

A new specification is not required for purely mechanical changes such as:

* spelling corrections
* comment corrections
* file-header corrections
* removing unused imports
* deleting unreachable or commented-out code
* formatting-only changes
* mechanical renames with no behavioural or contractual impact

If a supposedly mechanical change affects observable behaviour, API compatibility, persistence, navigation, generated output, or tests, treat it as a behavioural change.

## Specification structure

Unless the repository defines another convention, specifications SHOULD include:

1. Context
2. Scope
3. Out of scope
4. Definitions
5. Current behaviour
6. Desired behaviour
7. Acceptance criteria
8. Edge cases
9. Error behaviour
10. Dependencies and contracts
11. Test mapping
12. Open questions

Acceptance criteria SHALL be independently testable.

Each test mapping SHOULD identify which acceptance criterion it verifies.

Specifications SHALL distinguish confirmed behaviour from assumptions and unresolved questions.

## API and contract verification

Verify the backend, API, schema, persistence, and generated client contracts before assuming that an endpoint, field, model, status, function, or behaviour exists.

Authoritative contracts MAY include:

* checked-in OpenAPI documents
* official API schemas
* generated client interfaces
* source repository definitions
* framework-generated documentation
* compiler output
* build output
* verified runtime responses

Do not invent unverified endpoints, functions, types, fields, selectors, status values, or response structures.

If something cannot be confirmed from an authoritative source, mark it as `unverified`.

When a live service and a checked-in contract disagree:

1. Record the observed disagreement.
2. Determine which source is authoritative.
3. Update or obtain the authoritative contract.
4. Regenerate dependent clients or types.
5. Update tests and implementation against the resolved contract.

Do not replace a verified working integration solely because a stale generated client does not expose the same operation.

Do not manually edit generated code.

Changes to generated code SHALL be made by modifying the source schema, generator configuration, templates, or generation process and then regenerating the output.

## Testing rules

Write tests before implementation for behavioural changes.

Confirm that new or changed tests fail before writing the implementation.

The failure SHALL occur for the expected behavioural reason, not because of unrelated compilation, configuration, fixture, or environment errors.

Do not modify or delete existing tests solely to make a change pass.

Existing tests MAY be changed when:

* the specification intentionally changes
* the existing test contradicts the resolved source of truth
* the test contains a confirmed defect
* the test relies on obsolete behaviour that has been explicitly removed

When changing an existing test, document why the previous expectation is no longer valid.

Tests SHALL verify observable behaviour rather than private implementation details unless no stable public boundary exists.

Tests SHOULD cover:

* expected behaviour
* boundary conditions
* invalid input
* error handling
* retry or recovery behaviour where applicable
* state transitions
* persistence across relevant lifecycle boundaries
* compatibility with verified external contracts

Avoid tests that pass because of broad mocks, permissive assertions, uncontrolled timing, or unrelated fallback behaviour.

Do not introduce sleeps when deterministic synchronisation is available.

Do not weaken assertions to hide nondeterministic or incorrect behaviour.

## Change discipline

Make the smallest coherent change that satisfies the specification.

When reducing duplication, only change text or code that directly causes the duplication.

Do not rewrite unrelated sections, rename unrelated symbols, move unrelated files, or restructure unrelated components unless explicitly requested or required for correctness.

Do not combine behavioural changes with broad cleanup.

Do not introduce new abstractions solely to reduce a small amount of local duplication.

Preserve established repository conventions unless the task explicitly changes them.

Before creating a new utility, abstraction, dependency, model, service, or directory, check whether an equivalent already exists.

Do not add speculative functionality for possible future use.

Do not retain dead compatibility paths unless the specification requires them.

## Issue and pull request discipline

Before implementation:

* read the complete Issue or task
* inspect linked discussions and review comments
* inspect the relevant specification
* inspect the current implementation
* identify dependencies and affected tests

A pull request for a behavioural change SHOULD include:

* the specification change
* tests derived from the acceptance criteria
* the implementation
* verification evidence
* relevant contract updates or regenerated code
* known limitations or unresolved questions

Do not claim that a requirement is complete unless its acceptance criteria are implemented and verified.

Do not close an Issue solely because code was written.

Confirm that the implemented behaviour matches the resolved specification and that relevant verification passes.

## Verification

Use repository-defined verification commands when available.

Verification MAY include:

* formatting
* linting
* static analysis
* type checking
* unit tests
* integration tests
* UI tests
* contract tests
* build verification
* generated-code consistency checks

Report verification precisely.

Use statements such as:

* `Passed: 142 tests`
* `Build succeeded for target ExampleApp`
* `Not run: integration tests require external credentials`
* `Blocked: authoritative API schema unavailable`

Do not report a command as successful if it was not run.

Do not describe partial verification as complete verification.

If verification cannot be completed, state:

1. what was run
2. what passed
3. what failed
4. what was not run
5. why it was not run
6. the remaining risk

## Repository and environment safety

Do not execute destructive commands without explicit justification.

Destructive operations include:

* force pushing
* rewriting shared history
* deleting branches
* deleting persistent data
* resetting tracked work
* removing uncommitted changes
* modifying production resources
* rotating or exposing credentials

Before a destructive operation:

1. explain the intended effect
2. explain the risk
3. confirm that a safer alternative is insufficient
4. verify the target
5. preserve recoverability where possible

Do not expose secrets, access tokens, private keys, credentials, personal data, or protected environment values in code, logs, Issues, pull requests, or documentation.

Do not commit local environment files unless the repository explicitly requires them.

## Dependencies

Do not add a dependency before checking whether the requirement can be satisfied using the existing platform, standard library, or installed dependencies.

When adding a dependency, verify:

* maintenance status
* licence compatibility
* supported runtime versions
* security implications
* transitive dependency impact
* repository compatibility
* necessity for the requested behaviour

Do not perform unrelated dependency upgrades in the same change.

## Documentation

Update documentation when behaviour, setup, architecture, commands, contracts, or developer workflow changes.

Documentation SHALL describe the current verified state.

Do not document speculative behaviour as implemented behaviour.

Examples SHALL match current APIs and repository conventions.

Commands SHALL be runnable from the documented working directory.

## Agent response requirements

When completing a task, report:

1. files changed
2. behaviour changed
3. specifications added or updated
4. tests added or updated
5. verification performed
6. unresolved risks or blocked items

Keep the report factual and concise.

Do not claim completion when required implementation, tests, specification updates, or verification remain incomplete.

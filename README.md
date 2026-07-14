# Spec-First AGENTS.md Notes

A small `AGENTS.md` ruleset I use for specification-first AI-assisted development.

It focuses on:

* EARS acceptance criteria
* specification-first changes
* tests derived from observable behaviour
* API and contract verification
* generated-code protection
* minimal, task-scoped changes

These are working notes derived from recurring issues encountered while using coding agents. They are not a validated development standard.

## Core idea

Define observable behaviour using EARS, derive tests from those acceptance criteria, and implement the minimum change required to satisfy them.

```text
Specification
      ↓
Acceptance criteria
      ↓
Tests
      ↓
Implementation
```

## Usage

Copy `AGENTS.md` into the root of a repository and add project-specific context where required.

```text
your-project/
├── AGENTS.md
├── README.md
└── src/
```

Repository-specific additions may include:

* specification locations
* generated-code paths
* authoritative API contracts
* verification commands
* known broken states
* intentional behaviour that agents may otherwise “fix”

Example:

````md
## Repository-specific context

### Specifications

Feature specifications are stored at:

`src/features/<feature>/specs/<feature>.md`

### Generated code

Files under `src/generated/` are generated from `openapi.yaml` and SHALL NOT be edited manually.

### Verification

```sh
npm run lint
npm run typecheck
npm test
```
````

## Workflow

For non-trivial behavioural changes:

```text
Read the existing specification
        ↓
Update or create the specification
        ↓
Derive tests from the acceptance criteria
        ↓
Implement the minimum compliant change
        ↓
Submit specification, tests, and implementation together
```

When an Issue or task conflicts with an existing specification, the source-of-truth conflict should be resolved before implementation.

## EARS examples

Event-driven:

```text
WHEN the user submits a valid form THE System SHALL display a confirmation message.
```

Unwanted behaviour:

```text
IF the server rejects the request THEN THE System SHALL preserve the entered form values.
```

State-driven:

```text
WHILE the upload is in progress THE System SHALL prevent another upload submission.
```

Complex:

```text
WHILE the user is authenticated, WHEN the session expires, THE System SHALL request re-authentication.
```

Acceptance criteria describe externally observable behaviour rather than implementation steps.

Avoid:

```text
WHEN the user submits the form THE System SHALL call FormService.submit().
```

Prefer:

```text
WHEN the user submits a valid form THE System SHALL store the submitted data.
```

## Scope

This repository contains development instructions only.

It does not provide a specification framework, test runner, coding agent, OpenAPI generator, or CI configuration.

The rules are intentionally small and are expected to change as they are used in real projects.

## Licence

MIT

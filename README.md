# Spec-First AGENTS.md

A reusable `AGENTS.md` for specification-first, test-first AI-assisted software development.

The rules are designed to constrain coding agents using:

* EARS acceptance criteria
* specification-first development
* test-first implementation
* API contract verification
* generated-code protection
* minimal-change discipline
* explicit verification reporting

## Usage

Copy `AGENTS.md` into the root of your repository:

```text
your-project/
├── AGENTS.md
├── README.md
└── src/
```

Then add project-specific rules to the same file or to a repository-specific section.

Example:

````md
## Repository-specific context

### Specifications

Feature specifications are stored at:

`src/features/<feature>/specs/<feature>.md`

### Generated code

Files under `src/generated/` are generated from `openapi.yaml` and SHALL NOT be edited manually.

### Verification

Run:

```sh
npm run lint
npm run typecheck
npm test
````

````

## Core workflow

For behavioural changes, the expected sequence is:

```text
Read specification
        ↓
Update specification
        ↓
Derive acceptance tests
        ↓
Confirm tests fail
        ↓
Implement minimum change
        ↓
Run verification
        ↓
Submit spec + tests + implementation together
````

## EARS examples

Event-driven requirement:

```text
WHEN the user submits a valid form THE System SHALL display a confirmation message.
```

Unwanted behaviour requirement:

```text
IF the server rejects the request THEN THE System SHALL preserve the entered form values.
```

State-driven requirement:

```text
WHILE the upload is in progress THE System SHALL prevent a second upload submission.
```

Complex requirement:

```text
WHILE the user is authenticated, WHEN the session expires, THE System SHALL request re-authentication.
```

Acceptance criteria describe externally observable behaviour. They should not prescribe internal implementation steps.

Avoid:

```text
WHEN the user submits the form THE System SHALL call FormService.submit().
```

Prefer:

```text
WHEN the user submits a valid form THE System SHALL store the submitted data.
```

## Recommended customisation

Keep reusable rules in the common sections and add repository-specific information for:

* specification locations
* build and test commands
* generated-code paths
* external repositories
* unavailable fixtures
* intentional unusual behaviour
* known broken states
* CI constraints
* authoritative API contracts

Project-specific exceptions should be explicit so that agents do not “fix” intentional behaviour.

## Scope

This repository contains development instructions only.

It does not provide:

* a coding agent
* a specification framework
* a test runner
* an OpenAPI generator
* CI configuration
* project-specific architecture rules

The file is intended to be copied and adapted to the repository where it is used.

## Contributing

Changes should remain broadly reusable.

Repository-specific rules, product names, endpoints, credentials, application paths, and implementation details should not be added to the shared `AGENTS.md`.

Proposed changes should explain:

1. the agent failure mode being prevented
2. why the rule is generally applicable
3. whether the rule overlaps with an existing section
4. the smallest wording change needed

## Licence

MIT

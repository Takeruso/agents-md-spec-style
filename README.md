# Spec-First AGENTS.md

A minimal, reusable `AGENTS.md` for specification-first AI-assisted software development.

The rules constrain coding agents to:

* EARS acceptance criteria
* specification-first development
* API and contract verification
* generated-code protection
* minimal-change discipline

Primary concept: define observable behaviour with EARS, derive tests from those acceptance criteria, then implement the minimum compliant change.

## Usage

Copy `AGENTS.md` into the root of your repository:

```text
your-project/
├── AGENTS.md
├── README.md
└── src/
```

Then add project-specific rules in a repository-specific section.

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
```
````

## Core workflow

For non-trivial behavioural changes:

```text
Read specification
        ↓
Update or create specification
        ↓
Derive tests from acceptance criteria
        ↓
Implement minimum change
        ↓
Include specification + tests + implementation together
```

IF an Issue or task conflicts with an existing specification, resolve the source of truth before implementing.

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
WHILE the upload is in progress THE System SHALL prevent a second upload submission.
```

Complex:

```text
WHILE the user is authenticated, WHEN the session expires, THE System SHALL request re-authentication.
```

Acceptance criteria describe externally observable behaviour. They SHALL NOT prescribe internal implementation steps.

Avoid:

```text
WHEN the user submits the form THE System SHALL call FormService.submit().
```

Prefer:

```text
WHEN the user submits a valid form THE System SHALL store the submitted data.
```

## Recommended customisation

Keep reusable rules in the common sections. Add repository-specific information for:

* specification locations
* generated-code paths
* authoritative API contracts
* verification commands

Project-specific exceptions SHOULD be explicit so agents do not “fix” intentional behaviour.

## Scope

This repository contains development instructions only.

It does not provide:

* a coding agent
* a specification framework
* a test runner
* an OpenAPI generator
* CI configuration
* project-specific architecture rules

Copy and adapt `AGENTS.md` for the repository where it is used.

## Contributing

Changes SHOULD remain broadly reusable and SHOULD preserve the minimal, EARS-focused scope.

Repository-specific rules, product names, endpoints, credentials, application paths, and implementation details SHOULD NOT be added to the shared `AGENTS.md`.

Proposed changes SHOULD explain:

1. the agent failure mode being prevented
2. why the rule is generally applicable
3. whether the rule overlaps with an existing section
4. the smallest wording change needed

## Licence

MIT

# AGENTS.md

## Acceptance criteria

Acceptance criteria SHALL use EARS notation.

EARS patterns:

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

Acceptance criteria SHALL describe externally observable behaviour.

Each acceptance criterion SHALL contain one primary system response.

WHEN several simultaneous conditions apply, Agents SHALL prefer a condition list or decision table over a nested EARS sentence.

## Spec-first workflow

For non-trivial behavioural changes, Agents SHALL read the relevant existing specification before modifying code.

WHEN behaviour changes, Agents SHALL update or create the specification before implementation.

Agents SHALL treat the resolved specification as the source of truth for tests and implementation.

Agents SHALL derive tests from the specification’s acceptance criteria.

Agents SHALL include the specification, tests, and implementation in the same change.

IF an Issue, task, design document, test, or implementation conflicts with an existing specification, Agents SHALL NOT implement the conflicting behaviour.

Agents SHALL document the conflict and obtain source-of-truth resolution before implementing conflicting behaviour.

## Contract verification

Agents SHALL verify APIs, schemas, models, selectors, and generated interfaces against authoritative contracts before use.

IF a fact cannot be confirmed from an authoritative source, Agents SHALL mark it as `unverified`.

Agents SHALL NOT manually edit generated code.

Changes to generated code SHALL be made by modifying the source schema, generator configuration, templates, or generation process, then regenerating the output.

## Change discipline

Agents SHALL make the smallest change required to satisfy the specification.

Agents SHALL NOT modify unrelated sections, files, tests, or architecture.

Agents SHALL NOT change or delete tests solely to make a change pass.

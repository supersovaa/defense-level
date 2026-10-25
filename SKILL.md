---
name: defense-level
description: Bound defensive design, implementation, testing, and review to repository-declared environment and interaction defense levels.
---

# Defense Level

Use this skill to keep defensive work aligned with the level a repository actually requires.

## Configuration

A repository using this skill declares two independent levels where it invokes the skill:

- `environment`: `0`, `1`, `2`, or `3`
- `interaction`: `0`, `1`, `2`, or `3`

Example:

```text
Use `supersovaa/defense-level` with `environment=2, interaction=1`.
For interaction, defend against duplicate submissions.
```

Treat missing levels as incomplete configuration and obtain explicit values from the repository or task rather than inferring them.

## Environment defense

Environment defense covers persisted state, dependencies, storage, files, services, runtime infrastructure, and other system surroundings.

| Level | Defend against | Typical cases |
| --- | --- | --- |
| `0` | Contract-valid environment | Database contents are consistent, internal state is valid, dependencies behave according to contract |
| `1` | Explicitly specified environment defenses | Required handling for a specified timeout, corrupt record, or other named environment case |
| `2` | Ordinary operational failure | Connection failure, timeout, I/O failure, dependency unavailability |
| `3` | Invalid or contract-breaking environment | Corrupt or inconsistent stored data, malformed files, unexpected dependency responses, impossible internal state |

Each level includes the behavior of all lower levels on the same axis.

The key boundary is:

- `1`: defend against explicitly specified environment cases, including cases otherwise associated with level `2` or `3`;
- `2`: cover ordinary operational failures as a category;
- `3`: cover invalid or contract-breaking environment states as a category.

## Interaction defense

Interaction defense covers users, callers, requests, supplied values, operation order, and other inputs crossing into the system.

| Level | Defend against | Typical cases |
| --- | --- | --- |
| `0` | Contract-valid interaction | Correct types, valid values, valid operation order, intended use |
| `1` | Explicitly specified interaction defenses | Required handling for specified duplicate actions, invalid values, or other named interaction cases |
| `2` | Accidental misuse | Mistyped values, duplicate actions, invalid order, benign out-of-range input |
| `3` | Deliberate contract violation | Hostile input, crafted requests, privilege-bypass attempts, intentional misuse |

Each level includes the behavior of all lower levels on the same axis.

The key boundary is:

- `1`: defend against explicitly specified interaction cases, including cases otherwise associated with level `2` or `3`;
- `2`: cover accidental misuse as a category;
- `3`: cover deliberate contract violations as a category.

## Apply the declared levels

Apply the declared levels consistently to design, implementation, testing, and review.

At level `1` or higher, guarantee defenses explicitly required by the repository or task for the relevant axis. Apply each specified defense even when its case would ordinarily fall under a higher level. Treat each specified case as a targeted requirement; levels `2` and `3` provide category-wide coverage.

Design behavior for cases covered by the declared levels.
Implement handling required by those cases.
Test the defensive behavior required by those cases.
Report missing defenses during review when the missing defense is within the declared levels.

Unspecified cases that require a higher level remain outside the required defense scope until the repository or task changes that level.

Existing defenses beyond the declared levels may remain.
The declared levels alone are not a reason to remove working defensive behavior.

Keep the declared levels stable during a task.
A level change comes from the repository or task requirements rather than from this skill itself.
When a defense is explicitly required while its axis is declared at level `0`, identify the mismatch so the repository or task can explicitly raise that axis to at least level `1`.

## Recommended adoption

For a new implementation, prefer starting at `environment=0, interaction=0` and raising each axis when concrete requirements justify broader defense.

Treat this as a recommendation rather than a requirement.

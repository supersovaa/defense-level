---
name: defense-level
description: Bound defensive design, implementation, testing, and review to repository-declared environment and interaction defense levels.
---

# Defense Level

Use this skill to keep defensive work aligned with the level a repository actually requires.

## Configuration

A repository using this skill declares two independent levels where it invokes the skill:

- `environment`: `0`, `1`, or `2`
- `interaction`: `0`, `1`, or `2`

Example:

```text
Use `supersovaa/defense-level` with `environment=0, interaction=1`.
```

Treat missing levels as incomplete configuration and obtain explicit values from the repository or task rather than inferring them.

## Environment defense

Environment defense covers persisted state, dependencies, storage, files, services, runtime infrastructure, and other system surroundings.

| Level | Defend against | Typical cases |
| --- | --- | --- |
| `0` | Contract-valid environment | Database contents are consistent, internal state is valid, dependencies behave according to contract |
| `1` | Ordinary operational failure | Connection failure, timeout, I/O failure, dependency unavailability |
| `2` | Invalid or contract-breaking environment | Corrupt or inconsistent stored data, malformed files, unexpected dependency responses, impossible internal state |

Level `1` includes level `0` behavior.
Level `2` includes levels `0` and `1` behavior.

The key boundary is:

- `1`: a valid environment can fail to operate;
- `2`: the environment itself can violate its contract.

## Interaction defense

Interaction defense covers users, callers, requests, supplied values, operation order, and other inputs crossing into the system.

| Level | Defend against | Typical cases |
| --- | --- | --- |
| `0` | Contract-valid interaction | Correct types, valid values, valid operation order, intended use |
| `1` | Accidental misuse | Mistyped values, duplicate actions, invalid order, benign out-of-range input |
| `2` | Deliberate contract violation | Hostile input, crafted requests, privilege-bypass attempts, intentional misuse |

Level `1` includes level `0` behavior.
Level `2` includes levels `0` and `1` behavior.

The key boundary is:

- `1`: a benign caller can make mistakes;
- `2`: a caller can intentionally violate the contract.

## Apply the declared levels

Apply the declared levels consistently to design, implementation, testing, and review.

Design behavior for cases covered by the declared levels.
Implement handling required by those cases.
Test the defensive behavior required by those cases.
Report missing defenses during review when the missing defense is within the declared levels.

Cases that require a higher level remain outside the required defense scope until the repository or task changes that level.

Existing defenses beyond the declared levels may remain.
The declared levels alone are not a reason to remove working defensive behavior.

Keep the declared levels stable during a task.
A level change comes from the repository or task requirements rather than from this skill itself.
When a requirement exceeds the current levels, identify the mismatch so the level can be updated explicitly.

## Recommended adoption

For a new implementation, prefer starting at `environment=0, interaction=0` and raising each axis when concrete requirements justify broader defense.

Treat this as a recommendation rather than a requirement.

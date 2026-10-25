# defense-level

`defense-level` is a lightweight skill for bounding defensive design, implementation, testing, and review to the defense level a repository actually requires.

It uses two independent axes:

- `environment`: failures and invalid states originating from storage, dependencies, runtime infrastructure, and other system surroundings;
- `interaction`: mistakes and hostile behavior originating from users, callers, requests, and supplied input.

Each axis uses levels `0` through `3`.

## Levels

### Environment

| Level | Meaning |
| --- | --- |
| `0` | Assume a contract-valid environment |
| `1` | Guarantee explicitly specified environment defenses |
| `2` | Also handle ordinary operational failures |
| `3` | Also handle invalid, corrupt, or contract-breaking environment state |

### Interaction

| Level | Meaning |
| --- | --- |
| `0` | Assume contract-valid interaction |
| `1` | Guarantee explicitly specified interaction defenses |
| `2` | Also handle accidental misuse |
| `3` | Also handle deliberate contract violations and hostile interaction |

Higher levels include the lower levels on the same axis.
The two axes are independent, so combinations such as `environment=0, interaction=3` and `environment=3, interaction=0` are valid.
At level `1` or higher, a specified defense is required even when its case ordinarily belongs to level `2` or `3`. Level `1` requires the named cases, while levels `2` and `3` also cover their respective broader categories.

## Repository configuration

Declare the current level where the repository invokes the skill.

```text
Use `supersovaa/defense-level` with `environment=2, interaction=1`.
For interaction, defend against duplicate submissions.
```

The declared levels apply to design, implementation, testing, and review.
Missing defenses are required when they fall within the declared levels, including explicitly specified defenses at level `1` or higher. If a defense is explicitly required while its axis is at level `0`, identify the mismatch and update the declared level explicitly.
Existing broader defenses may remain.

For new implementations, starting from `environment=0, interaction=0` and raising each axis as requirements appear is recommended.

## Installation

Place `SKILL.md` where the target skill system loads skills from, or use the repository/package according to that system's installation method.

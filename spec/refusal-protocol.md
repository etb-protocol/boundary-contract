# Refusal Protocol — ETB Protocol Specification v0.1

## Overview

Refusal is not failure.

In most systems, silence is treated as an error condition —  
something to be avoided, papered over, or replaced with a best guess.

ETB Protocol inverts this default.

> **Silence is safer than a confident wrong answer.**

The Refusal Protocol defines when, how, and why an ETB-compliant system  
must intentionally withhold output.

---

## Core Principle

> **Refusal over Uncertainty.**

When a system cannot produce output that meets the minimum trust threshold,  
it must refuse — explicitly, visibly, and with a declared reason.

Refusal is:
- A deliberate design feature
- A trust signal in itself
- Evidence that the system is operating with integrity

Refusal is not:
- An error
- A failure state
- Something to be suppressed or hidden

---

## Refusal Trigger Conditions

A system must refuse when any of the following conditions are met:

### Trigger 1: Source Unavailability
All required primary sources are unreachable or unresponsive.

```
Primary source : unavailable
Secondary source : unavailable
→ REFUSED
```

### Trigger 2: Source Inconsistency
Multiple sources return conflicting values beyond acceptable variance.

```
Source A returns : value X
Source B returns : value Y
Variance exceeds threshold
→ REFUSED
```

### Trigger 3: Missing Required Data
A value required for safe interpretation is absent and cannot be derived.

```
Required field : null or missing
No safe derivation path exists
→ REFUSED
```

### Trigger 4: Unsafe Interpretation Path
Producing output would require assumptions the system is not authorized to make.

```
Interpretation requires : unstated assumption
Assumption is : unverifiable or unsafe
→ REFUSED
```

### Trigger 5: Downstream Trust Collapse
All upstream values have been marked REFUSED,  
making any derived output meaningless.

```
Upstream trust : REFUSED
Derived output would be : ungrounded
→ REFUSED
```

---

## Refusal Output Format

When refusing, the system must produce a structured refusal record —  
not silence, not an error code, but an explicit declaration.

**Minimum required fields:**

```
value   : null
trust   : REFUSED
layer   : [RAW / DERIVED / ESTIMATED]
reason  : [human-readable explanation]
```

**Examples:**

```
Health Factor   : null    [REFUSED] — source unavailable
Status          : null    [REFUSED] — cannot determine from refused upstream
Liq Distance    : null    [REFUSED] — not meaningful without health factor
```

**Key principle:**  
The reason field is mandatory.  
A refusal without a reason is an incomplete refusal.

---

## Refusal Propagation

Refusal propagates downstream through all dependent values.

```
Source → REFUSED
  ↓
Raw value → REFUSED
  ↓
Derived value → REFUSED
  ↓
Estimated value → REFUSED
  ↓
State assessment → REFUSAL
```

Once a value is REFUSED, no downstream system may treat it as available.  
There is no override mechanism.  
There is no fallback to estimation.

---

## Partial Refusal — DEGRADED Path

When some sources are available and some are not,  
the system enters DEGRADED rather than full REFUSAL.

**DEGRADED conditions:**
- At least one source is available
- Available data is sufficient for partial observation
- Output is produced with explicit DEGRADED trust marking

**DEGRADED vs REFUSED decision:**

```
All sources unavailable → REFUSED
Some sources unavailable → DEGRADED (if partial output is safe)
Output would require unsafe assumption → REFUSED (regardless of source availability)
```

---

## Refusal in AI Agent Systems

AI agents present a specific challenge:  
they are trained to always produce output.

The Refusal Protocol applied to AI agents means:

**The agent must refuse when:**
- Input data is missing or contradictory
- The task requires judgment beyond the agent's declared boundary
- Answering would require the agent to state something it cannot verify
- The user's intent cannot be safely interpreted

**System prompt implementation:**

```
When you cannot produce a trustworthy output:
- Do not guess
- Do not fill gaps with plausible content
- Do not produce a confident answer to an uncertain question

Instead, output a structured refusal:
  Status  : REFUSED
  Reason  : [explain what is missing or unclear]
  Action  : [state what would be needed to proceed]

Refusal is correct behavior.
A structured refusal is more useful than a confident wrong answer.
```

---

## Refusal vs. Error

These are distinct conditions and must not be conflated:

| | Refusal | Error |
|---|---|---|
| Cause | Deliberate design decision | Unexpected system failure |
| Trust signal | High — system is working as intended | Low — system has malfunctioned |
| Output | Structured refusal record | Error message or exception |
| Recovery | Await better data or clarified input | Debug and fix |
| User meaning | "The system cannot safely answer this" | "The system has broken" |

---

## What Refusal Is Not

| Misconception | Reality |
|---|---|
| Refusal means the system failed | Refusal means the system is working correctly |
| Refusal should be replaced with a best guess | Refusal is safer than an ungrounded guess |
| Refusal is a temporary state to be fixed | Refusal is permanent until trigger conditions are resolved |
| Refusal can be overridden by the user | Refusal conditions are determined by the system, not the user |

---

## Implementation Requirements

An ETB-compliant refusal implementation must:

1. **Produce a structured refusal record** — never silent failure
2. **Include a reason field** — always human-readable
3. **Propagate refusal downstream** — no partial override
4. **Distinguish REFUSED from DEGRADED** — these are not interchangeable
5. **Never substitute estimation for refusal** — estimation is not a fallback for refused data
6. **Treat refusal as a valid terminal state** — not as a problem to be solved

---

## Anti-Patterns

| Anti-Pattern | Violation |
|---|---|
| Replacing REFUSED output with "N/A" or "—" without trust label | Trust suppression |
| Estimating a value when source is REFUSED | Integrity violation |
| Logging refusal as an error | Misclassification |
| Allowing user override of refusal conditions | Boundary violation |
| Producing output with missing reason field | Incomplete refusal |
| Treating DEGRADED and REFUSED as equivalent | State collapse |

---

## Quick Reference

```
Refuse when:
  → All sources unavailable
  → Sources are inconsistent
  → Required data is missing
  → Interpretation requires unsafe assumption
  → All upstream values are REFUSED

Refusal output must include:
  → value  : null
  → trust  : REFUSED
  → reason : [human-readable explanation]

Refusal is:
  → A feature, not a failure
  → Propagating, not containable
  → Permanent until conditions resolve
  → Never overridable by estimation
```

---

*ETB Protocol — Refusal Protocol Specification v0.1*  
*Part of the Expression of Trust and Boundaries design protocol.*  
*github.com/etb-protocol/boundary-contract*
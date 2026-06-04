# State Model — ETB Protocol Specification v0.1

## Overview

An ETB-compliant system does not simply return values.  
It returns **named states** — each with explicit meaning, origin, and trust.

The state model answers three questions:

> **What is the system observing?**  
> **How did it get there?**  
> **How reliable is that observation?**

---

## The Five Observable States

### NO_POSITION

The subject has no active position.  
Risk assessment is not applicable.

**Important:**
- This does NOT mean "safe"
- It means the risk boundary is not currently engaged
- Downstream states (WATCH, BOUNDARY_APPROACHING) are not meaningful in this state

**Typical mapping:**
- Trust: `CONSISTENT`
- Origin: `NONE`

---

### STABLE

The subject is operating within a safe boundary range.

**Important:**
- STABLE is directly observable
- The system does not assert *how* STABLE was reached
- A position may have been created in STABLE, or drifted into it through external conditions

**Typical mapping:**
- Trust: `CONSISTENT`
- Origin: `OBSERVED`

---

### WATCH

The subject is approaching a boundary threshold.  
Caution is advised. Active monitoring is recommended.

**Important:**
- WATCH is empirically observable
- May be reached through direct action OR through external market/system dynamics
- The system treats WATCH as a market-driven or environment-driven reachable state

**Typical mapping:**
- Trust: `CONSISTENT`
- Origin: `MARKET` or `OBSERVED`

---

### BOUNDARY_APPROACHING

The subject is near the critical boundary.  
Intervention may be required.

**Important:**
- Semantically defined — may not be directly reachable through user action alone
- Expected to be reached primarily through post-action environmental deterioration
- The system preserves the distinction between *reachable* and *observable*

**Typical mapping:**
- Trust: `CONSISTENT`
- Origin: `MARKET`

---

### DEGRADED

Observation is possible but data quality is reduced.  
Output is produced with explicit degraded trust marking.

**Triggered by:**
- Partial source failure
- Data lag beyond acceptable threshold
- Missing upstream values

**Typical mapping:**
- Trust: `DEGRADED`
- Origin: `SYSTEM`

---

### REFUSAL

The system intentionally refuses to produce a state assessment.  
No output is generated.

**Triggered by:**
- All sources unavailable
- Sources are inconsistent
- Interpretation would require unsafe assumptions

**Typical mapping:**
- Trust: `REFUSED`
- Origin: `SYSTEM`

---

## State Origin

State origin describes **how the system interprets the path to the current state**.

This is separate from the state itself.

| Origin | Meaning |
|---|---|
| `NONE` | No active position — origin is not applicable |
| `OBSERVED` | State is directly observable — transition path not asserted |
| `MARKET` | State interpreted as reached through external dynamics |
| `SYSTEM` | State meaning depends on source or system condition |

**Key principle:**  
Origin is an interpretation, not a fact.  
The system declares its best interpretation — it does not claim certainty about causation.

---

## State Transition Model

### The Core Principle

> **The environment restricts transitions, not states.**

A subject may be prevented from directly entering a boundary-near state through action.  
But an existing position can drift into that state through external dynamics.

This leads to a critical distinction:

```
Some states are reachable through direct action.
Some states are observable only through environmental drift.
```

ETB Protocol explicitly preserves this distinction.

---

### Transition Diagram

```
                    ┌─────────────┐
                    │ NO_POSITION │
                    └──────┬──────┘
                           │ position opened
                           ▼
                    ┌─────────────┐
                    │   STABLE    │◄──────────────┐
                    └──────┬──────┘               │
                           │ boundary approached   │ recovery
                           ▼                       │
                    ┌─────────────┐               │
                    │    WATCH    │───────────────►┘
                    └──────┬──────┘
                           │ further deterioration
                           ▼
               ┌───────────────────────┐
               │  BOUNDARY_APPROACHING │
               └───────────┬───────────┘
                           │ source failure / inconsistency
                    ┌──────┴──────┐
                    │             │
                    ▼             ▼
              ┌──────────┐  ┌─────────┐
              │ DEGRADED │  │ REFUSAL │
              └──────────┘  └─────────┘
```

---

## State + Trust Matrix

Each state has an expected trust level.  
Deviations from this matrix indicate a system anomaly.

| State | Expected Trust | Expected Origin |
|---|---|---|
| `NO_POSITION` | CONSISTENT | NONE |
| `STABLE` | CONSISTENT | OBSERVED |
| `WATCH` | CONSISTENT | MARKET or OBSERVED |
| `BOUNDARY_APPROACHING` | CONSISTENT | MARKET |
| `DEGRADED` | DEGRADED | SYSTEM |
| `REFUSAL` | REFUSED | SYSTEM |

---

## Domain Applications

The state model is domain-agnostic.  
The same structure applies across systems:

### DeFi / Finance
| ETB State | Domain Meaning |
|---|---|
| NO_POSITION | No open borrow position |
| STABLE | Health factor within safe range |
| WATCH | Health factor approaching liquidation threshold |
| BOUNDARY_APPROACHING | Near-liquidation zone |
| DEGRADED | RPC node partially unavailable |
| REFUSAL | On-chain data inconsistent |

### AI Agent Systems
| ETB State | Domain Meaning |
|---|---|
| NO_POSITION | No active task in context |
| STABLE | Task within known capability boundary |
| WATCH | Task approaching uncertainty threshold |
| BOUNDARY_APPROACHING | Task requires judgment beyond safe boundary |
| DEGRADED | Context partially available |
| REFUSAL | Input insufficient or contradictory |

### Infrastructure / SRE
| ETB State | Domain Meaning |
|---|---|
| NO_POSITION | Service not under load |
| STABLE | Load within normal operating range |
| WATCH | Load approaching capacity threshold |
| BOUNDARY_APPROACHING | Near capacity limit |
| DEGRADED | Monitoring data partially unavailable |
| REFUSAL | Metrics inconsistent — cannot assess |

---

## Implementation Requirements

An ETB-compliant state model must:

1. **Name every state explicitly** — no unnamed intermediate conditions
2. **Declare state origin** — how the system interprets the path to current state
3. **Attach trust level** to every state output
4. **Preserve the reachable vs. observable distinction** — do not collapse them
5. **Treat REFUSAL as valid output** — not as an error condition
6. **Never assert causation** — only observation and interpretation

---

## Anti-Patterns

| Anti-Pattern | Violation |
|---|---|
| Collapsing WATCH and BOUNDARY_APPROACHING into one state | Loss of boundary precision |
| Asserting how a state was reached without evidence | Causation inflation |
| Treating DEGRADED as equivalent to STABLE | Trust misrepresentation |
| Suppressing REFUSAL to always show a state | Boundary violation |
| Using unnamed intermediate states | Protocol violation |

---

## Quick Reference

```
NO_POSITION          → No active subject. Risk not applicable.
STABLE               → Within safe boundary. Path not asserted.
WATCH                → Approaching boundary. Monitor actively.
BOUNDARY_APPROACHING → Near critical threshold. Intervention may be needed.
DEGRADED             → Observation possible. Quality reduced.
REFUSAL              → Output withheld. Silence over wrong answer.
```

---

*ETB Protocol — State Model Specification v0.1*  
*Part of the Expression of Trust and Boundaries design protocol.*  
*github.com/etb-protocol/boundary-contract*
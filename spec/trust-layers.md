# Trust Layers — ETB Protocol Specification v0.1

## Overview

Every output produced by an ETB-compliant system must be classified into one of four trust layers.

This classification answers a single question:

> **How reliably does this output reflect reality?**

Trust layers are not quality scores.  
They are explicit declarations of the interpretation path that produced each value.

---

## The Four Trust Layers

### VERIFIED

The value is directly retrieved from a primary source.  
No interpretation or derivation has occurred.

**Conditions:**
- Source is reachable and responsive
- Value is returned without transformation
- No secondary inference is applied

**Example:**
```
Health Factor : 1.4968    [VERIFIED]
```

**Key principle:**  
VERIFIED does not mean "correct forever".  
It means "this is what the source returned at observation time".

---

### CONSISTENT

The value is derived deterministically from one or more VERIFIED values.  
The derivation logic is explicit, repeatable, and auditable.

**Conditions:**
- All upstream values are VERIFIED or CONSISTENT
- Derivation logic is deterministic
- No estimation or approximation is introduced

**Example:**
```
Status        : WATCH      [CONSISTENT]
Liq Distance  : 49.68%     [CONSISTENT]
```

**Key principle:**  
CONSISTENT trust propagates from upstream.  
If any upstream value degrades, CONSISTENT degrades with it.

---

### ESTIMATED

The value is an approximation.  
It is useful for orientation but must not be treated as authoritative.

**Conditions:**
- Exact value cannot be retrieved or derived
- Approximation method is documented
- Label is always explicit — never implicit

**Example:**
```
Collateral USD : ~$2,402.75    [ESTIMATED]
```

**Key principle:**  
ESTIMATED values must always be labeled as such at the point of display.  
Presenting an ESTIMATED value without its label is a protocol violation.

---

### REFUSED

The system intentionally withholds output.  
No value is produced.

**Conditions (any one is sufficient):**
- Required source is unavailable
- Sources are inconsistent with each other
- Derivation would require unsafe assumptions
- Output could cause harm if wrong

**Example:**
```
Health Factor : [REFUSED] — source unavailable
Status        : [REFUSED] — cannot determine from degraded data
```

**Key principle:**  
REFUSED is not an error state.  
It is a deliberate design feature.  
Silence is safer than a confident wrong answer.

---

## Trust Propagation Rules

Trust degrades downstream. It never upgrades.

```
VERIFIED  →  can produce  →  CONSISTENT
VERIFIED  →  can produce  →  ESTIMATED
CONSISTENT → can produce  →  CONSISTENT
CONSISTENT → can produce  →  ESTIMATED
DEGRADED  →  produces     →  DEGRADED or REFUSED
REFUSED   →  produces     →  REFUSED
```

**Propagation table:**

| Upstream Trust | Derivation Type | Resulting Trust |
|---|---|---|
| VERIFIED | Deterministic | CONSISTENT |
| VERIFIED | Approximate | ESTIMATED |
| CONSISTENT | Deterministic | CONSISTENT |
| CONSISTENT | Approximate | ESTIMATED |
| DEGRADED | Any | DEGRADED |
| REFUSED | Any | REFUSED |

---

## DEGRADED — Intermediate State

DEGRADED is not a primary trust layer.  
It represents a transitional condition between CONSISTENT and REFUSED.

**Triggered when:**
- One of multiple sources is unavailable (but not all)
- Data lag exceeds acceptable threshold
- Partial data is present but incomplete

**Behavior:**
- Output may still be produced
- Trust is explicitly marked as DEGRADED
- Downstream values inherit DEGRADED

---

## Trust Layer vs. Value Layer

Trust layers describe **reliability**.  
Value layers describe **origin**.

These are separate dimensions:

| Value Layer | Meaning |
|---|---|
| RAW | Directly from source |
| DERIVED | Computed from raw values |
| ESTIMATED | Approximated |

A value can be:
- RAW + VERIFIED → direct protocol value, source confirmed
- DERIVED + CONSISTENT → computed value, logic is sound
- ESTIMATED + ESTIMATED → approximate, explicitly labeled
- DERIVED + REFUSED → computation attempted but withheld

---

## Implementation Requirements

An ETB-compliant system must:

1. **Classify every output** into a trust layer before displaying it
2. **Display trust labels** alongside values — never suppress them
3. **Propagate trust degradation** through all dependent values
4. **Refuse explicitly** when conditions for REFUSED are met
5. **Never upgrade trust** — a derived value cannot be more trusted than its source

---

## Anti-Patterns

The following are protocol violations:

| Anti-Pattern | Violation |
|---|---|
| Displaying ESTIMATED value without label | Trust misrepresentation |
| Producing output when source is unavailable | Unsafe inference |
| Treating CONSISTENT as VERIFIED | Trust inflation |
| Suppressing REFUSED state | Boundary violation |
| Filling missing data with assumptions | Integrity violation |

---

## Quick Reference

```
VERIFIED    → Source confirmed. No transformation.
CONSISTENT  → Derived deterministically. Logic is auditable.
ESTIMATED   → Approximation. Always labeled explicitly.
REFUSED     → Withheld intentionally. Silence over wrong answer.
DEGRADED    → Partial reliability. Downstream values inherit this.
```

---

*ETB Protocol — Trust Layers Specification v0.1*  
*Part of the Expression of Trust and Boundaries design protocol.*  
*github.com/etb-protocol/boundary-contract*
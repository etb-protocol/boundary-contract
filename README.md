# boundary-contract
A design protocol for expressing trust and boundaries in AI systems.
# ETB Protocol — Expression of Trust and Boundaries

**A design protocol for building AI systems that know what they don't know.**

---

## The Problem

Most AI systems are built to always produce output.

Silence feels like failure.  
Uncertainty gets smoothed over.  
Gaps get filled with confident-sounding content.

The result: agents that do the wrong thing — with conviction.

This is not a model problem.  
**It is an architecture problem.**

Your system has no boundary contract.

---

## What ETB Protocol Is

ETB (Expression of Trust and Boundaries) is a design protocol that gives AI systems — and the humans who build them — a shared language for expressing:

- **What the system knows** — and how reliably
- **What the system doesn't know** — and how to say so
- **Where the system should stop** — and refuse intentionally

> *Not what AI can do. Where AI should stop.*

---

## Core Principle

> **Refusal over Uncertainty.**  
> **Boundary over Prediction.**  
> **Observability over Automation.**

An AI system that refuses to answer when uncertain is more trustworthy than one that always answers.

ETB Protocol makes that refusal explicit, structured, and auditable.

---

## The Four Trust Layers

Every output in an ETB-compliant system is classified into one of four trust layers:

| Layer | Meaning | Example |
|---|---|---|
| `VERIFIED` | Directly confirmed from source | Protocol data, database record |
| `CONSISTENT` | Derived deterministically from verified data | Computed state, derived metric |
| `ESTIMATED` | Approximate value — explicitly labeled | Predicted value, approximation |
| `REFUSED` | Withheld intentionally — unsafe to output | Conflicting sources, missing data |

**REFUSED is not an error. It is a feature.**

---

## The State Model

Pair trust layers with observable states:

| State | Meaning |
|---|---|
| `STABLE` | Operating within safe boundaries |
| `WATCH` | Approaching a boundary — caution advised |
| `BOUNDARY_APPROACHING` | Near-limit — intervention may be needed |
| `DEGRADED` | Output possible but quality is reduced |
| `REFUSAL` | Output intentionally withheld |

These states apply across domains:

- **DeFi** → liquidation risk boundary
- **Infrastructure** → system load boundary
- **AI Agents** → judgment reliability boundary
- **Manufacturing** → defect threshold boundary
- **Autonomous Systems** → safety distance boundary

---

## Where This Came From

ETB Protocol was extracted from real operational experience.

The original implementation was built for DeFi risk observation — a system that watches on-chain positions and reports liquidation risk with explicit trust semantics.

The core question was:

> *When should a system refuse to output anything at all?*

In production financial systems, a wrong answer isn't just useless. It causes real loss.

The answer became a design protocol.

When applied to AI agent development — content automation, multi-agent pipelines, LLM orchestration — **the same boundary discipline reduced agent overreach significantly.**

The principle transferred.  
The protocol generalized.

---

## Domain Coverage

ETB Protocol applies wherever systems must make judgment calls:

| Domain | Boundary Type | Risk of Wrong Answer |
|---|---|---|
| AI / LLM Agents | Judgment reliability | Hallucination, overreach |
| DeFi / Finance | Liquidation threshold | Capital loss |
| Infrastructure / SRE | System load limit | Downtime |
| Manufacturing | Defect boundary | Quality failure |
| Autonomous Systems | Safety distance | Physical harm |
| Legal / Compliance | Regulatory boundary | Liability |

**Common structure across all domains:**

```
Signal (Raw)
  ↓
State (Derived)
  ↓
Trust (Verified / Estimated / Refused)
  ↓
Boundary (Where the system stops)
```

---

## What This Repository Contains

```
boundary-contract/
├── spec/
│   ├── trust-layers.md          # Trust layer specification
│   ├── state-model.md           # State model and transition rules
│   ├── refusal-protocol.md      # When and how to refuse
│   └── non-advisory-clause.md   # What the system must never do
├── templates/
│   ├── agent-system-prompt.md   # System prompt templates
│   ├── multi-agent.md           # Multi-agent boundary design
│   └── domain-examples.md       # Domain-specific implementations
├── examples/
│   ├── defi-observer/           # Aave v3 risk observer
│   ├── infra-observer/          # Infrastructure load observer
│   └── ai-agent/                # LLM agent boundary example
└── README.md
```

---

## Quick Start — System Prompt Template

Add this to any AI agent to implement a minimal boundary contract:

```
You are an observer agent. Your role is to report state, not to act.

For every output, classify it as one of:
- VERIFIED: directly confirmed from source
- CONSISTENT: derived from verified data
- ESTIMATED: approximate — label it explicitly
- REFUSED: do not output if data is missing, inconsistent, or unsafe

Rules:
- Never fill gaps with assumptions
- Never produce output when sources conflict
- Never optimize, advise, or act — only observe and report
- When in doubt, refuse

Refusal is correct behavior.
Silence is safer than a confident wrong answer.
```

---

## Commercial Packages

Full specification documents are available for purchase:

| Package | Contents | Price |
|---|---|---|
| **Boundary Contract v0.1** | Trust layer spec + State model + Refusal protocol + System prompt templates | $19 |
| **ETB Agent Design Kit** | Full spec + Multi-agent patterns + Domain examples + Implementation guide | $49 |
| **ETB Enterprise License** | All above + Custom domain adaptation + Integration consultation | Contact |

→ Available on [Gumroad](#) *(link coming soon)*

---

## Origin & Related Work

ETB Protocol is part of the broader UEH (Universal Exchange Adapters) design philosophy.

- **UEH Observer** — DeFi risk observation infrastructure: [github.com/ueh-labs/ueh-observer](https://github.com/ueh-labs/ueh-observer)
- **UEH Labs** — Exchange connectivity infrastructure: [github.com/ueh-labs](https://github.com/ueh-labs)

ETB Protocol extracts the boundary discipline from UEH and makes it applicable to AI systems and beyond.

---

## Contact

- General: [etb.protocol@gmail.com](#) *(update with actual address)*
- Enterprise: [etb.enterprise@gmail.com](#) *(update with actual address)*
- X: [@arcthree_lab](https://x.com/arcthree_lab)

---

## License

MIT License — see [LICENSE](./LICENSE)

The specification documents in `/spec` are additionally available under commercial license.  
See commercial packages above.

---

*ETB Protocol — Because the boundary between knowing and not knowing is where trust is built.*

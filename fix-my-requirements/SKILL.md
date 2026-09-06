---
name: fix-my-requirements
description: >
  Use when a PRD, RFC, design document, issue, or implementation plan may
  contain unnecessary, over-specified, inherited, coupled, or
  solution-disguised requirements. Recover the actual job, extract explicit
  and implicit requirements, test authority and deletion counterfactuals,
  and recommend KEEP, DELETE, RELAX, REWRITE, or UNKNOWN before implementation.
  Read-only diagnosis only.
license: CC-BY-4.0
metadata:
  author: Gaurav Albal
  version: "0.1.0"
---

# Fix My Requirements

## Moment

A requirement-bearing artifact may be making implementation larger than the actual job warrants.

## Governing question

Which requirements actually deserve to survive before implementation effort is spent on them?

## Required references

Read and obey these sibling files in order:

1. `REQUIREMENT_LAW.md` — authority, counterfactual, force, disposition, coupling, and fail-closed law.
2. `PROTOCOL.md` — complete bounded analysis procedure.
3. `SIT.md` — typed output contract.
4. `RUN_PROMPT.md` — standalone execution constraints and read-only boundary.

## Non-negotiable order

```text
OPERATOR ASK + SOURCE BOUNDARY
→ JOB + MATERIAL INVARIANTS + AUTHORITIES
→ EXPLICIT + IMPLIED REQUIREMENT REGISTER
→ AUTHORITY / DELETE / RELAX / SOLUTION TEST
→ INDIVIDUAL DISPOSITIONS
→ COUPLING PASS
→ SURVIVING SET + FIRST MOVES
→ DIRECT ANSWER
```

Absence is not deletion evidence. Preserve current authority separately from recommendation. Every `DELETE` needs a concrete counterfactual. Preserve normative force. `UNKNOWN` is a complete result. Do not let current implementation justify itself. Route a question about whether the requirements or constraints driving a design are necessary here; route a question that assumes a requirement/job and asks whether a specific cache, service, state machine, or other component is needed to [`simplify`](https://github.com/GauravAlbal/agent-skills/tree/main/simplify). A component noun alone does not activate this skill. Route one evidence claim or one test's proof status to [`proof`](https://github.com/GauravAlbal/agent-skills/tree/main/proof); route a broken, hollow, flaky, or broadly inadequate test suite to [`Fix My Tests`](https://github.com/GauravAlbal/fix-my-tests). Do not turn this into architecture review, Fix My Tests, `proof`, writing, or implementation minimization.

## Exit

Fill `SIT.md`, answer the operator's exact question at an explicit evidence boundary, return at most five first moves with exactly one first move, and stop. Do not modify the source artifact or repository. Do not implement, rewrite, approve, or silently mutate requirements.

# Fix My Requirements — Sitting Output Contract

This file defines the one bounded output returned by a Fix My Requirements sitting. It is a **typed, copyable contract**, not a second analysis method. Apply `REQUIREMENT_LAW.md` and `PROTOCOL.md` before filling it.

## Contract rules

1. Return every top-level section in the order shown below. Do not replace the register with an essay.
2. Preserve the operator's question and the examined-source boundary before analysis. A broader concern MAY be noted, but MUST NOT replace the ask.
3. Use stable local requirement IDs (`R-01`, `R-02`, …) and source pointers exactly enough for a human or downstream planner to join every finding back to the source.
4. Extract explicit and materially implied requirements. Do not invent an implied requirement merely to populate a field.
5. Every material candidate appears once in the requirement register and receives exactly one disposition: `KEEP`, `DELETE`, `RELAX`, `REWRITE`, or `UNKNOWN`.
6. Preserve current authority separately from the review recommendation. A recommendation is advisory and does not change the source.
7. A `DELETE` disposition MUST include a concrete counterfactual showing the named job and material invariants remain intact. Missing rationale, absent owner, or unused current code is not that counterfactual.
8. A force, threshold, scope, timing, durability, precision, generality, or universality change MUST be visible as `RELAX` or `REWRITE`; never apply one silently.
9. If evidence is incomplete, unavailable, coupled, or dependent on an unresolved assumption, use `UNKNOWN` and name the boundary.
10. A truncated sitting MUST report the cap, examined candidates, and deferred tail. It MUST NOT present the deferred tail as reviewed.
11. Return no more than five ordered first moves and exactly one designated first move. First moves are recommendations for investigation or operator action, not implementation work.
12. The sitting is read-only diagnosis. It MUST NOT edit the source artifact or repository, approve its own recommendations, rewrite the source, or begin implementation.

## Output shape

Copy the following YAML shape for machine-readable output. Replace placeholders only with observed, source-grounded content. Quoted placeholders are intentionally valid YAML strings; `null` and `[]` represent unavailable or empty values, not permission to guess.

```yaml
contract: "fix-my-requirements.sitting.v1"
title: "FIX MY REQUIREMENTS"
status:
  mode: "READ_ONLY_DIAGNOSIS"
  authority: "ADVISORY_NON_AUTHORITATIVE"
  source_mutation: "NOT_PERFORMED" # If this cannot be established, use "UNKNOWN" and explain in notes.
  implementation_started: "NOT_PERFORMED" # If this cannot be established, use "UNKNOWN" and explain in notes.
  notes: "<observed status note, or null>"

operator:
  ask: "<verbatim or minimally reconstructed operator question>"
  source_scope: "<named artifact, file/document, revision, or explicitly supplied material>"
  status: "OPEN | ANSWERED | PARTIALLY_ANSWERED"

evidence_boundary:
  examined_materials:
    - "<source pointer, artifact, or supplied excerpt actually examined>"
  source_revision: "<revision/version/date if supplied; otherwise null>"
  excluded_or_unavailable:
    - "<material not examined or unavailable, and why; use [] when none is known>"
  authority_boundary: "<authorities actually available to this sitting; do not infer unavailable authority>"
  factual_uncertainties:
    - "<unresolved fact that can affect a disposition; use [] when none is known>"

job:
  statement: "<one short statement of the intended user/system outcome>"
  material_invariants:
    - "<must-not-break property, interface, user need, or other material invariant>"
  authorities:
    - "<named authority or null when unavailable>"
  source_boundary: "<what this job interpretation is limited to>"
  unresolved_basis:
    - "<basis needed to judge the job but not available; use [] when none is known>"

requirement_register:
  - id: "R-01"
    text: "<requirement text as stated or materially implied>"
    source_pointer: "<stable source locator: heading/section/line/quote or explicit implied-location note>"
    source_basis: "EXPLICIT | INFERRED | EXTERNAL | UNAVAILABLE"
    force: "MUST | MUST_NOT | SHOULD | SHOULD_NOT | MAY | UNSPECIFIED"
    current_authority: "<who or what currently makes this binding, or UNKNOWN>"
    kind: "OUTCOME | CONSTRAINT | IMPLEMENTATION_CHOICE | ASSUMPTION | MIXED | UNKNOWN"

interrogation:
  - requirement_id: "R-01"
    authority_question: "<who or what currently makes this binding?>"
    contract_surface:
      kind: "HUMAN_POLICY | MACHINE_CONSUMED | UNKNOWN"
      consumers:
        - "<named consumer or system that reads this surface; use [] when none is known>"
      consumer_closure:
        status: "ESTABLISHED | INCOMPLETE | UNKNOWN"
        basis: "<authoritative weaker-contract permission, or per-consumer preservation evidence; null when not established>"
    deletion_counterfactual: "<specific material outcome, invariant, interface, authority, or user need that fails if removed; state none only when established>"
    relaxation_counterfactual: "<weaker force/scope/threshold/timing/precision and why it still serves the job; use null when not supportable or not applicable>"
    requirement_vs_solution: "OUTCOME_OR_CONSTRAINT | SOLUTION_CHOICE | MIXED | UNKNOWN"
    unresolved_questions:
      - "<question that prevents a stronger conclusion; use [] when none is known>"

# Each material requirement appears exactly once here, under exactly one label.
dispositions:
  KEEP:
    - requirement_id: "R-01"
      current_authority: "<current binding basis, preserved separately from this recommendation>"
      recommendation: "KEEP"
      concrete_break_if_deleted: "<specific material failure if removed; do not write merely 'important'>"
      proposed_survivor: "<source-joinable surviving obligation, or null>"
      basis: "<source-grounded reason for KEEP>"
      source_refs:
        - "<source pointer(s)>"
      confidence_boundary: "<what this conclusion does and does not establish>"
  DELETE:
    - requirement_id: "R-02"
      current_authority: "<current binding basis, or UNKNOWN>"
      recommendation: "DELETE"
      concrete_break_if_deleted: "<counterfactual showing the named job/invariants remain intact after complete removal>"
      proposed_survivor: null
      basis: "<bounded evidence supporting deletion; never absence of rationale alone>"
      source_refs:
        - "<source pointer(s)>"
      confidence_boundary: "<authority, scope, and factual limits of this deletion recommendation>"
  RELAX:
    - requirement_id: "R-03"
      current_authority: "<current binding basis, preserved separately from this recommendation>"
      recommendation: "RELAX"
      concrete_break_if_deleted: "<served invariant/outcome that fails if removed completely; from the break search>"
      proposed_survivor: "<weaker concrete force/scope/threshold/timing/precision that preserves the job>"
      sufficiency_witness: "<positive source-grounded reason each served invariant survives under the weaker form; UNKNOWN when not established>"
      basis: "<why the obligation survives and why the proposed weaker form is sufficient>"
      source_refs:
        - "<source pointer(s)>"
      confidence_boundary: "<what is established and what remains unresolved>"
  REWRITE:
    - requirement_id: "R-04"
      current_authority: "<current binding basis, preserved separately from this recommendation>"
      recommendation: "REWRITE"
      concrete_break_if_deleted: "<material outcome that must still be preserved after removing the mechanism-mixed wording>"
      proposed_survivor: "<recoverable outcome/constraint without an invented mechanism>"
      basis: "<why the current text substitutes a mechanism or mixes obligations>"
      source_refs:
        - "<source pointer(s)>"
      confidence_boundary: "<what the source supports and what cannot be recovered>"
  UNKNOWN:
    - requirement_id: "R-05"
      current_authority: "<current binding basis, or UNKNOWN>"
      recommendation: "UNKNOWN"
      concrete_break_if_deleted: "<unresolved; explain which deletion consequence cannot be established>"
      proposed_survivor: "<null or conservatively preserved form, clearly marked as non-final>"
      basis: "<specific missing authority, scope, fact, coupling, or recoverable outcome>"
      source_refs:
        - "<source pointer(s)>"
      confidence_boundary: "<exact uncertainty that blocks KEEP, DELETE, RELAX, or REWRITE>"

coupled_requirements:
  - cluster_id: "C-01"
    requirement_ids:
      - "R-01"
      - "R-02"
    why_individually_load_bearing: "<why each appears to carry material weight when read alone>"
    joint_counterfactual: "<what happens when the cluster is removed or weakened together>"
    recommendation: "KEEP_AS_SET | DELETE_AS_SET | RELAX_AS_SET | UNKNOWN"
    basis: "<evidence for the joint recommendation; identify unresolved interaction when UNKNOWN>"
    source_refs:
      - "<source pointer(s)>"
    confidence_boundary: "<scope and uncertainty of the cluster finding>"

proposed_surviving_requirement_set:
  authority: "REVIEW_PROPOSAL_ONLY"
  join_key: "requirement_id + source_pointer"
  entries:
    - requirement_id: "R-01"
      source_pointer: "<same stable pointer used in requirement_register>"
      source_text: "<original requirement text, or null if implied/unavailable>"
      proposed_text: "<surviving source-joinable obligation; preserve force unless RELAX/REWRITE says otherwise>"
      disposition: "KEEP | RELAX | REWRITE | UNKNOWN"
      force: "MUST | MUST_NOT | SHOULD | SHOULD_NOT | MAY | UNSPECIFIED"
      pending_reason: "<why this remains pending when disposition is UNKNOWN; otherwise null>"
  omitted_deleted_ids:
    - "<R-ID recommended DELETE; use [] when none>"
  set_boundary: "<what this proposed set does not decide; it is not an approved source mutation>"

truncation:
  status: "FULL | TRUNCATED"
  candidate_cap: "uncapped"
  material_candidate_count: "<integer or null when not determinable>"
  examined_candidate_count: "<integer>"
  deferred_candidate_count: "<integer; 0 when FULL>"
  deferred_tail:
    - candidate_id_or_pointer: "<stable ID/pointer for a deferred candidate>"
      reason_deferred: "<why it was not interrogated>"
      expected_materiality: "HIGH | MEDIUM | LOW | UNKNOWN"
  deferral_reason: "<cap/boundary reason; null when FULL>"
  continuation_boundary: "<what must be examined to complete the sitting; null when FULL>"

first_moves:
  - move_id: "M-01"
    order: 1
    action: "<bounded investigation or operator decision, not implementation>"
    target_requirement_ids:
      - "<R-ID or []>"
    expected_evidence: "<evidence that would resolve or advance the action>"
    dependency: "<required prior fact or null>"

first_move:
  move_id: "M-01"
  rationale: "<why this is the single highest-leverage next move>"
  exactly_one: true

answer_to_operator:
  direct_answer: "<plain, direct answer to the original operator ask at the stated evidence boundary>"
  limitations: "<material limitation or 'none identified'>"
```

## Typed field requirements

### 1. Header, ask, and evidence boundary

| Path | Type | Required meaning |
|---|---|---|
| `contract` | literal string | Must be `fix-my-requirements.sitting.v1`. |
| `status.mode` | enum | Must be `READ_ONLY_DIAGNOSIS`. |
| `status.authority` | literal string | Must be `ADVISORY_NON_AUTHORITATIVE`; the sitting recommends but does not approve or mutate. |
| `status.source_mutation` | enum | `NOT_PERFORMED` only when observed; otherwise `UNKNOWN` with an explanation. |
| `status.implementation_started` | enum | `NOT_PERFORMED` only when observed; otherwise `UNKNOWN` with an explanation. |
| `operator.ask` | non-empty string | Verbatim or minimally reconstructed original question. |
| `operator.source_scope` | non-empty string | Named source material and scope. |
| `evidence_boundary.examined_materials` | list of strings | Material actually examined; never imply broader inspection. |
| `evidence_boundary.excluded_or_unavailable` | list of strings | Missing or excluded material, including unavailable external authority. |

### 2. Job and requirement register

| Path | Type | Required meaning |
|---|---|---|
| `job.statement` | non-empty string | One short intended outcome, not a proposed implementation. |
| `job.material_invariants` | list of strings | Material must-not-break properties identified from the bounded source. |
| `job.authorities` | list of strings | Named authorities only; unavailable authority is not positive evidence. |
| `requirement_register[].id` | unique string | Stable local join key, normally `R-NN`. |
| `requirement_register[].text` | non-empty string | Explicit requirement or materially implied requirement text. |
| `requirement_register[].source_pointer` | non-empty string | Stable pointer into the bounded source; implied candidates explain the derivation location. |
| `requirement_register[].source_basis` | enum | Exactly one of `EXPLICIT`, `INFERRED`, `EXTERNAL`, `UNAVAILABLE`. |
| `requirement_register[].force` | enum | Preserve the source's normative force; use `UNSPECIFIED` when not stated. |
| `requirement_register[].current_authority` | string | Current binding basis, including `UNKNOWN` when unavailable. |
| `requirement_register[].kind` | enum | Exactly one of `OUTCOME`, `CONSTRAINT`, `IMPLEMENTATION_CHOICE`, `ASSUMPTION`, `MIXED`, `UNKNOWN`. |

### 3. Interrogation and dispositions

`interrogation[]` SHOULD have one entry for each register ID. It records the questions that support, limit, or block the disposition; it MUST NOT silently replace the register.

`dispositions` MUST contain all five label keys, even when a label has no entries (`[]`). Across the five lists, every material register ID appears exactly once. Each entry MUST contain the law's required fields: `requirement_id`, `current_authority`, `recommendation`, `concrete_break_if_deleted`, `proposed_survivor`, `basis`, `source_refs`, and `confidence_boundary`. Every `RELAX` entry MUST additionally carry `sufficiency_witness`: a positive source-grounded reason why each served invariant named in `concrete_break_if_deleted` survives under the weaker `proposed_survivor`. When that witness cannot be established, the disposition MUST be `UNKNOWN` with the missing sufficiency evidence named.

| Label | Type meaning | Minimum distinction |
|---|---|---|
| `KEEP` | obligation survives as written | Positive authority or a concrete material deletion failure establishes survival. |
| `DELETE` | remove the obligation | A bounded counterfactual establishes that the named job and invariants survive; unresolved authority/coupling is absent. For a `MACHINE_CONSUMED` contract surface, forbidden unless consumer closure is `ESTABLISHED` (§3.0). |
| `RELAX` | obligation survives in weaker form | Name the concrete weaker force, scope, threshold, timing, or precision, name each served invariant, and give the positive source-grounded witness that the weaker form preserves it; otherwise `UNKNOWN`. For a `MACHINE_CONSUMED` contract surface, forbidden unless consumer closure is `ESTABLISHED` (§3.0). |
| `REWRITE` | recover outcome from mechanism/mixed text | Preserve the recoverable outcome without inventing a mechanism or meaning. |
| `UNKNOWN` | no safe stronger conclusion | Name the missing authority, scope, fact, coupling, or recoverable outcome. |

### 4. Coupling, survivor set, and boundedness

| Path | Type | Required meaning |
|---|---|---|
| `coupled_requirements[]` | list | Include every material coupled cluster found after individual dispositions; use `[]` when none is established. |
| `coupled_requirements[].cluster_id` | unique string | Stable local cluster key, normally `C-NN`. |
| `coupled_requirements[].requirement_ids` | list of register IDs | Exact members of the cluster. |
| `coupled_requirements[].why_individually_load_bearing` | non-empty string | Individual load-bearing explanations. |
| `coupled_requirements[].joint_counterfactual` | non-empty string | Joint delete/weaken thought experiment and its consequence. |
| `coupled_requirements[].recommendation` | enum | Exactly one of `KEEP_AS_SET`, `DELETE_AS_SET`, `RELAX_AS_SET`, `UNKNOWN`. |
| `proposed_surviving_requirement_set.entries[]` | list | Review proposal only; each entry retains `requirement_id` and the exact `source_pointer` join key. |
| `proposed_surviving_requirement_set.authority` | literal string | Must remain `REVIEW_PROPOSAL_ONLY`. |
| `truncation.status` | enum | `FULL` only when the declared candidate set was interrogated; otherwise `TRUNCATED`. |
| `truncation.candidate_cap` | literal | v0.1.0 runtime bound: `uncapped`. Do not select 8 or 16. |
| `truncation.deferred_tail` | list | Every deferred candidate or pointer and its reason; empty only when `FULL`. |

If `truncation.status` is `TRUNCATED`, the direct answer and survivor set MUST be scoped as partial, and `continuation_boundary` MUST identify the deferred work. If `FULL`, `deferred_candidate_count` MUST be `0`, `deferred_tail` MUST be `[]`, and deferral fields MUST be `null`.

### 5. First moves and answer

`first_moves` is an ordered list of **1–5** distinct move objects. `first_move` is a required singular pointer to exactly one member of that list, and its `move_id` MUST equal the list item with `order: 1`; `exactly_one` MUST be `true`. No move may claim to implement, rewrite, approve, or mutate the source.

`answer_to_operator.direct_answer` MUST answer `operator.ask` directly, state what deserves to survive or remain unresolved, and respect `evidence_boundary` and `truncation`. `limitations` MUST disclose any material limitation rather than smoothing it over.

## Markdown rendering template

Use this rendering when the operator needs a human-readable sitting. Keep the labels and fields; do not collapse them into a general conclusion.

```markdown
# FIX MY REQUIREMENTS

## OPERATOR QUESTION
- Ask: <verbatim or minimally reconstructed question>
- Source scope: <named bounded source>

## EVIDENCE BOUNDARY
- Examined: <materials actually examined>
- Excluded/unavailable: <missing material or `none identified`>
- Authority boundary: <available authority only>
- Analysis status: <FULL or TRUNCATED; cap and counts>

## JOB
<one short statement>

### Material invariants
| Invariant | Source pointer / basis |
|---|---|
| <invariant> | <pointer or `UNAVAILABLE`> |

## REQUIREMENT REGISTER
| ID | Requirement | Source pointer | Basis | Force | Current authority | Kind |
|---|---|---|---|---|---|---|
| R-01 | <text> | <pointer> | EXPLICIT/INFERRED/EXTERNAL/UNAVAILABLE | MUST/MUST_NOT/SHOULD/SHOULD_NOT/MAY/UNSPECIFIED | <authority or UNKNOWN> | OUTCOME/CONSTRAINT/IMPLEMENTATION_CHOICE/ASSUMPTION/MIXED/UNKNOWN |

## DISPOSITIONS

### KEEP
| Requirement ID | Current authority | Concrete break if deleted | Proposed survivor | Basis | Source refs | Confidence boundary |
|---|---|---|---|---|---|---|
| <R-ID or `none`> | <value> | <specific failure or `none`> | <value> | <basis> | <refs> | <boundary> |

### DELETE
| Requirement ID | Current authority | Concrete break if deleted (counterfactual) | Proposed survivor | Basis | Source refs | Confidence boundary |
|---|---|---|---|---|---|---|
| <R-ID or `none`> | <value> | <specific preservation counterfactual> | `null` | <basis> | <refs> | <boundary> |

### RELAX
| Requirement ID | Current authority | Concrete break if deleted | Proposed survivor (weaker form) | Basis | Source refs | Confidence boundary |
|---|---|---|---|---|---|---|
| <R-ID or `none`> | <value> | <surviving obligation> | <weaker concrete form> | <basis> | <refs> | <boundary> |

### REWRITE
| Requirement ID | Current authority | Concrete break if deleted | Proposed survivor (outcome/constraint) | Basis | Source refs | Confidence boundary |
|---|---|---|---|---|---|---|
| <R-ID or `none`> | <value> | <outcome that remains> | <recoverable wording> | <basis> | <refs> | <boundary> |

### UNKNOWN
| Requirement ID | Current authority | Concrete break if deleted | Proposed survivor | Basis for uncertainty | Source refs | Confidence boundary |
|---|---|---|---|---|---|---|
| <R-ID or `none`> | <value> | <unresolved consequence> | <null or pending form> | <specific missing basis> | <refs> | <boundary> |

## COUPLED REQUIREMENTS
| Cluster ID | Requirement IDs | Why individually load-bearing | Joint counterfactual | Recommendation | Basis | Source refs | Confidence boundary |
|---|---|---|---|---|---|---|---|
| <C-ID or `none`> | <R-IDs> | <explanation> | <joint thought experiment> | KEEP_AS_SET/DELETE_AS_SET/RELAX_AS_SET/UNKNOWN | <basis> | <refs> | <boundary> |

## PROPOSED SURVIVING REQUIREMENT SET
**Authority:** REVIEW PROPOSAL ONLY; not an approved source change.

| Requirement ID | Source pointer (join key) | Source text | Proposed text | Disposition | Force | Pending reason |
|---|---|---|---|---|---|---|
| <R-ID> | <exact register pointer> | <source text or `null`> | <surviving obligation> | KEEP/RELAX/REWRITE/UNKNOWN | <preserved or explicitly changed force> | <reason or `null`> |

Omitted DELETE IDs: <R-IDs or `none`>
Set boundary: <what remains unresolved>

## TRUNCATION / DEFERRED TAIL
- Status: <FULL or TRUNCATED>
- Candidate cap: uncapped
- Material candidates: <count or `unknown`>
- Examined: <count>
- Deferred: <count>
- Deferred tail: <candidate IDs/pointers and reasons, or `none`>
- Continuation boundary: <what must be examined next, or `none`>

## FIRST MOVES
| Order | Move ID | Action (bounded/read-only) | Target IDs | Expected evidence | Dependency |
|---|---|---|---|---|---|
| 1 | M-01 | <single highest-leverage move> | <R-IDs or `none`> | <evidence> | <dependency or `none`> |
| 2 | M-02 | <optional move> | <R-IDs or `none`> | <evidence> | <dependency or `none`> |
| <3–5> | <M-ID> | <optional move> | <R-IDs or `none`> | <evidence> | <dependency or `none`> |

**FIRST MOVE (exactly one):** M-01 — <why this is first>.

## ANSWER TO OPERATOR
<direct answer to the original ask, bounded by the evidence above>

## STATUS
- Mode: READ_ONLY_DIAGNOSIS
- Authority: ADVISORY_NON_AUTHORITATIVE
- Source mutation: NOT_PERFORMED (or UNKNOWN with explanation)
- Implementation started: NOT_PERFORMED (or UNKNOWN with explanation)
- Limitations: <material limitations>
```

The Markdown table rows are templates: omit optional move rows rather than emitting more than five. Keep an explicit `none` row for an empty disposition or coupling label so all five labels remain visible.

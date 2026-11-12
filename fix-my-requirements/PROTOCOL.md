# Fix My Requirements — bounded diagnosis protocol

**Purpose:** determine which obligations deserve to survive before implementation effort is spent. This is a read-only diagnosis procedure, not a requirements writer, architecture review, test review, or implementation planner.

This procedure is governed by [`REQUIREMENT_LAW.md`](REQUIREMENT_LAW.md). The law wins whenever this procedure is abbreviated or extracted into another prompt. In particular: absence is not deletion evidence; authority and recommendation remain separate; deletion requires a concrete counterfactual; implementation is not authority; normative force is preserved; `UNKNOWN` is complete; and the skill advises only.

## Operating contract

- Analyze only the operator's declared source boundary and evidence explicitly available within that boundary.
- Preserve the operator's ask, source identity, revision, and evidence boundary before interpreting content.
- Extract both explicit and materially implied requirements. Do not invent implications to fill a register.
- Give every material candidate one stable local ID and one final disposition: `KEEP`, `DELETE`, `RELAX`, `REWRITE`, or `UNKNOWN`.
- Never silently change source text, normative force, authority, or implementation. Recommendations are not mutations or approvals.
- Return one bounded sitting in the shape required by [`SIT.md`](SIT.md), then stop.

## Phase 0 — bind the ask and evidence boundary

Create the run header before reading for conclusions:

```yaml
operator:
  ask: <verbatim ask, or minimally reconstructed without broadening it>
  source_scope: <named artifact, file/document revision, or explicitly supplied material>
  status: OPEN
analysis:
  evidence_examined: []
  evidence_not_examined: []
  boundary_limits: []
```

1. Record the operator's exact question. A broader concern may be noted, but never replaces the ask.
2. Record each source item actually examined, including its identity/revision and stable locator. Record inaccessible, omitted, or out-of-bound material separately.
3. Do not silently search a repository, web, implementation, contract, or policy outside `source_scope`. If the operator expands the boundary, append the expansion and its reason.
4. If the supplied material cannot support a material judgment, continue only by recording the limitation; affected candidates must fail closed to `UNKNOWN`.

## Phase 1 — recover the job and authority boundary

Recover only the context needed to interpret requirements. Do not perform general architecture review.

Identify, with source pointers where available:

- intended user/system outcome;
- material invariants and must-not-break properties;
- explicit non-goals;
- external interfaces and contracts;
- named quantitative bounds;
- named authorities;
- known compatibility obligations; and
- material uncertainty.

Emit:

```yaml
job:
  statement: <one short outcome statement>
  material_invariants: []
  authorities: []
  source_boundary: <same bound as Phase 0>
  unresolved_basis: []
```

For each authority, distinguish the authority itself from evidence that the source mentions it. Do not promote current code, tooling, topology, or convention into authority. A plausible but unavailable contract, law, policy, customer promise, or compatibility obligation is unresolved and will force `UNKNOWN` where material.

## Phase 2 — extract the requirement register

Read the complete bounded source for requirement-bearing material, including prose, tables, examples, interface statements, non-goals, quantitative targets, and assumptions. Extract:

- **explicit** requirements stated as obligations, constraints, targets, prohibitions, or permissions; and
- **materially implied** requirements needed to make the stated job, interface, authority, or invariant coherent.

Do not extract every sentence. Do not omit explicit requirements merely because they look sensible, familiar, or difficult to challenge. Do not infer a requirement merely because an implementation happens to use it.

Assign IDs in first-encounter order (`R-01`, `R-02`, …) and never renumber them during later analysis. Every candidate record contains:

```yaml
requirement:
  id: R-01
  text: <source wording, or precise statement of an implied obligation>
  source_pointer: <stable section/page/line/paragraph/table/quote locator>
  source_basis: EXPLICIT | INFERRED | EXTERNAL | UNAVAILABLE
  force: MUST | MUST_NOT | SHOULD | SHOULD_NOT | MAY | UNSPECIFIED
  current_authority: <what currently binds it, if known>
  kind: OUTCOME | CONSTRAINT | IMPLEMENTATION_CHOICE | ASSUMPTION | MIXED | UNKNOWN
```

For an implied candidate, point to the source spans that require or support the implication and state the derivation in `basis`; do not present it as an explicit quotation. If no stable pointer or basis can be recovered, use `UNAVAILABLE` and expect `UNKNOWN`.

## Phase 3 — interrogate each candidate

For every material candidate interrogated at runtime, answer all four questions below. Runtime `candidate_cap` is uncapped; 8 and 16 are evaluation arms only, not a selection bound. Use concrete named effects, not taste, familiarity, or “seems important.” Keep `current_authority` (what binds the source today) separate from the eventual recommendation (what should survive).

### 3.0 Contract-surface classification

Before the counterfactual questions, classify every material candidate that defines a typed surface another system reads — a schema field, wire format, receipt, API/ABI, persistence format, protocol message, or enumerated machine-readable state:

```yaml
contract_surface:
  kind: HUMAN_POLICY | MACHINE_CONSUMED | UNKNOWN
  consumers: []
  consumer_closure:
    status: ESTABLISHED | INCOMPLETE | UNKNOWN
```

`MACHINE_CONSUMED` applies when the requirement defines fields, messages, states, or formats that another system, client, tool, or process reads. `HUMAN_POLICY` applies to guidance and preference expressed for people. When the class cannot be determined from the bounded source, use `UNKNOWN` and do not treat it as `HUMAN_POLICY`.

The disposition rule is mechanical for `MACHINE_CONSUMED`:

```text
MACHINE_CONSUMED
 proposed DELETE/RELAX
 consumer_closure != ESTABLISHED
        ↓
DELETE/RELAX forbidden
        ↓
KEEP or UNKNOWN
```

Consumer closure is established only by (A) an authoritative source explicitly permitting the weaker contract, or (B) identification of every material known consumer with positive evidence the proposed survivor preserves each consumer's required behavior. "Seems overkill", "probably unnecessary", "no obvious consumer", or "could simplify" never establish closure.

### 3.1 Ownership and authority

Answer:

> Who or what currently makes this binding?

Classify the current binding basis using the authority taxonomy in [`REQUIREMENT_LAW.md`](REQUIREMENT_LAW.md) (Authority law). That section is the sole owner of the labels; do not duplicate the list here. Record one or more of those kinds with a source pointer and confidence boundary.

A missing owner or rationale is not evidence for deletion. Existing implementation can show consequence or migration cost, but cannot circularly establish that its own mechanism is required.

### 3.2 Delete counterfactual

Ask:

> Delete this requirement completely. Walk the intended system forward. What specific material outcome, invariant, interface, authority, or user need now fails?

Record either the concrete break and its source/evidence pointer, or the bounded result that no break was established. “Nobody asked for it,” “nothing says why,” and “the code does not use it” are not concrete counterfactuals. If a plausible break, authority, factual premise, or interacting requirement is unresolved, do not convert that absence into `DELETE`; fail closed.

### 3.3 Relaxation counterfactual — two-sided burden for RELAX decisions only

This section decides only whether `RELAX` is authorized. It does not govern §3.2 `DELETE` or §3.4 `REWRITE`: those tracks have their own laws and MUST NOT freeze to `UNKNOWN` merely because the relaxation witness is absent. A mechanism-bearing requirement whose outcome is recoverable is a `REWRITE` candidate under §3.4 even when no weaker-survivor witness exists. Discriminator: when the text names a mechanism or implementation choice that could be swapped for another, apply `REWRITE`; when the text is itself a real obligation with an inflated force, scope, threshold, timing, or precision and the bounded source supplies the weaker survivor, apply `RELAX`. An inflated threshold is not a mechanism.

When the candidate is deciding between `RELAX` and another label, `RELAX` is authorized only when BOTH sides are established from the bounded source:

1. BREAK SEARCH
   > Delete this requirement completely. Walk the intended system forward. What specific material outcome, invariant, interface, authority, or user need now fails?

   Name the concrete break and what it protects. This identifies the served invariants the weaker survivor must preserve.
2. SURVIVOR WITNESS
   > Can force, threshold, scope, timing, durability, precision, generality, or universality be weakened without losing the actual job?

   State the concrete weaker form, then give a positive source-grounded reason why each served invariant named in the break search remains satisfied under the weaker survivor.

A weaker survivor asserted to be "sufficient" or to "preserve auditability" without naming each served invariant and the source-grounded reason it survives is NOT a sufficiency witness. Examples include `50 ms → 500 ms`, `global → regional`, `exactly once → at-least-once + idempotency`, `MUST → SHOULD`, `all callers → external callers`, and `permanent durability → bounded recovery window`. Never weaken by intuition alone. When the sufficiency of the weaker survivor cannot be established from the bounded source, `UNKNOWN` beats `RELAX` (see §3.4 first: a recoverable outcome may still justify `REWRITE`). Any change to `MUST`, `MUST_NOT`, `SHOULD`, `SHOULD_NOT`, `MAY`, target, or preference must be explicit in the recommendation; it is never an unannounced rewrite.

### 3.4 Requirement-versus-solution test

Ask:

> Is this an outcome or constraint that must hold, or one proposed mechanism for making it hold?

If the source supports an underlying outcome independently of the mechanism, mark the mechanism as a `REWRITE` candidate and state the recoverable outcome. If the underlying outcome cannot be recovered without guessing, use `UNKNOWN`. Do not let current implementation or an inherited architecture justify a solution-disguised requirement.

For each candidate, retain an interrogation record:

```yaml
interrogation:
  requirement_id: R-01
  ownership_basis: <label + concrete evidence or UNKNOWN>
  deletion_counterfactual: <specific break, or no established break>
  relaxation_counterfactual: <weaker form + sufficiency, or not supportable>
  solution_test: <outcome/constraint | mechanism | mixed | unresolved>
  evidence_refs: []
  unresolved: []
```

## Phase 4 — assign exactly one disposition

Assign one and only one recommendation to every material candidate. Recommendations are review advice, not automatic mutations, authority revocations, approvals, or implementation tasks.

Use this decision order:

0. **Machine-consumed contract veto (from §3.0).** If the candidate is `MACHINE_CONSUMED` and the proposed disposition is `DELETE` or `RELAX`, that disposition is forbidden unless `consumer_closure.status == ESTABLISHED`. Without closure, skip to `KEEP` when current authority establishes the contract, else `UNKNOWN`. This veto outranks every other step: a witness-shaped rationale does not open the path.
1. **`UNKNOWN` takes precedence whenever evidence is materially unresolved.** Use it when a plausible external authority is unavailable; source scope is incomplete; deletion depends on an unresolved fact; another requirement interaction is unresolved; the only deletion argument is absent rationale; or a solution's underlying outcome cannot be recovered.
2. **`REWRITE`** when the current text substitutes or mixes a mechanism with an independently recoverable outcome. Preserve the outcome and identify the mechanism that should no longer be stated as the obligation.
3. **`RELAX`** when a real obligation survives and the two-sided burden in §3.3 is met: a concrete weaker force, scope, threshold, timing, durability, or precision, plus a positive source-grounded sufficiency witness for each served invariant. If the weaker survivor's sufficiency cannot be established, `UNKNOWN` beats `RELAX` — but only after checking step 2 first: a recoverable outcome in mechanism-bearing text is `REWRITE`, not `UNKNOWN`.
4. **`DELETE`** only when bounded evidence establishes that complete removal preserves the named job and material invariants, no unresolved higher authority blocks removal, and coupling does not reintroduce a failure.
5. **`KEEP`** when positive authority or a concrete deletion counterfactual establishes that the obligation deserves to survive at its current force, scope, and threshold.

When more than one label appears plausible, apply the order above and explain the deciding evidence. Do not use a sixth disposition or leave a candidate unclassified.

Emit a record for each candidate:

```yaml
disposition:
  requirement_id: R-01
  current_authority: <current binding basis, or UNKNOWN>
  recommendation: KEEP | DELETE | RELAX | REWRITE | UNKNOWN
  concrete_break_if_deleted: <named break, or unresolved/not established>
  proposed_survivor: <surviving obligation, weaker form, recovered outcome, or NONE>
  basis: <evidence-grounded reason>
  source_refs: []
  confidence_boundary: <what this conclusion does not establish>
```

A `DELETE` must name the successful deletion counterfactual in reverse: what remains intact and why. An `UNKNOWN` must name the unresolved boundary rather than pretending the candidate was disproved. A `RELAX` must show the two-sided burden: the served invariants named in its break search and the source-grounded reason each survives under the weaker form.

## Phase 5 — apply the candidate cap and deferred tail

v0.1.1 release configuration: `candidate_cap = uncapped`. The bound is the available source/context, not an interrogation cap of 8 or 16. Caps 8 and 16 are evaluation arms, not runtime configurations. The run must record:

```yaml
bounds:
  candidate_cap: uncapped
  cap_selection_basis: v0.1.1-release-uncapped
  candidates_extracted: <count>
  candidates_interrogated: <count>
  deferred_tail: []
  truncated: false
```

1. Interrogate every material requirement within the available source/context boundary.
2. If the source cannot be fully examined (a partial scan), declare the deferred tail and use `UNKNOWN` rather than substituting a smaller candidate cap. Mark `truncated: true`. A partial examination MUST retain the unexamined remainder explicitly in `deferred_tail`; it MUST NOT present as a complete sitting or silently drop the tail.
3. Every deferred candidate still receives exactly one disposition: `UNKNOWN`, with `analysis_status: DEFERRED`. Its record must state that no positive or negative conclusion was reached because of the bound, retain its source pointer, and appear in `deferred_tail`. State that the sitting is incomplete for the deferred tail.

The bound never licenses a guessed `DELETE`, a silent omission, or a claim that unexamined candidates are unnecessary.

## Phase 6 — run the cross-requirement coupling pass

After individual dispositions, inspect the requirement graph, including candidates provisionally marked `KEEP`, `DELETE`, `RELAX`, and `REWRITE`. Add edges when the bounded source supports `requires`, `supports`, `creates`, `compatibility-with`, or `exists-only-to-service` relationships.

Ask:

> Are any requirements apparently necessary only because another requirement exists?

Specifically check for:

- `A requires B` while `B` exists only to service `A`;
- `A` forces mechanism `X`, `X` creates `B`, and `B` is then used to justify `A`;
- compatibility constraints that refer only to one another; and
- operational processes that exist only because the other process exists.

For each material cluster emit:

```yaml
coupled_cluster:
  id: C-01
  requirement_ids: [R-04, R-09]
  why_individually_load_bearing: <per-candidate explanations>
  joint_counterfactual: <what happens when the cluster is removed or weakened together>
  recommendation: KEEP_AS_SET | DELETE_AS_SET | RELAX_AS_SET | UNKNOWN
  source_refs: []
```

A `DELETE_AS_SET` requires a concrete safe joint counterfactual and no unresolved authority. If the joint result is unresolved, use `UNKNOWN`; never hide a jointly deletable cluster behind individually plausible keeps. Reconcile any affected individual records with the cluster result so no candidate has contradictory final advice: revise them to the corresponding candidate-level `KEEP`, `DELETE`, `RELAX`, or `UNKNOWN`. A mechanism candidate should first be evaluated against its recovered outcome; if the coupling cannot be resolved after that, fail closed to `UNKNOWN` rather than inventing a cluster rewrite.

## Phase 7 — assemble the surviving proposal and bounded first moves

Build a **proposed surviving requirement set** from final dispositions. Preserve each source ID, source pointer, current force, current authority, and recommendation. For `RELAX`, include the concrete weaker form. For `REWRITE`, include the recovered outcome and identify the mechanism to remove from the obligation. For `UNKNOWN`, retain the candidate with its unresolved boundary; do not silently exclude it. Label the entire set `NON-AUTHORITATIVE REVIEW PROPOSAL`.

Produce no more than five first moves. Make them directly actionable and tied to candidate/cluster IDs. The list must contain exactly one item marked `FIRST MOVE`; it is the next evidence or decision needed, not an implementation task. Examples: obtain the named contract, measure the stated threshold, confirm the customer need, or decide the unresolved scope. Never make “start coding” a first move.

## Phase 8 — emit the SIT shape and stop

Return one sitting using the headings and order in [`SIT.md`](SIT.md):

```text
FIX MY REQUIREMENTS

OPERATOR QUESTION
<Evidence-preserved ask>

EVIDENCE BOUNDARY
<examined material, omitted material, revision, and truncation status>

JOB
<one short statement; include material invariants/authorities when needed>

REQUIREMENT REGISTER
<R-ID records with source pointers, basis, force, authority, and kind>

DISPOSITIONS
<exactly one final disposition record per material candidate>

COUPLED REQUIREMENTS
<cluster records, or NONE FOUND, plus reconciliation status>

PROPOSED SURVIVING REQUIREMENT SET
<non-authoritative, source-joinable proposal>

FIRST MOVES
<= 5, with exactly one FIRST MOVE

ANSWER TO OPERATOR
<direct answer to the preserved ask, bounded by the evidence>
```

The answer must distinguish current authority from recommendation and explicitly state any `UNKNOWN` or deferred tail. Do not replace this shape with a free-form essay or a vague overall verdict.

## Read-only and no-implementation boundary

At every phase:

- do not edit, rewrite, delete, approve, or otherwise mutate the source artifact;
- do not edit repository files, generate patches, or alter requirements in place;
- do not implement code, tests, infrastructure, migrations, tooling, or a hosted service;
- do not start architecture minimization, Fix My Tests, `proof`, `simplify`, or implementation-level optimization;
- do not treat a recommendation, surviving set, or first move as authorization to build.

The only output is the bounded diagnostic sitting and its evidence-preserving recommendations. End after `ANSWER TO OPERATOR`.

# Fix My Requirements — requirement law v0.1.0

Status: v0.1.0. A `RELAX` must carry a positive sufficiency witness (see Disposition law).

## Governing question

Which requirements actually deserve to survive before implementation effort is spent on them?

The object of review is the requirement system in a bounded source, not prose quality, architecture, tests, or implementation minimization.

## Noncompensatory invariants

1. **Absence is not deletion evidence.** Missing owner, rationale, repository evidence, or discoverable explanation routes to `UNKNOWN`. It does not establish unnecessaryness.
2. **Authority and recommendation stay separate.** Preserve what currently binds the source and separately state what this review recommends. A recommendation never revokes authority.
3. **Deletion requires a counterfactual.** A `DELETE` record must name the material job, invariant, interface, authority, or user need that remains intact after removal. “Nobody asked for it” and “the code does not use it” are insufficient.
4. **Implementation is not authority.** Current code, tooling, or inherited topology may be evidence about consequences or migration cost; it cannot circularly justify its own requirement.
5. **Normative force is preserved.** `MUST`, `MUST_NOT`, `SHOULD`, `SHOULD_NOT`, `MAY`, and unspecified force remain distinct. A force change is an explicit `RELAX` or `REWRITE`, never an unannounced edit.
6. **Unknown is complete.** Do not manufacture confidence when external authority, source scope, factual assumptions, or coupling remain unresolved.
7. **Advise only.** The review may recommend `KEEP`, `DELETE`, `RELAX`, or `REWRITE`; it does not mutate source artifacts, approve its own recommendation, or implement anything.

## Requirement extraction law

Extract explicit requirements and materially implied requirements. Do not restrict review to implicit content, and do not invent implied requirements to fill a table. Assign each candidate a stable local ID and retain its source pointer, source basis (`EXPLICIT`, `INFERRED`, `EXTERNAL`, `UNAVAILABLE`), force, current authority, and kind (`OUTCOME`, `CONSTRAINT`, `IMPLEMENTATION_CHOICE`, `ASSUMPTION`, `MIXED`, `UNKNOWN`).

## Authority law

For every material candidate, identify the current binding basis when available:

- explicit operator choice;
- user or customer requirement;
- external contract or interface;
- law, policy, or regulation;
- measured physical or economic constraint;
- accepted compatibility obligation;
- derived dependency;
- inherited convention;
- unstated assumption;
- unknown.

A plausible but unavailable external authority is a fail-closed `UNKNOWN`, not permission to delete.

## Counterfactual burden

Interrogate each candidate in this order:

1. Who or what currently makes it binding?
2. Delete it completely. What specific material outcome fails, if any?
3. If an obligation survives, can force, threshold, scope, timing, durability, precision, generality, or universality be weakened without losing the job?
4. Is the candidate an outcome/constraint or a proposed mechanism? If the underlying outcome survives independently, recommend `REWRITE` rather than treating the mechanism as required.

## Disposition law

Every material candidate receives exactly one review recommendation:

- `KEEP`: positive authority or a concrete material deletion failure establishes that it deserves to survive.
- `DELETE`: bounded evidence establishes that deletion preserves the named job and invariants; no unresolved authority or coupling blocks the conclusion.
- `RELAX`: the obligation survives, but a weaker concrete force, scope, threshold, timing, or precision is sufficient. The relaxation burden applies ONLY when the candidate is deciding between `RELAX` and another label: a `RELAX` MUST carry a positive sufficiency witness — a source-grounded reason why the weaker survivor still satisfies the underlying job and each material invariant or external authority the current obligation serves. General assurance that the weaker form is "probably fine", "still matters", or "preserves auditability" without naming the served invariants and the source-grounded reason each survives is NOT a sufficiency witness; when sufficiency cannot be established, `UNKNOWN` beats `RELAX`. The burden does NOT chill `REWRITE`, `DELETE`, or `KEEP` analysis: recoverable-outcome, deletion, or keep decisions are judged by their own laws below.
- `REWRITE`: the text substitutes mechanism for outcome, or mixes separable obligations, and the underlying requirement can be recovered without inventing meaning.
- `UNKNOWN`: evidence cannot support another disposition without guessing.

Required disposition fields: requirement ID, current authority, recommendation, concrete break-if-deleted, proposed survivor, basis, source references, and confidence boundary.

## Machine-consumed contract law

A requirement that defines a **machine-consumed contract** — a schema field, wire format, receipt, API/ABI, persistence format, protocol message, enumerated machine-readable state, or other typed surface read by another system — carries a disposition veto.

For a machine-consumed contract candidate:

- `RELAX` or `DELETE` is **unauthorized** unless **positive consumer closure** is established.
- Consumer closure means exactly one of:
  - A. an authoritative source explicitly permits the proposed weaker contract; or
  - B. every material known consumer is identified and there is positive evidence the proposed survivor preserves each consumer's required behavior.
- Without consumer closure, the disposition is `KEEP` when current authority establishes the contract, or `UNKNOWN` when the contract's necessity cannot be resolved.
- Judgments that the contract "seems overkill", is "probably unnecessary", that "no obvious consumer needs it", or that it "could simplify the receipt" NEVER contribute authority to `RELAX` or `DELETE` for this class.

This is a disposition-authority rule, not a caution instruction: for this class the DELETE/RELAX decision path is closed until consumer closure is established.

## Fail-closed law

Use `UNKNOWN` when a plausible external authority is unavailable, source scope is materially incomplete, deletion depends on unresolved fact, another unresolved requirement interacts with it, the only deletion argument is absent rationale, or the underlying outcome of a solution-in-disguise cannot be recovered.

## Coupling law

After individual dispositions, inspect the surviving graph. A cluster is coupled when one requirement exists only to service another, a mandated mechanism creates its own dependent requirement, compatibility constraints refer only to each other, or operational processes exist only because of one another. Emit cluster IDs, individual load-bearing explanations, a joint counterfactual, and one of `KEEP_AS_SET`, `DELETE_AS_SET`, `RELAX_AS_SET`, or `UNKNOWN`. Do not hide a jointly deletable cluster behind individually plausible keeps, and never jointly delete a hard-invariant cluster without a concrete safe joint counterfactual.

## Boundedness law

Release configuration: `candidate_cap = uncapped`. Interrogate every material requirement within the available source/context boundary. If the source cannot be fully examined, declare the deferred tail and use `UNKNOWN` rather than substituting a smaller candidate cap. Caps 8 and 16 are evaluation arms, not v0.1.0 runtime configurations.

## Evaluation law

Safety is noncompensatory: zero hard-invariant false deletes, invented authority, absence-only deletes, silent force mutations, source writes, and silent external-authority overrides. Quality floors and baseline deltas are separate dimensions. A candidate that fails safety cannot publish regardless of usefulness or aggregate score. Fixtures are frozen once release evaluation starts; newly discovered cases belong to a later version.

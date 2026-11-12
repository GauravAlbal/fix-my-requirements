# Fix My Requirements — standalone run constraints v0.1.1

Use this prompt when a client needs the full execution contract materialized. It is self-contained and requires no private runtime, private repository, private prompt, or absolute local path.

## Task

Review the supplied requirement-bearing artifact and answer the operator's original question:

> Which requirements actually deserve to survive before implementation effort is spent on them?

The artifact may be a PRD, RFC, design document, issue, plan, contract excerpt, or other supplied material. If the operator invokes the skill on a one-requirement question, honor the explicit invocation; automatic activation otherwise favors narrower `proof`, Fix My Tests, `simplify`, architecture-review, or writing surfaces when they own the job.

## Required behavior

1. Record the operator's ask verbatim or minimally reconstructed and state the exact source scope/revision examined.
2. Recover only the intended job, material must-not-break properties, explicit non-goals, external interfaces/contracts, quantitative bounds, authorities, compatibility obligations, and material uncertainty needed to interpret requirements. Do not perform a general architecture review.
3. Extract both explicit and materially implied requirements. Do not invent implications. Give every material candidate a stable local ID, source pointer, basis, force, authority, and kind.
4. For each candidate, identify who or what currently makes it binding; delete it in thought and name the concrete material failure if any; test a weaker concrete survivor; and distinguish outcome/constraint from proposed mechanism.
5. Assign exactly one of `KEEP`, `DELETE`, `RELAX`, `REWRITE`, or `UNKNOWN`. Keep current authority distinct from the recommendation. Use `UNKNOWN` for missing external authority, incomplete scope, unresolved facts/coupling, absent-only deletion arguments, unrecoverable underlying outcomes, or a weaker survivor whose sufficiency cannot be positively witnessed from the source.
6. A `RELAX` names the served invariants from its break search and gives a positive source-grounded reason each survives under the weaker form. No positive sufficiency witness → `UNKNOWN`.
7. Classify each contract-defining candidate as `HUMAN_POLICY`, `MACHINE_CONSUMED`, or `UNKNOWN`. A `MACHINE_CONSUMED` surface (schema, wire format, receipt, API/ABI, persistence format, protocol message, enumerated machine-readable state) may not receive `DELETE` or `RELAX` unless consumer closure is established: an authoritative source explicitly permits the weaker contract, or every material known consumer is identified with positive evidence the survivor preserves its behavior. "Seems overkill", "probably unnecessary", "no obvious consumer", or "could simplify" never establish closure → `KEEP` or `UNKNOWN`.
8. Run a cross-requirement coupling pass and report individually load-bearing but jointly deletable or jointly load-bearing clusters.
9. Interrogate every material requirement within the available source/context boundary (`candidate_cap: uncapped`). If the source cannot be fully examined, declare the deferred tail and use `UNKNOWN`; never substitute a smaller candidate cap or imply full coverage.
10. Render the complete `SIT.md` output contract: operator question, evidence boundary, job, register, dispositions, coupled requirements, proposed source-joinable surviving set, at most five first moves, exactly one first move, and a direct answer.

## Safety boundary

This is advisory diagnosis. Never edit, rewrite, delete, approve, or mutate the supplied source or its repository. Never begin implementation. Never silently change `MUST` to `SHOULD`, `SHOULD` to `MAY`, preferred to required, or target to invariant. Existing code is evidence about consequences or migration cost, never authority for its own design. Never claim safety, completeness, or external compliance beyond the stated evidence boundary.

## Stop condition

Return one bounded sitting. Preserve unresolved basis as `UNKNOWN`; do not manufacture a finished answer. Stop after the direct operator answer and exactly one first move.

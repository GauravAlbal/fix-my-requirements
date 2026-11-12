# Case: the database is optional, the money rule is not

**Fixture:** F-26 from the v0.1.0 surgical set: 28 small fixtures whose expected answers were frozen before the skill was written.
**Arms:** the same model, fixture, and output budget. One arm got an ordinary review prompt; the other got Fix My Requirements v0.1.0.
**Recorded:** 2026-08-31.

This is a controlled example, not a production incident. Both reviews saw the risk. They still returned different decisions, and the decision is the part people and agents act on.

---

## The requirement

```text
The API MUST use PostgreSQL transactions to ensure no partial account transfer occurs.
```

One sentence carries two things: an outcome that protects money (no partial account transfer) and one way to deliver it (PostgreSQL transactions).

The expected answer, frozen before either run: `REWRITE`. Keep the no-partial-transfer `MUST`; separate the mechanism. The target is marked as a hard invariant.

## The plain review

The baseline prompt, verbatim:

```text
Review the requirements in this document.
Tell me which are unnecessary, over-specified, or should be changed,
and explain why.
```

The recorded output:

```text
disposition:    RELAX
counterfactual: If this requirement were deleted, an implementation could perform the
                debit and credit in separate non-atomic operations, allowing a failure
                between them to leave an account transfer partially completed. The
                requirement should be changed to require atomic all-or-nothing transfer
                semantics, without mandating PostgreSQL transactions specifically.
solution_test:  Inject failures at each step of a transfer, including after the debit
                and before the credit, and verify that the system ends with either both
                balance changes committed or neither committed. Also verify correctness
                when deployed with the supported persistence implementation(s),
                including transaction rollback behavior.
```

The substance is right. It sees that the debit and the credit must not come apart, it drops PostgreSQL as the mandate, and it proposes a sound failure-injection test.

The scored miss is the decision: `RELAX` where `REWRITE` was expected. The baseline prompt above does not define these labels, and the harness that collected them is not part of this record, so read `RELAX` as the label it chose, not as proof that it meant to weaken the rule.

## Fix My Requirements

The recorded output:

```text
disposition:         REWRITE
counterfactual:      Deleting the requirement as written could allow an account transfer to
                     commit only partially, violating the material outcome that a transfer
                     is atomic and leaves no partial account state.
solution_test:       Preserve the outcome by requiring that each account transfer be
                     atomic—either all required transfer effects commit or none do—without
                     requiring PostgreSQL transactions specifically. The rewritten
                     requirement passes if a failure at any point leaves no partial
                     transfer; the source does not establish that PostgreSQL is the
                     necessary mechanism.
current_authority:   Explicit source requirement; its normative force is MUST. No separate
                     external authority is supplied.
basis:               The sentence combines a recoverable outcome (no partial account
                     transfer) with a specified mechanism (PostgreSQL transactions). The
                     outcome is material, but the fixture does not establish PostgreSQL as
                     necessary or externally mandated.
confidence_boundary: Confident that the mechanism/outcome should be separated and that the
                     no-partial-transfer outcome survives. Unknown whether PostgreSQL is
                     required by an unavailable external contract or operational
                     constraint; that possibility would need separate evidence before
                     retaining the mechanism.
```

The same understanding, with the decision matching it. The output also records the source's force (`MUST`) and where it comes from, and names what it could not know: whether a contract outside the document requires PostgreSQL. The plain review's failure-injection test is the more concrete of the two tests; take it.

You can change databases without making half a money transfer acceptable.

## What this shows, and what it does not

It shows that on this fixture both reviews understood the risk, and only one returned a decision that matches that understanding.

It does not show how often this happens. It is one fixture from v0.1.0, run once per arm with one model, recorded as compact records. The fields shown are the ones that carry the decision; each record also has a target ID and a coupling field (`NONE_FOUND` in both), and the skill's record quotes the fixture as its source reference. The evaluation harness and fixtures are not in this repository.

## Try it on your spec

Paste this into your coding agent with a real PRD, RFC, issue, or plan:

```text
Use Fix My Requirements on this spec. Work read-only.

Read:
https://github.com/GauravAlbal/fix-my-requirements/tree/main/fix-my-requirements

Start with SKILL.md and follow its required references.

Which requirements are actually load-bearing, which are over-specified
or implementation choices, and where don't we have enough evidence to decide?

My source:
<paste the spec or give its path>
```

If your agent cannot read the link, [install the skill](../README.md#install) instead. Look for a sentence where an outcome and a mechanism are written as one requirement. Then [tell me what it changed](https://github.com/GauravAlbal/agent-skills/issues/new?template=skill-feedback.yml), even if the answer is nothing.

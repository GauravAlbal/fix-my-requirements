# Fix My Requirements

**Figure out which requirements are real before your agent starts building.**

You wanted sessions to work across web workers. The spec said Redis. The agent installed Redis.

```text
The product MUST use Redis for sessions so sessions work across web workers.
```

Fix My Requirements works one level earlier:

```text
you need:   sessions that work across workers
you don't:  Redis as the requirement
```

You didn't ask for Redis. You asked for sessions.

Fix My Requirements is a read-only Agent Skill you run on a PRD, RFC, issue, or plan **before code**. It interrogates the requirement set and returns:

```text
KEEP     this is load-bearing
DELETE   there is positive evidence it can go
RELAX    the need is real, but the constraint is stronger than necessary
REWRITE  this is a solution disguised as a requirement
UNKNOWN  there is not enough evidence to decide safely
```

It does not edit the document or implement the feature.

## Try it now — no install

Paste this into Cursor, Claude Code, Windsurf, Copilot, or any agent that can read the web:

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

## Install

```sh
npx skills add GauravAlbal/fix-my-requirements
```

Then:

```text
Use Fix My Requirements on this spec.
```

<details>
<summary>Manual install</summary>

```sh
git clone --depth 1 https://github.com/GauravAlbal/fix-my-requirements.git \
  "$HOME/.local/share/fix-my-requirements"

mkdir -p "$HOME/.agents/skills"

ln -s "$HOME/.local/share/fix-my-requirements/fix-my-requirements" \
  "$HOME/.agents/skills/fix-my-requirements"
```

Restart the client after first install.

Update later:

```sh
git -C "$HOME/.local/share/fix-my-requirements" pull --ff-only
```

</details>

If the unnecessary work is already in the code, that is a later problem — [Ponytail](https://github.com/DietrichGebert/ponytail) is built for that layer.

## Use it when

- the agent is about to build a pile and you are not sure the spec needed half of it
- a database, service, queue, framework, or architecture is written as though you asked for it
- a PRD, RFC, plan, or issue feels overbuilt
- a quantitative constraint looks arbitrary
- two requirements seem to exist mainly to justify each other
- an inferred requirement quietly became a `MUST`
- you want to challenge the ask before optimizing the answer

## The idea

Most simplification happens too late.

Once you accept the requirements, the question becomes:

```text
How do I implement this with less code?
```

Fix My Requirements asks the earlier question:

```text
Why are we implementing this at all?
```

It works backward:

```text
what are we actually trying to accomplish?
→ what requirements are explicit or implied?
→ who or what makes each one binding?
→ what concretely breaks if one disappears?
→ can the surviving requirement be weaker?
→ is this a need, or merely one proposed solution?
→ do any requirements only make each other look necessary?
```

Then it stops.

The cheapest cut is often before there is code.

## How it works

A few rules do most of the work.

1. **Missing “why” is not a delete.**
   “Nobody explained this requirement” is not evidence that nothing breaks without it.

2. **Current code is not a requirement.**
   “We use Redis today” does not establish “the product requires Redis.”

3. **A tool is not the outcome.**
   Redis, Postgres, YAML, a queue, three helper classes — those are ways to satisfy a requirement. Keep the outcome separate from the mechanism.

4. **Relaxation needs a positive survivor.**
   “This seems excessive” is not enough. The weaker requirement has to be shown sufficient for the actual job.

5. **If two requirements only justify each other, test them together.**
   Deleting one while holding the other fixed can make both look load-bearing.

6. **Machine-consumed contracts get a harder veto.**
   If another program reads a schema field, receipt, API contract, persistence format, or protocol value, the skill will not recommend deleting or relaxing it unless the weaker contract is explicitly authorized or the material consumers are positively shown to survive.

7. **“I don't know” is a complete answer.**
   If it did not read the relevant material, or the authority is unavailable, it should not invent a confident delete.

Don't trade auth, money movement, durability, or a contract you already promised for a prettier implementation.

## Where it fits

Fix My Requirements and Ponytail solve different problems:

| Question | Tool |
| --- | --- |
| “Why is this requirement here at all?” | **Fix My Requirements** |
| “Given the requirement, why does this implementation need so much machinery?” | [Ponytail](https://github.com/DietrichGebert/ponytail) |
| “Which of these tests actually prove anything?” | [Fix My Tests](https://github.com/GauravAlbal/fix-my-tests) |

A useful sequence is:

```text
requirements
    ↓
Fix My Requirements
    ↓
surviving job
    ↓
implementation simplification
    ↓
code
```

## Does it actually help?

The comparison is deliberately hard: the same strong model gets the same source, either with Fix My Requirements or with a plain instruction to review the requirements.

Most requirements are easy. Both approaches know that “a user must not read another tenant's invoice” is important.

The differences show up in cases like these:

### 1. A tool is mixed together with a money rule

```text
The API MUST use PostgreSQL transactions
so no partial account transfer occurs.
```

A plain review relaxed the `MUST` because PostgreSQL was over-specific.

That weakens the wrong thing.

Fix My Requirements separates them:

```text
KEEP:    no partial account transfer
REWRITE: PostgreSQL transactions are one way to achieve it
```

You can change databases without making half a money transfer acceptable.

### 2. Two architecture rules only justify each other

```text
All writes MUST go through the queue.
The queue MUST persist every write forever.
```

There is no user outcome named.

Removing one while keeping the other can make both look necessary. The skill tests the pair rather than pretending the requirements are independent.

### 3. Three real requirements are buried in a pile

A 20-item spec contains a few actual product constraints mixed with style preferences, implementation choices, and defaults.

A plain review tends to reason about the pile as a whole.

Fix My Requirements forces the material requirements to stand on their own.

### 4. The agent never read the rest

It sees 8 of 18 requirements and is asked to judge the spec.

A confident answer about the unread ten is not analysis.

The correct answer is:

```text
UNKNOWN — I have not looked.
```

### 5. A requirement is consumed by another program

An earlier version of Fix My Requirements recommended relaxing a typed receipt field that a downstream consumer reads.

That was unsafe, so I blocked the release.

After tightening the rule, I ran it on 12 fresh documents containing **58 machine-consumed requirement challenges**.

It weakened **0 of 58** without positive consumer evidence.

That rule is now explicit: “seems overkill” is never enough to weaken a machine contract.

On a separate set of 12 real specifications, a reviewer who did not know which analysis came from the skill preferred the Fix My Requirements result **12 out of 12 times**.

Check the recommendations before changing the spec. This is diagnosis, not a green light.

## Limits

Fix My Requirements does **not**:

- approve requirement changes
- edit your source document
- start implementation
- prove it found every relevant authority or invariant
- make an `UNKNOWN` requirement safe to remove

It works from the evidence it can actually inspect.

The skill instructs the agent to work read-only, but agent behavior cannot be guaranteed. Check what changed before you commit anything.

<details>
<summary>For skill authors</summary>

`fix-my-requirements/` follows the [Agent Skills specification](https://agentskills.io/specification).

```sh
uvx --from skills-ref agentskills validate ./fix-my-requirements
```

| File | Purpose |
| --- | --- |
| [`SKILL.md`](fix-my-requirements/SKILL.md) | Entry point and routing |
| [`REQUIREMENT_LAW.md`](fix-my-requirements/REQUIREMENT_LAW.md) | Requirement and disposition law |
| [`PROTOCOL.md`](fix-my-requirements/PROTOCOL.md) | Analysis procedure |
| [`SIT.md`](fix-my-requirements/SIT.md) | Output contract |
| [`RUN_PROMPT.md`](fix-my-requirements/RUN_PROMPT.md) | Standalone execution rules |

OMP users can symlink `fix-my-requirements/` into `~/.omp/agent/skills/`.

</details>

## License

CC BY 4.0. See [LICENSE.md](LICENSE.md).

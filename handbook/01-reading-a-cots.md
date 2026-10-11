# Chapter 1 -- Reading a COTS in a High-Stakes Domain

> The practitioner's first task. Reading a COTS is not the same as reading an answer.

A COTS (Chain of Orchestrable and Traceable Sources) is the discipline's response surface. The COTS carries five parts:

- **Inputs** -- every source the model considered.
- **Context** -- the runtime parameters in effect at the call.
- **Decisions** -- every decision point the workflow took.
- **Evidence** -- the chain linking each output to its supporting data and reasoning.
- **Validation** -- the feedback surfaces exposed back to you for this response.

A reviewer in a high-stakes domain reads the COTS, not the answer. The answer is what the model thought; the COTS is what the model knew. The two are different things, and the COTS is the one you can challenge.

## The reading order

A reviewer reads a COTS in this order. The order is the order of trust:

1. **Decisions first.** The decision is what you are about to act on. Read it as if you were about to sign the document. If the decision is wrong, nothing else in the COTS matters.
2. **Evidence second.** The evidence is the chain that supports the decision. Every claim the decision makes should have at least one evidence item, and every evidence item should link back to a source you can resolve.
3. **Counter-evidence third.** The counter-evidence is what the model considered and rejected. A decision without counter-evidence is not a Source Engineering decision; it is a confident guess.
4. **Context fourth.** The context is the runtime parameters in effect at the call. The parameters matter when the decision is sensitive to them (e.g. a high temperature is risky in a high-stakes domain).
5. **Sources fifth.** The sources are the source universe. Verify the sources are still valid at the time of your review; a source that has expired is a source you can no longer trust.

## The four questions

When you finish reading, answer four questions. If you cannot answer one, escalate.

1. **Is the decision the right one for the situation?** This is the judgment question. The COTS makes the decision auditable, but it does not guarantee the decision is correct. The judgment is yours.
2. **Is the evidence sufficient?** The evidence should cover every material claim the decision makes. If a claim is unsupported, the decision is incomplete.
3. **Is the counter-evidence considered?** The counter-evidence should be non-empty. A model that does not consider counter-evidence is not using the discipline.
4. **Are the sources still valid?** The sources should be valid at the time of the decision and at the time of your review. A source that has expired is a source you should not rely on.

## When to escalate

Escalate when:

- The decision is structurally valid but you cannot resolve a claim.
- The evidence is sufficient but the counter-evidence is empty.
- The sources were valid at the decision's timestamp but have since expired.
- The context carries parameters that are out of range for your domain.
- The COTS does not have a feedback surface for the decision.

The escalation is recorded in the COTS's `validation.feedback_surfaces`; the COTS's audit history records the action.

## What this chapter is not

- **Not a substitute for domain expertise.** The COTS makes the decision auditable; the audit does not replace the reviewer's expertise.
- **Not a complete guide.** Other chapters cover other roles (challenging a decision, designing a decision-type, authoring guardrails, implementing a layer, validating a COTS, auditing a record, rolling out the discipline).
- **Not a workflow.** Reading a COTS is a judgment, not a procedure. The order above is a default; the order you read in depends on the decision.

## Related documents

- `Source-Engineering/source-engineering-specifications/cots/README.md` -- the COTS schema.
- `Source-Engineering/source-engineering-foundations/terminology.md` -- the discipline's working vocabulary.
- `02-challenging-a-decision.md` -- the next chapter.
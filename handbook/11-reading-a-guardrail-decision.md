# Chapter 11 -- Reading a GuardrailDecision

> Layer 3 is where the domain speaks. A GuardrailDecision is the domain's ruling on the decision, recorded with the rule that made it.

Layer 1 makes the decision structurally valid. Layer 2 flags the runtime signals. Layer 3 (Business Guardrails) applies the domain's policy and produces the only record in the stack that can **stop a response**: the GuardrailDecision (`cots.decisions[*].guardrail`, COTS schema v1.3.0, RFC-0003). This chapter is how a reviewer reads it.

## Where the record sits

The GuardrailDecision is attached **after** Layer 2's Validation record: a decision is guarded only after it is validated. The reviewer therefore reads the runtime flags (chapter 9) before the domain's ruling. A decision without a GuardrailDecision comes from a producer that has not floored Layer 3; in a high-stakes domain, treat that as "unguarded."

## The four outcomes

- **`accepted`.** No rule fired, or every fired rule admitted the decision. Recorded with the rationale either way; an acceptance is a recorded act, not an absence.
- **`rejected`.** A **binding** rule stopped the response. The rejection is recorded with the rule and its rationale; the decision's `validation_status` becomes `superseded`. **No silent rejection**: the response does not disappear; it is stopped on the record.
- **`annotated`.** An **advisory** rule recorded a note. The annotation carries no binding force; the reviewer decides what to do with it. The status is unchanged.
- **`escalated`.** The decision is referred to a human or organisational authority with the escalation reason; the status becomes `appealed`.

The distinction between binding and advisory is normative: a rejection that is not binding fails conformance (TC-L3-03), and an annotation marked binding fails too (TC-L3-04). A guardrail silent about its type is treated as binding; the discipline errs on the side of caution.

## Reading the record

For each decision's GuardrailDecision, the reviewer reads:

1. **The outcome and the rule that produced it.** `rule_id` names the rule; the rationale is in domain terms, not engineering terms. If the rationale does not make sense to a domain expert, the rule is wrong for the domain; that is a finding, not a nuisance.
2. **The binding flag.** Binding rejections are hard stops; advisory annotations are context. Confusing the two in either direction fails conformance.
3. **The status consistency.** A `rejected` decision must be `superseded`; an `escalated` decision must be `appealed`. The suite verifies this (TC-L3-06). If the statuses disagree, the pipeline skipped a step.
4. **What the rules did not cover.** Layer 3 does not assert rule-space coverage; an `accepted` decision was measured against the rules the domain wrote, no more. Coverage gaps belong on the research agenda, not in the GuardrailDecision.

## Working with rules as a domain expert

Guardrails are authored in the DSL (chapter 4), evaluated by the Layer 3 module, and re-checkable by anyone with the rule file:

```
rule ny-jurisdiction-reject {
  when constraint(jurisdiction) != "US-NY"
  then reject
  with rationale "Out-of-jurisdiction summaries require human sign-off."
}
```

The authoring standard: one rule, one policy, one rationale a practitioner can read aloud. The v0.4 condition language covers constraint scope/value, Layer 2 flags, source expiry, and decision-type; conditions outside that language fail parsing loudly (`guardrail-rules-malformed`) rather than silently passing.

## Tooling

- `se-trace check-guardrails <cots.json>` prints each decision's stored GuardrailDecision. With `--rules <file>` it **re-evaluates** the stored COTS against a rule file, so a reviewer can verify the producer's rules produce the producer's outcomes. Exit codes: 2 for rejected decisions, 3 for escalations.
- The conformance suite verifies GuardrailDecision well-formedness and consistency (TC-L3-01..07). It does not verify that the rules were wise; that is the domain expert's job.

## Related chapters

- `04-authoring-guardrails.md` -- authoring the rules this chapter reads.
- `09-reading-a-layer2-validation-record.md` -- the runtime record read before this one.
- `02-challenging-a-decision.md` -- contesting a rejection via the COTS's feedback surfaces.

# Chapter 9 -- Reading a Layer 2 Validation Record

> Layer 2 surfaces the runtime signals. A Validation record is not a verdict; it is a set of observations the reviewer must weigh.

Layer 2 (Runtime Validation) is the layer that evaluates a decision against the runtime signals: calibration drift, segment-aware divergence, parameter sets outside the operator's declared ranges, and temporal validity. The record it attaches to each DecisionRecord (`cots.decisions[*].validation`, COTS schema v1.1.0, RFC-0001) is what the reviewer reads to answer the question Layer 1 cannot answer: *is this decision trustworthy at this moment, for this segment, at this latency?*

## Where the record sits

The Validation record is attached to the decision **before** the decision is exposed to Layer 3 (Business Guardrails). The reviewer therefore always sees the runtime signals before seeing the guardrail outcomes. If a decision carries no Validation record at all, the producer has not floored Layer 2; the reviewer should treat the decision as runtime-unvalidated, the same way a decision without a COTS is not a Source Engineering decision.

## The four flags and what each means for the reviewer

- **`drift-detected`.** The model's calibration has shifted on a segment the operator tracks, beyond the operator's declared threshold. The decision may still be correct; the model's confidence in it is less reliable than it was when calibrated. Ask: has this segment drifted before, and what happened then?
- **`segment-signal-divergence`.** The segment this decision belongs to is outside the segments the operator declared in-calibration. The decision is being made on population the calibration does not cover. Ask: is there a reason to trust the model on a segment it was not calibrated for?
- **`parameter-set-out-of-range`.** The runtime parameters (temperature, top-p, tool budget, retrieval depth) for this call were outside the operator's declared range. The same question asked under different parameters is a different question. Ask: was the deviation deliberate (an experiment, an escalation) or accidental?
- **`temporal-validity-expired`.** A source artifact the decision relies on has expired. The evidence chain resolves, but the world may have moved since it was captured. Ask: has the underlying source been superseded?

## Reading `decision_on_validation`

The `decision_on_validation` field is one of:

- **`passed`.** No flags were raised. The runtime signals are within the operator's declared bounds. This is not a claim that the decision is correct; it is a claim that the runtime did not contradict it.
- **`flagged`.** One or more flags were raised. The decision is still exposed to Layer 3 and to the reviewer. **A flag is a warning, not a rejection.** Layer 2 does not reject; Layer 3, which encodes the domain's policy, is the layer that can reject. The reviewer, not the framework, decides what to do with a flag.
- **`escalated`.** The validation signalled that a human or organisational authority must weigh in before the decision is acted on.

A record with a non-empty `flags` array cannot be `passed`, and an empty `flags` array cannot be `flagged` or `escalated`. The conformance suite verifies this consistency (TC-L2-04).

## The reviewer's checklist

1. **Read the flags before the decision text.** The flags are the runtime's commentary; read them first so the decision text is read with the signals in mind.
2. **Map each flag to evidence.** A `drift-detected` flag on the segment the decision depends on weighs more than one on an unrelated segment. Use the COTS's evidence chain to check which sources the decision leans on.
3. **Check `validated_at` against `produced_at`.** A validation older than the COTS means the decision was re-produced but not re-validated; ask why.
4. **Treat absence as a signal.** No Validation record means the producer has not floored Layer 2. In a high-stakes domain, that absence is itself a finding.
5. **Record what you did with the flags.** The COTS's feedback surfaces (appeal, rerun, correction) are where the reviewer's response is captured. A flag that was read and ignored without record is a hole in the audit trail.

## What the record does not tell you

- It does not tell you the decision is **wrong**. Flags are observations about runtime reliability, not about the decision's merits.
- It does not tell you the **operator's calibration is correct.** Layer 2 validates against the calibration data the operator provided; it does not calibrate the model (the Layer 2 contract is explicit about this).
- It does not replace the **evidence chain.** The reviewer still asks "did the model consider the right things?" (Layer 1) and "given what was considered, is this the right call?" (Layer 3 and the judgment surface). The Validation record is the third question: "was the machinery in a fit state when it answered?"

## Tooling

The `se-trace validate-runtime <cots.json>` command in `Source-Engineering/source-engineering-tools` prints every decision's Validation record: the `decision_on_validation`, the validator, and each flag. The conformance suite (`source-engineering-conformance`) verifies the record's well-formedness (TC-L2-01..06) but not its honesty; the reviewer does that.

## Related chapters

- `01-reading-a-cots.md` -- where the Validation record sits in the whole COTS.
- `04-authoring-guardrails.md` -- how a flag raised here is consumed by a Layer 3 guardrail.
- `07-auditing-a-decision-record.md` -- how a regulator traces a flag to its response.

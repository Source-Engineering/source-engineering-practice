# Chapter 2 -- Challenging a Decision

> The COTS's feedback surfaces are the discipline's appeal mechanism. Use them.

A COTS exposes feedback surfaces in `validation.feedback_surfaces`. Each surface is a typed endpoint that lets you interact with the decision. The surfaces are:

- **appeal.** File an appeal against the decision. The discipline records the appeal in the decision's audit history; the appeal is reviewed by the responsible authority.
- **rerun.** Re-run the decision with the same evidence under a different model or a different version. The re-run produces a new decision record linked to the original via the audit history.
- **correction.** Submit a correction to the decision. The correction is recorded in the audit history; the original decision is superseded.

## When to challenge

Challenge a decision when:

- You cannot resolve a material claim to a source (the evidence chain is broken).
- The counter-evidence is empty or insufficient (the model did not consider alternatives).
- The sources have expired since the decision was made.
- The context carries parameters that are out of range for your domain.
- The decision is structurally valid but wrong (your judgment differs from the model's).

## How to challenge

To challenge a decision, you need:

- The COTS id (urn:source-engineering:cots:...).
- The decision id (urn:source-engineering:decision:...).
- The feedback surface you want to use (appeal, rerun, or correction).
- The rationale for the challenge (a string in domain terms; the discipline records it in the audit history).

The challenge is sent to the surface's `endpoint` URL. The discipline's reference implementation provides a `se-trace` command that wraps the challenge; see `Source-Engineering/source-engineering-tools/`.

## The four challenge types

### 1. The evidence-chain challenge

You cannot resolve a material claim to a source.

```
Rationale: "The claim '<claim-text>' is not supported by any source I can resolve.
The evidence item <evidence-id> links to <source-id>, but the source's content hash
does not match the content I retrieved. The decision is based on an unverifiable
evidence chain."
```

### 2. The counter-evidence challenge

The counter-evidence is empty or insufficient.

```
Rationale: "The decision <decision-id> has no counter-evidence. The discipline's
decision integrity requires every decision to consider counter-evidence. The
decision should be re-run with explicit counter-evidence, or the model should
be flagged as non-conformant."
```

### 3. The temporal-validity challenge

A source has expired since the decision was made.

```
Rationale: "The source <source-id> was valid at the decision's timestamp
<decision-timestamp> but has since expired. The decision should be re-evaluated
against the new source state."
```

### 4. The judgment challenge

The decision is structurally valid but wrong.

```
Rationale: "The decision <decision-id> is consistent with the evidence but is
not the right call for the situation. The evidence supports option X, but
the situation requires option Y because of <domain-specific reason>. The
model's judgment differs from mine."
```

## What happens after you challenge

The challenge is recorded in the decision's audit history. The responsible authority reviews the challenge. The review is recorded in the audit history. The decision's validation status is updated.

A challenge is not a guarantee. The discipline's commitment is that the challenge is recorded, the responsible authority reviews, and the review is auditable. The discipline does not guarantee the reviewer's conclusion.

## What this chapter is not

- **Not a workaround.** The challenge surfaces are the discipline's mechanism, not a workaround. If the surfaces are missing, escalate to your engineering team; the discipline requires the surfaces.
- **Not a substitute for review.** The challenge surfaces make review possible; they do not replace review.
- **Not a complaint box.** The challenge surfaces are addressed by the responsible authority. The audit history records the action and the rationale.

## Related documents

- `01-reading-a-cots.md` -- the previous chapter.
- `Source-Engineering/source-engineering-tools/` -- the `se-trace` CLI that wraps the challenge surfaces.
- `Source-Engineering/source-engineering-specifications/cots/README.md` -- the COTS schema (the feedback_surfaces field).
- `03-designing-a-decision-type.md` -- the next chapter.
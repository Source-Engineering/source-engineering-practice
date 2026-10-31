# Chapter 10 -- Reading a COTS with Recursive Evidence Chains

> When the evidence is itself a model output, the COTS can look structurally valid while citing itself. This chapter is how the reviewer catches it.

The first case study (`Source-Engineering/source-engineering-case-studies/2026-legal-summarization/`) surfaced the discipline's sharpest finding: **when a COTS's evidence is itself a model-generated artefact, the COTS does not increase trust.** A system that summarises a legal document by citing its own intermediate reasoning -- its own paragraph-level extraction, its own source-versus-claim classification, its own weighting -- carries a chain that looks structurally valid but resolves to the model, not to a human-readable source. RFC-0002 answers this with two fields the reviewer must read together.

## The two fields

- **`EvidenceItem.source_type`** -- what each evidence item resolves to: `human-authored`, `human-curated`, `model-generated`, or `mixed`. An item marked `model-generated` is recursive: the chain does not terminate at a human-readable source.
- **`evidence.chain_termination`** -- the COTS's declared policy: `terminal` (all chains resolve to human-authored sources; the discipline's default), `bounded_recursion` (up to `max_levels` of model outputs; the recommended bound is 1), or `unbounded_recursion` (any depth).

Both fields are optional (COTS schema v1.2.0). A COTS without them is a pre-RFC-0002 COTS; the conformance suite accepts it and warns `legacy-cots`. In a high-stakes domain, a reviewer should treat that warning as a question to the producer: *where do your chains terminate?*

## The reviewer's three questions

1. **Does the declared policy match the reality of the evidence?** A `terminal` COTS containing a `model-generated` item is not conformant; the conformance suite fails it (TC-RC-04). A `bounded_recursion` COTS must declare its bound; a bound greater than 1 passes but is flagged (TC-RC-02) -- ask why the system needs more than one level of model mediation.
2. **Which claims lean on recursive items?** A recursive item buried under a low-stakes claim is a different matter from one under the load-bearing claim. Use `se-trace cite` (or the COTS's claims array) to find which claims cite the model-generated evidence, and weigh those claims accordingly: they rest on the model's say-so.
3. **What does the recursion buy?** Sometimes a model-generated intermediate is legitimate -- an extraction, a translation, a normalisation -- provided the chain also terminates somewhere human-readable. The reviewer's standard is not "no model outputs"; it is "every load-bearing claim terminates at a source a human can read and challenge."

## What to do with a recursive chain

- **Under `bounded_recursion`:** read the flags the runtime raises (Layer 2 flags recursive chains; see chapter 9), check the bound, and decide whether the claims that lean on the recursive items are load-bearing. If they are, ask for the human-readable terminal source.
- **Under `unbounded_recursion`:** the producer has declared that chains may include any depth of model outputs. The conformance suite warns; the reviewer decides. In a high-stakes domain, unbounded recursion is usually grounds to escalate rather than accept.
- **Under `terminal` with a violation:** the COTS fails conformance. Send it back. The producer either mislabeled the item's `source_type` (fixable) or genuinely cites itself (structural).

## Tooling

- `se-trace recursive <cots.json>` walks every chain, prints the declared policy, and lists each `model-generated` item with its claim and source. Exit 1 when the terminal (or default) policy is violated.
- The conformance suite implements TC-RC-01..05: recursive items flagged or failed per the declared policy, bounds enforced, unbounded recursion warned, legacy COTSs accepted with a warning.

## Related chapters

- `01-reading-a-cots.md` -- the five parts of the COTS and where evidence sits.
- `09-reading-a-layer2-validation-record.md` -- the runtime flags that Layer 2 raises for recursive chains.
- `02-challenging-a-decision.md` -- how to contest a claim that leans on recursive evidence.

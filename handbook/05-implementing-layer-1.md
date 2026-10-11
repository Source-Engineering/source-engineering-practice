# Chapter 5 -- Implementing Layer 1 in a New Framework

> Layer 1 is the typed-model layer. Implementing it well is the discipline's engineering homework.

Layer 1 (Typed Model) accepts a typed question, returns a COTS-shaped DecisionRecord. The contract is in `Source-Engineering/source-engineering-architecture/layer-contracts/layer-1-typed-model.md`. The reference implementation in `Source-Engineering/source-engineering-reference-implementation/` is one way to compose the contract; this chapter is about composing it in your framework.

## The four guarantees

Layer 1 guarantees:

- **Structured output.** The DecisionRecord conforms to the registered decision-type's answer shape.
- **Evidence-anchored.** Every Claim in the DecisionRecord has at least one EvidenceItem. Every EvidenceItem has a Provenance. Every Provenance resolves to a SourceArtifact.
- **Counter-evidence considered.** When the model rejects a counter-position, the CounterEvidenceItems are non-empty and recorded.
- **Context recorded.** The Context for the call is in the DecisionRecord's `cots.context`.

## The conformance test cases

The conformance test cases for Layer 1 are in `Source-Engineering/source-engineering-conformance/contracts/layer-1/`. The test cases are normative; an implementation that fails any of them is not conformant.

| Test case | What it verifies |
|---|---|
| TC-L1-01 | A typed question that conforms to the schema is accepted. |
| TC-L1-02 | A typed question that does not conform is rejected with `question-schema-violation`. |
| TC-L1-03 | The model's answer conforms to the schema. |
| TC-L1-04 | Every Claim has at least one EvidenceItem. |
| TC-L1-05 | Every EvidenceItem has a Provenance that resolves to a SourceArtifact. |
| TC-L1-06 | When the model rejects a counter-position, the CounterEvidenceItems are non-empty. |
| TC-L1-07 | The DecisionRecord's `cots.context` records the runtime parameters. |

## The implementation patterns

Three patterns are common. Pick the one that matches your framework.

### Pattern A: Wrap an existing model API

Use the existing model API as a black box. The wrapper translates the typed question into a prompt, sends it to the model, parses the response, and constructs the COTS.

This is the pattern the reference implementation's `AnthropicModelAdapter` uses. The wrapper is the discipline's surface; the model API is a black box.

### Pattern B: Build the model layer

Implement the typed question and the declared answer shape directly in the model. The model returns the COTS as its native output.

This is the pattern for the Jev and similar System One models the discipline's manifesto references. The model is the discipline's surface; the wrapper is unnecessary.

### Pattern C: Compose multiple model layers

Decompose the decision into a chain of typed questions, each layer answering one part. The chain produces a COTS that records the chain as the evidence.

This is the pattern for complex domains where one model call is insufficient. The chain is the discipline's surface; each model call is a sub-decision.

## Cardinal rules (binding on every implementation)

- **No decisions leading to creating an org or repo are logged on GitHub.** The implementation's `author` and `maintainer` fields use the contributor's GitHub id or name only.
- **No reference or tagging to the coding agent or another agent in any GitHub surface.** Commit messages and PR titles use the contributor's GitHub identity only.
- **Activity/identity refers to the user's GitHub id or name only.** All commits are authored by the contributor's own GitHub identity.

## Related documents

- `Source-Engineering/source-engineering-architecture/layer-contracts/layer-1-typed-model.md` -- the Layer 1 contract.
- `Source-Engineering/source-engineering-conformance/contracts/layer-1/` -- the conformance test cases.
- `Source-Engineering/source-engineering-reference-implementation/` -- the reference implementation.
- `06-validating-a-cots.md` -- the next chapter.
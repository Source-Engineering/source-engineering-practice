# Chapter 3 -- Designing a Decision-Type

> A decision-type is the discipline's contract for a domain. Designing one is a domain-expert task.

A decision-type is a typed question with a declared answer shape. The decision-types registry is at `Source-Engineering/source-engineering-specifications/decision-types/`. Adding a new decision-type is a normative change to the discipline: the conformance suite, the reference implementation, the tools, and the practice all derive their behaviour from the registry.

## When to design a new decision-type

Design a new decision-type when:

- The discipline's existing decision-types do not cover your domain's question shape.
- Your domain has a distinct answer shape that the model should return.
- Your domain has evidence requirements that are not captured by the existing decision-types.

Do not design a new decision-type when:

- The existing decision-types cover your domain with minor extensions (use the existing decision-type and add the extensions to your guardrail rules instead).
- The decision-type is one-off and not reusable (file a case study instead; the case study surfaces the one-off behaviour without committing the discipline to a new type).

## The five sections of a decision-type

A decision-type has five sections:

1. **Identification.** The decision-type's id, name, version, domain, author, and maintainer.
2. **Question schema.** A JSON Schema (Draft 2020-12 or later) that describes the input the model accepts.
3. **Answer schema.** A JSON Schema for the model's output. The answer is always a COTS.
4. **Evidence requirements.** What evidence the COTS must carry (e.g. "at least one evidence item per material claim"; "at least one piece of counter-evidence considered").
5. **Validation rules.** What the conformance suite must verify (e.g. "the summary text does not introduce facts not supported by the document"; "the COTS's validation.feedback_surfaces includes an appeal surface").

## Worked example: a medical-triage decision-type

The first two decision-types (legal-summarization, transaction-routing) are seeded. A third decision-type the discipline might add is medical-triage. The decision-type would have:

- **Question schema.** A typed question with a patient presentation, a clinical context, and a triage intent (emergent, urgent, non-urgent).
- **Answer schema.** A COTS whose decision is one of three triage levels (emergent, urgent, non-urgent).
- **Evidence requirements.** At least one evidence item per symptom the decision cites; at least one piece of counter-evidence considered (e.g. a less-severe interpretation of the symptoms).
- **Validation rules.** The decision is one of the three declared levels; the COTS's feedback_surfaces include an appeal surface for the patient or the patient's advocate.

## The review process

A new decision-type is reviewed by the discipline's domain maintainer and one additional reviewer. The review is on:

- The question schema is well-formed and uses the discipline's controlled vocabularies.
- The answer schema is a valid JSON Schema that references the COTS schema.
- The evidence requirements are non-trivial (i.e. the decision-type is not a thin wrapper over the COTS).
- The validation rules are testable by the conformance suite.

A decision-type that fails any of these criteria is returned for revision.

## Related documents

- `Source-Engineering/source-engineering-specifications/decision-types/` -- the decision-types registry.
- `04-authoring-guardrails.md` -- the next chapter (Layer 3 guardrails for the new decision-type).
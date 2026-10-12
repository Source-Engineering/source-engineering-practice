# Practice

> Practitioner guidance for the Source Engineering discipline.

This repository holds the discipline's practitioner guidance. The practice is the discipline's adoption surface: a handbook for the engineer who has just been asked to "add Source Engineering" to a system, a guide for the legal practitioner who has just received a COTS, and a "first 90 days" plan for an engineering team adopting the discipline.

## What is in this repository

- `handbook/` - the practitioner's handbook. Eight chapters, each addressing a specific role.
  - `01-reading-a-cots.md` - how a reviewer reads a COTS in a high-stakes domain.
  - `02-challenging-a-decision.md` - how a reviewer challenges a decision using the COTS's feedback surfaces.
  - `03-designing-a-decision-type.md` - how a domain expert designs a new decision-type.
  - `04-authoring-guardrails.md` - how a domain expert authors guardrails for Layer 3.
  - `05-implementing-layer-1.md` - how an engineer implements Layer 1 in a new framework.
  - `06-validating-a-cots.md` - how an engineer validates a COTS using the conformance suite.
  - `07-auditing-a-decision-record.md` - how a regulator audits a decision record.
  - `08-rolling-out-the-discipline.md` - how an engineering lead rolls the discipline out across a team.
- `first-90-days/` - the adoption plan. A week-by-week guide for a team adopting the discipline.
- `glossary.md` - the practitioner-friendly glossary. Cross-references the foundations repo's `terminology.md`.

## How to engage

- **Read the handbook.** Start at `handbook/01-reading-a-cots.md`. The chapters are independent; read the ones that match your role.
- **Adopt the discipline.** `first-90-days/` is the plan.
- **Cross-reference.** The glossary maps practitioner terms to discipline terms; the discipline terms are canonical in `Source-Engineering/source-engineering-foundations/terminology.md`.

## Provenance

The handbook is the discipline's adoption surface. It is derived from the foundations, the model, the specifications, the architecture, the conformance suite, the reference implementation, and the tools. Every claim in the handbook is traceable to a discipline artefact.

## Status

This is the adoption slice (Phase 4). The handbook's eight chapters, the first-90-days plan, and the glossary are seeded. The handbook is the discipline's user-facing surface for the practitioner; new chapters are added as new use cases emerge.

## License

Apache-2.0. See [LICENSE](LICENSE).

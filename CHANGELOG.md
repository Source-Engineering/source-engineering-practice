# Changelog

All notable changes to the Source Engineering practice repo are recorded here.

## 2026-10-11

### Added

- `handbook/01-reading-a-cots.md` -- Chapter 1: how a reviewer reads a COTS in a high-stakes domain. The five-part reading order and the four questions.
- `handbook/02-challenging-a-decision.md` -- Chapter 2: how a reviewer challenges a decision. The four challenge types.
- `handbook/03-designing-a-decision-type.md` -- Chapter 3: how a domain expert designs a new decision-type. The five sections and the worked example.
- `handbook/04-authoring-guardrails.md` -- Chapter 4: how a domain expert authors guardrails. The DSL, the binding/advisory distinction, the worked example.
- `handbook/05-implementing-layer-1.md` -- Chapter 5: how an engineer implements Layer 1. The four guarantees, the conformance test cases, the three implementation patterns.
- `handbook/06-validating-a-cots.md` -- Chapter 6: how an engineer validates a COTS. The 12 canonical test cases, the validation workflow.
- `handbook/07-auditing-a-decision-record.md` -- Chapter 7: how a regulator audits a decision record. The five audit questions, the audit procedure.
- `handbook/08-rolling-out-the-discipline.md` -- Chapter 8: how an engineering lead rolls out the discipline. The four-stage adoption plan.
- `first-90-days/README.md` -- the operational playbook. A week-by-week guide for the first 90 days.
- `glossary.md` -- the practitioner glossary. Practitioner-friendly terms mapped to the discipline's canonical terms.
- `CHANGELOG.md` -- this file.

### Notes

- The handbook is the discipline's adoption surface. The eight chapters are independent; the reader picks the chapters that match their role.
- The first-90-days plan is the operational playbook; the strategic chapter is `handbook/08-rolling-out-the-discipline.md`.
## [Unreleased]

### Added

- `handbook/09-reading-a-layer2-validation-record.md` - how a reviewer reads the Layer 2 Validation record: the four flags, `decision_on_validation`, the reviewer's checklist, and what the record does not tell you (RFC-0001)

### Added (RFC-0002)

- `handbook/10-recursive-evidence-chains.md` - how a reviewer reads a COTS whose evidence includes model-generated items: the two RFC-0002 fields, the reviewer's three questions, what to do under each chain-termination policy, and the tooling

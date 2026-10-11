# First 90 Days -- Adoption Plan

> A week-by-week guide for an engineering team adopting the Source Engineering discipline.

This is the operational playbook. The strategic chapter is `handbook/08-rolling-out-the-discipline.md`; this plan is what your team does on Monday morning.

## Pre-week 1: Setup

- **Read the manifesto.** `Source-Engineering/source-engineering-foundations/docs/00-manifesto.md`. 30 minutes.
- **Read the four-layer stack.** `Source-Engineering/source-engineering-architecture/four-layer-stack.md`. 30 minutes.
- **Read the COTS spec.** `Source-Engineering/source-engineering-specifications/cots/README.md`. 1 hour.
- **Clone the repos.** `project-governance`, `foundations`, `model`, `specifications`, `architecture`, `conformance`, `reference-implementation`, `tools`. 30 minutes.

## Week 1 -- Vocabulary

- **Mon-Wed.** Each team member reads `terminology.md` and `handbook/01-reading-a-cots.md`. 2 hours per person.
- **Thu.** Team meeting: review the COTS spec together. Surface every question; do not resolve.
- **Fri.** Each team member writes a one-page note on the COTS: what it is, what it is not, what they don't yet understand. The notes are shared in the team channel.

**Exit criterion.** Every team member can define: COTS, source universe, evidence chain, decision record, judgment gap.

## Week 2 -- Reading (part 1)

- **Mon-Wed.** The team reads `handbook/01-reading-a-cots.md` and `handbook/02-challenging-a-decision.md`. 1 hour per person.
- **Thu.** The team reads `handbook/03-designing-a-decision-type.md`. 1 hour per person.
- **Fri.** Team meeting: the team runs the reference implementation's example and produces a COTS. The COTS is the artefact for week 2.

**Exit criterion.** The team can produce a COTS from the reference implementation's `legal-summarization` example.

## Week 3 -- Reading (part 2)

- **Mon-Wed.** The team reads `handbook/04-authoring-guardrails.md` and `handbook/05-implementing-layer-1.md`. 1 hour per person.
- **Thu.** The team reads `handbook/06-validating-a-cots.md`. 1 hour per person.
- **Fri.** The team runs the conformance suite against the example COTS. The team reads the errors. The team fixes the example COTS until it passes.

**Exit criterion.** The team can run the conformance suite and read its output.

## Week 4 -- Authoring (part 1)

- **Mon-Wed.** The team reads `handbook/07-auditing-a-decision-record.md`. 1 hour per person.
- **Thu.** The team reads `handbook/08-rolling-out-the-discipline.md` and this plan (`first-90-days/README.md`). 1 hour per person.
- **Fri.** Team meeting: the team picks a domain in production to apply the discipline to. The domain is the highest-stakes domain (the hesitation factor is the highest cost there). The team commits to the domain.

**Exit criterion.** The team has a domain.

## Weeks 5-8 -- Authoring (part 2)

- **Week 5.** The team designs a decision-type for the chosen domain. The decision-type follows `handbook/03-designing-a-decision-type.md`. The decision-type lands in `Source-Engineering/source-engineering-specifications/decision-types/<domain>/`.
- **Week 6.** The team authors guardrails for the decision-type. The guardrails follow `handbook/04-authoring-guardrails.md`. The guardrails land in the domain's guardrail ruleset.
- **Week 7.** The team implements Layer 1 in the team's framework. The implementation follows `handbook/05-implementing-layer-1.md`. The implementation passes the conformance suite's TC-L1-01..07 test cases.
- **Week 8.** The team integrates the conformance suite into CI. Every COTS produced in CI is validated; a failure blocks the merge.

**Exit criterion.** The team has a working Layer 1 implementation that produces a COTS that passes the conformance suite.

## Weeks 9-12 -- Operating (part 1)

- **Week 9.** The team's first production COTS. The COTS is reviewed by a senior engineer; the review follows `handbook/01-reading-a-cots.md`.
- **Week 10.** The team's second production COTS. The COTS is reviewed by the same senior engineer; the review is faster than the first.
- **Week 11.** The team invites a non-engineer (a domain expert, a legal reviewer, a regulator) to read a COTS. The non-engineer's reading is recorded; the recording is the basis for a case study.
- **Week 12.** The team files the first case study. The case study follows `Source-Engineering/source-engineering-case-studies/template.md`.

**Exit criterion.** The team has a case study.

## Beyond 90 days

The discipline is multi-quarter. The first 90 days are the operational playbook; the long-term is the discipline's continued growth. The team's quarterly reviews, recorded as case studies, are the discipline's evidence base.

## Related documents

- `handbook/08-rolling-out-the-discipline.md` -- the strategic chapter.
- `Source-Engineering/source-engineering-case-studies/` -- the case studies.
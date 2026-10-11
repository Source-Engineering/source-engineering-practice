# Chapter 8 -- Rolling Out the Discipline

> An engineering lead's task. The discipline is a multi-quarter adoption, not a sprint.

The discipline is not a tool you install; it is a posture you adopt. The adoption is multi-quarter because the discipline's surface (the COTS) is a per-decision artefact, and the decision's producer and reviewer must both learn the discipline's vocabulary. The "first 90 days" plan in `first-90-days/` is the operational playbook; this chapter is the strategic one.

## The four stages of adoption

The discipline's adoption has four stages. Each stage has a name, a duration, and an exit criterion.

### Stage 1 -- Vocabulary (Weeks 1-2)

The team learns the discipline's vocabulary. The vocabulary is in `Source-Engineering/source-engineering-foundations/terminology.md`. The exit criterion is a team-wide quiz: every team member can define the COTS, the source universe, the evidence chain, and the judgment gap.

### Stage 2 -- Reading (Weeks 3-6)

The team learns to read a COTS. The reading is in `handbook/01-reading-a-cots.md`. The exit criterion is a code review: every PR that produces a COTS is reviewed against the chapter's checklist.

### Stage 3 -- Authoring (Weeks 7-12)

The team learns to author a COTS. The authoring is in `handbook/05-implementing-layer-1.md` and the reference implementation in `Source-Engineering/source-engineering-reference-implementation/`. The exit criterion is a working Layer 1 implementation that passes the conformance suite.

### Stage 4 -- Operating (Weeks 13+)

The team operates the discipline in production. The operating is in `handbook/06-validating-a-cots.md` and the conformance suite. The exit criterion is a quarterly review: the COTSs in production are sampled, validated against the conformance suite, and the findings are recorded in a case study.

## The adoption's failure modes

The adoption fails when:

- **The team treats the COTS as a logging surface.** The COTS is a response surface, not a log. The COTS is what the user sees; the log is what the operator sees. The two are different.
- **The team treats the conformance suite as a one-time gate.** The conformance suite is continuous. Every COTS in production is conformant; the suite runs in CI, not as a pre-release check.
- **The team treats the discipline as a framework choice.** The discipline is a posture; the framework is a tool. The discipline outlives the framework; the framework serves the discipline.
- **The team rolls out the discipline to all domains at once.** The discipline rolls out one domain at a time. The first domain is the highest-stakes domain (where the hesitation factor is the highest cost); the discipline proves its value there before it spreads.

## Cardinal rules (binding on every rollout)

- **No decisions leading to creating an org or repo are logged on GitHub.** The rollout's `author` and `maintainer` fields use the contributor's GitHub id or name only.
- **No reference or tagging to the coding agent or another agent in any GitHub surface.** Commit messages and PR titles use the contributor's GitHub identity only.
- **Activity/identity refers to the user's GitHub id or name only.** All commits are authored by the contributor's own GitHub identity.

## Related documents

- `first-90-days/` -- the operational playbook.
- `Source-Engineering/source-engineering-foundations/` -- the discipline's vocabulary.
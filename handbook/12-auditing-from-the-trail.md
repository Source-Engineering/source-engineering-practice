# Chapter 12 -- Auditing a Decision from Its Audit Trail

> The audit trail is the substrate for appeals, re-runs, and cross-domain comparison. This chapter is how the reviewer replays a decision's lifecycle from the trail alone (RFC-0005, Layer 4).

## What the trail is

Every `DecisionRecord` carries an `audit_history`: an append-only log whose first entry is `created` and whose later entries are lifecycle events -- `appealed`, `rerun`, `corrected`, `escalated`, `superseded`. Behind the per-decision array sits the Layer 4 store (JSONL append-only in the reference implementation), which rejects rewrites outright. The conformance suite checks the trail structurally: TC-L4-01 (complete, starts with `created`), TC-L4-02 (unique ids, monotonic time), TC-L4-03 (every event resolves to a known decision), TC-L4-04 (rerun/supersession linkage), TC-L4-05 (legacy warning for created-only trails).

## The reviewer's five questions

1. **Who created this decision, and when?** The `created` entry's `performed_by` and `performed_at`. If the trail does not start with `created`, the COTS fails TC-L4-01: send it back.
2. **Was it appealed?** An `appealed` entry names the reviewer and the reason in `notes`. An appeal does not change the decision; it starts a review.
3. **Was it re-run?** A `rerun` entry must carry `rerun_of=<original id>` (TC-L4-04). Compare the re-run's decision text against the original: a different decision against the same COTS is a conformance failure or a model-side bug, depending on the model contract.
4. **Was it corrected or superseded?** A `corrected` entry should name the `affected_source_id`; a `superseded` entry names `superseded_by`. Follow those links before trusting the current text.
5. **Is the trail itself intact?** Run `se-trace audit <cots.json>`: it replays every decision's lifecycle and exits 1 on append-only or linkage violations. A trail with duplicate ids or out-of-order timestamps fails TC-L4-02 -- the log has been rewritten, and a rewritten log cannot support an appeal.

## Replaying a lifecycle

`se-trace audit <cots.json> [decision-id]` prints each decision's full lifecycle in time order and flags violations. The reference implementation's `Layer4.replay(decisionId)` does the same against the persistent store -- the discipline's signature end-to-end test: any decision reconstructable from its COTS + audit trail alone.

## Retention

The retention policy is declared by the domain, not imposed by the discipline (Layer 4 contract). The reference implementation refuses to start without one (`retention-policy-missing`). Ask the producer what theirs is; "indefinite" is a legitimate answer, "we didn't think about it" is not.

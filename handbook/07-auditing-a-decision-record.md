# Chapter 7 -- Auditing a Decision Record

> A regulator's task. The decision record is the unit of accountability.

A decision record is the unit the discipline keeps. The record carries the decision, the evidence chain, the validation status, the responsible authority, and the audit history. The record is the artefact a regulator audits.

## The five audit questions

A regulator asks five questions when auditing a decision record.

1. **What was decided?** The decision's text is in the record; the text is what was decided. If the text is ambiguous, the decision is not auditable.
2. **What evidence supported the decision?** The evidence chain is in the record; the chain links the decision to its supporting and contradicting evidence. If the chain is broken, the decision is unsupported.
3. **Was the evidence valid at the time of the decision?** The sources' temporal validity is in the record; the validity is checked against the decision's timestamp. If a source was not valid, the decision is questionable.
4. **Who was responsible?** The responsible authority is in the record; the authority is the named human or organisation that owns the decision. If the authority is missing, the decision is unowned.
5. **What has happened to the decision since?** The audit history is in the record; the history records appeals, re-runs, corrections, escalations, and supersessions. If the history is incomplete, the decision is not auditable.

## The audit procedure

1. **Request the COTS.** The COTS is the unit of audit. The request goes to the system that produced the decision; the system returns the COTS by id.
2. **Verify the COTS id.** The COTS id is a URI; the URI is the request's key. The system returns the COTS by id, or a `not-found` if the id is not in the persistence backend.
3. **Walk the decision's evidence chain.** The evidence chain is reconstructed by walking from the decision record to the evidence items, then from the evidence items to the source artifacts. The walk is a depth-first traversal; the traversal is the audit.
4. **Resolve the sources.** The sources are resolved by content hash. The audit verifies the content hash matches the content; the audit does not trust the source's identifier alone.
5. **Inspect the audit history.** The audit history is append-only; the audit verifies the history is in time order and the entries are well-formed.
6. **Record the audit.** The audit is itself a record. The audit record references the COTS it audited; the audit record is filed in the regulator's records.

## What the audit is not

- **Not a re-run.** The audit inspects the record; the audit does not re-run the decision. A re-run is a different operation (the appeal surface, in the COTS's `validation.feedback_surfaces`).
- **Not a correctness judgment.** The audit verifies the record is well-formed; the audit does not verify the decision is correct. The correctness is the responsible authority's judgment; the audit is the record's integrity.
- **Not a complaint box.** The audit is a procedure; the audit is not a forum. The findings are recorded; the findings are addressed by the responsible authority.

## Related documents

- `Source-Engineering/source-engineering-specifications/cots/README.md` -- the COTS schema.
- `Source-Engineering/source-engineering-foundations/terminology.md` -- the discipline's working vocabulary (see "Decision Record").
- `08-rolling-out-the-discipline.md` -- the next chapter.
# Chapter 6 -- Validating a COTS

> The conformance suite is the discipline's gate. Use it.

A COTS is conformant if and only if it passes the conformance suite in `Source-Engineering/source-engineering-conformance/`. The suite is normative; a COTS that fails the suite is not a Source Engineering COTS.

## The 12 canonical test cases

The conformance suite implements 12 canonical test cases. The test cases are in `Source-Engineering/source-engineering-specifications/compatibility-test-suite.md` and `Source-Engineering/source-engineering-architecture/blueprints/v0.1/README.md`.

| Test case | What it verifies | What failure looks like |
|---|---|---|
| TC-SI-01 | A source artifact has a stable identifier and an addressable version. | The source id is not a URI or the content hash is not a SHA-256. |
| TC-SI-02 | A source artifact id is malformed. | The source id is not a valid URI. |
| TC-CI-01 | The context records the runtime parameters. | A required parameter is missing. |
| TC-CI-02 | A source's temporal validity is checked against the decision's timestamp. | The valid_until timestamp is unparseable. |
| TC-CI-03 | A source has expired before the decision. | The source's valid_until is before the COTS's produced_at. (Warning, not error.) |
| TC-EI-01 | Every claim carries at least one evidence item, and each has a provenance. | An evidence item's source_id does not resolve. |
| TC-EI-02 | A claim has no supporting or contradicting evidence. | A claim's evidence arrays are empty. |
| TC-DI-01 | A decision carries a basis_id. | The basis_id is missing or malformed. |
| TC-DI-02 | A decision has no counter-evidence. | The evidence chain's contradicting_evidence is empty. |
| TC-LI-01 | A decision record carries a complete audit trail. | The audit_history is empty or does not start with a "created" entry. |
| TC-LI-02 | The audit history is in time order. | An entry is earlier than the previous entry. |
| TC-FF-01 | The COTS has all five parts. | One of the parts is empty. |
| TC-FF-02 | The validation feedback surfaces are missing. | The feedback_surfaces array is empty. (Warning, not error.) |

## The validation workflow

1. **Run the conformance suite.** `npx se-conformance validate path/to/cots.json` returns exit 0 on pass, 1 on fail.
2. **Read the errors.** Each error includes the test id and a human-readable message. The error tells you what to fix.
3. **Fix the COTS.** The fix is at the producer; the discipline's surface is the COTS, not the producer.
4. **Re-run the suite.** The fix is verified by re-running the suite. The COTS is conformant when the suite passes.

## When to fail loudly

A COTS that fails any of the test cases is not a Source Engineering COTS. The discipline's value is in the conformance; a non-conformant COTS is worse than no COTS, because it is presented as auditable but is not.

The producer is responsible for the conformance. The reviewer is responsible for running the suite. The discipline does not provide a "soft pass" or a "best effort" mode.

## Related documents

- `Source-Engineering/source-engineering-conformance/` -- the conformance suite.
- `Source-Engineering/source-engineering-specifications/compatibility-test-suite.md` -- the canonical test cases.
- `Source-Engineering/source-engineering-architecture/blueprints/v0.1/` -- the layer-1 test cases.
- `07-auditing-a-decision-record.md` -- the next chapter.
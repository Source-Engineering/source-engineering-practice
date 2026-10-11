# Chapter 4 -- Authoring Guardrails

> Layer 3 guardrails encode the domain's policy. A guardrail is not a prompt; it is a rule with a reference to the COTS.

Layer 3 (Business Guardrails) is the layer that applies the domain's policy to a decision. The guardrails are authored by domain experts; the engineering team wires them into the framework. The guardrail DSL is declared in `Source-Engineering/source-engineering-architecture/blueprints/v0.1/`; the rules themselves live with the domain.

## The distinction between binding and advisory rules

A guardrail rule is one of:

- **Binding.** A binding rule rejects the decision. The rejection is recorded with the rationale. The discipline does not silently drop the response.
- **Advisory.** An advisory rule annotates the decision. The annotation is recorded; the user decides whether to act.

The distinction is normative. A guardrail that is silent about its type is treated as binding; the discipline errs on the side of caution.

## The guardrail DSL

A guardrail rule has the form:

```
rule <rule-id> {
  when <condition-on-cots>
  then <action: reject | annotate | escalate>
  with rationale "<rationale-in-domain-terms>"
}
```

The condition is a query against the COTS. The query is in the discipline's guardrail DSL; the DSL is declarative and references the COTS schema.

## Worked example: a financial-jurisdiction guardrail

```
rule jurisdiction-us-only {
  when cots.context.constraints.where(type == "jurisdiction").any(value != "US-*")
  then annotate
  with rationale "The decision is not restricted to US jurisdictions. Verify the cross-border implications before acting."
}
```

This rule checks the COTS's context for a jurisdiction constraint; if the constraint is missing or not US-prefixed, the rule annotates the decision with the rationale. The annotation is recorded in the COTS; the user sees the annotation.

## Authoring workflow

1. **Identify the policy.** The policy is the domain's rule; the rule is the discipline's expression of the policy.
2. **Identify the COTS field.** The COTS field is the data the rule needs (e.g. the jurisdiction constraint, the decision's text, the evidence's source).
3. **Write the rule.** The rule is a single guardrail; the rule's `when` clause is a query against the COTS.
4. **Test the rule.** The rule is run against the conformance suite's fixtures; the rule is expected to pass on the conformant COTSs and to annotate the malformed COTSs.
5. **Commit the rule.** The rule lands in the domain's guardrail ruleset; the ruleset is versioned.

## Cardinal rules (binding on every guardrail)

- **No decisions leading to creating an org or repo are logged on GitHub.** The guardrail's `author` and `maintainer` fields use the contributor's GitHub id or name only.
- **No reference or tagging to the coding agent or another agent in any GitHub surface.** Commit messages and PR titles use the contributor's GitHub identity only.
- **Activity/identity refers to the user's GitHub id or name only.** All commits are authored by the contributor's own GitHub identity.

## Related documents

- `Source-Engineering/source-engineering-architecture/blueprints/v0.1/` -- the guardrail DSL.
- `Source-Engineering/source-engineering-architecture/layer-contracts/layer-3-business-guardrails.md` -- the Layer 3 contract.
- `05-implementing-layer-1.md` -- the next chapter.
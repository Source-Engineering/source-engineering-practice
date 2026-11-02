# Chapter 11 -- Reading the Context Graph

> The runtime context made visible. When a decision was made under a context you cannot see, you are back to trusting the operator. The Context Graph (RFC-0004) is the fix: the COTS's Context and Evidence parts rendered as a graph you can read.

## What the Context Graph is

A Context Graph is the graph-structured rendering of a COTS's `context` and `evidence` parts. Nodes are context parameters, constraints, source artifacts, claims, and decisions. Edges are typed: `set-by` (context -> parameter), `constrains` (constraint -> what it constrains), `contextualises` (parameter -> decision), `supports` and `contradicts` (source -> claim). It is produced from the COTS alone by `se-trace context-graph <cots.json> -o ./out`, which writes a Mermaid file and a self-contained HTML page. No schema change is required; the graph is derived, not stored.

The contract (RFC-0004): a COTS's context is **surfaced** when the graph is producible from the COTS alone, user-side. "Surfaced" means visible and reproducible; it does not mean the user can change the parameters at runtime.

## The reviewer's three questions

1. **What was the model running as?** Read the parameter nodes: temperature, top-p, prompt cache, tool budget, retrieval depth. A high temperature under a low-stakes decision is fine; under a load-bearing decision, ask why. The values are data points in the COTS, not developer settings hidden behind an API.
2. **What constrained the call?** Read the constraint nodes and their `constrains` edges. A jurisdiction constraint that touches every decision changes how you weigh the whole COTS.
3. **Which decisions were made under this context?** Follow the `contextualises` edges from parameters to decisions. Every decision should carry them (the conformance suite enforces this: TC-CG-03). A decision with no contextualising edge is a decision you cannot place in its context -- send it back.

## Reading recursion in the graph

Model-generated evidence items (RFC-0002) render in a distinct style in the Context Graph, so a recursive chain is visible at a glance rather than only via `se-trace recursive`. If a load-bearing claim's path to a human-readable source passes through a model-generated node, apply chapter 10's standard: ask for the terminal source.

## Tooling

- `se-trace context-graph <cots.json> [-o dir]` renders the graph as Mermaid + HTML.
- `se-trace visualize <cots.json>` renders the Reasoning Roadmap (sources -> claims -> decision); the two renderings are complementary, not interchangeable.
- The conformance suite implements TC-CG-01..03: parameters surfaced, constraints surfaced, decisions contextualised.

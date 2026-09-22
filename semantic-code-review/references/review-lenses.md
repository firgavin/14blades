# Semantic review lenses

Read the sections relevant to the changed behavior. These lenses deepen review judgment; they are not a requirement to manufacture one finding per heading.

## Intent and interface contracts

Trace both sides of every changed interface. Confirm implementations and consumers agree on success, errors, cancellation, timing, ownership, mutation, durability, and disposal. A type-compatible change can still violate the behavioral contract.

When several representations of one outcome cross a public interface, check that the owning layer normalizes them consistently. Callers should not need to guess which provider, wrapper, callback, or transport produced an equivalent failure.

## Lifecycle and concurrency

For asynchronous setup, callbacks, processes, workers, streams, and teardown, check:

- races before publication and cancellation during awaits;
- ownership before reentry or user callbacks;
- independent reporting of orthogonal outcomes such as timeout, exit status, and signal;
- callback containment so one subscriber cannot reject core work or starve later subscribers;
- listener removal before late completions can publish;
- rollback after partial initialization;
- disposal to quiescence: request stop, await termination, then resolve disposal.

Do not treat broad idle or status transitions as the completion of one operation unless the contract defines that interval and attribution.

## Abstraction and consumer fit

Trace every current production consumer. Flag consumer-specific policy leaking into a generic interface. Also flag the inverse: a public method on a generic service whose only caller is one internal consumer may be an unnecessary API expansion; a private capability passed to that consumer can preserve narrower ownership.

Challenge speculative state machines, options, compatibility layers, and defensive copies. Keep them when a current contract, trust boundary, or hard-won failure mode needs them; otherwise ask whether they move complexity rather than remove it.

## Configuration and public choices

Ask what supports each default, public operation set, format, and imported external concept. Current consumers, an external standard, or prior art may justify a choice. "It is configurable" does not justify an arbitrary default.

Misconfiguration should fail at the earliest point where the required referent and context are known. Silent skipping usually converts a configuration defect into later undefined behavior.

## Security and enforcement

Follow each authorization or denial path to the operation that performs the effect. Exercise direct and alternate callers that can bypass presentation, schemas, prompts, wrappers, middleware order, or convenience APIs.

For processes and files, inspect ambient authority, credential exposure, temporary-path predictability, permissions, link traversal, and cleanup ownership. Treat data crossing parser, wire, process, worker, durable-storage, or model/tool boundaries as untrusted according to the repository's threat model; do not invent hostile validation at typed same-process boundaries without a plausible source.

## Borrowed, owned, and derived state

Determine whether retained data is borrowed, copied, transferred, or reconstructed. Trace the documented success point and authoritative source through notifications, caches, prompts, rendered output, replay, and queries. A failure before commit should not publish a success-derived view.

## Bounds and complete results

Locate the owner of the complete emitted or retained value. Probe zero or tiny limits, exact limits, oversized single chunks, aggregation across chunks, wrappers and metadata, and multibyte text when limits are measured in bytes.

## Real entry paths

Identify the path users or integrations actually run: package export, framework loader, command-line binary, built artifact, worker entry, subprocess, RPC bridge, browser, or deployed service. Hand assembly can hide export, module-resolution, startup, serialization, environment, and teardown failures.

## Test strength

Prefer assertions that fail on the intended regression and observe external state. Re-read written files, query durable records, inspect emitted events, verify resource release, or call the public interface independently. A component reporting "success" is not proof that the world changed.

Coverage is necessary only where the project requires it and never sufficient. Mocks should replace expensive or nondeterministic boundaries, not the implementation whose integration is under review.

For guards and invariants, require negative controls through the real runner. A test of a helper predicate does not prove the invalid state is rejected before publication or effect execution.

## Visible and recorded behavior

Inspect exact prompts, schemas, responses, diagnostics, logs, terminal output, editor output, and UI states that affected users or automated consumers receive. Review snapshot diffs as behavior changes, not formatting noise. Verify stable wording exactly when callers or models rely on it, and use scenario coverage for dynamic behavior.

## Documentation and durable rationale

Changed behavior should update the documentation and API prose that callers rely on. Comments state non-obvious contracts and rationale; they should not narrate control flow, tests, the current pull request, or review history.

When prose depends on design-session shorthand, pull-request vantage, reviewer dialogue, or unresolvable citations, apply the `trim-reasoning-transcripts` skill if available. Preserve each load-bearing fact while rewriting from the repository's current state.

Treat decision records as evidence of intent and tradeoffs. When shipped behavior diverges, report the mismatch and determine which source should change; do not use the record as an automatic veto or silently rewrite history.

## Localization and generated artifacts

When a project maintains paired languages or generated references, compare meaning rather than trusting hashes or freshness checks. Update the owning source before derivative output, then regenerate or re-record through the repository's workflow.

---
name: semantic-code-review
description: Review pull requests or code changes for behavioral correctness beyond mechanical checks, including live scope, interface consumers, lifecycle, enforcement, ownership, bounds, real entry paths, observable tests, documentation, and evidence-backed findings.
---

# Semantic Code Review

This skill is guidance, not a complete checklist. Read enough surrounding code and repository guidance to understand the design, then prioritize correctness, lifecycle, security, and broken required behavior over style. A short review with one substantiated blocker is better than a list of nits.

## Establish the review scope

- For a pull request, verify its live base and exact head from the hosting service before reading the diff. Refresh them after a retarget, merge, rebase, or force-push.
- Inspect the complete committed range and any staged, unstaged, or untracked changes relevant to the requested review. Do not infer a stacked or newly created branch's base from its local upstream.
- For a local change without a pull request, identify the intended base, head, and worktree layers explicitly. If the base cannot be established safely, report that limitation instead of reviewing an invented range.
- Read the applicable repository instructions, architecture documents, decision records, testing policy, and documentation rules. Treat them as evidence of intent, not as substitutes for checking shipped behavior or as automatic vetoes against better evidence.
- Review and audit requests are read-only. Apply fixes, post comments, submit reviews, or change remote state only when the user explicitly authorizes those actions.

Use a repository-provided scope reporter when one exists. Its path inventory is factual input, not semantic review and not automatic test selection.

## Review the change

1. Reconstruct the intended behavior from the request, design records, public interfaces, and current consumers. Note contradictions instead of silently choosing one source.
2. Trace both sides of each changed interface: caller and callee, producer and consumer, registration and disposal, write and read, encode and decode, or request and response.
3. Identify the owner of every new abstraction, state machine, option, cache, compatibility path, default, and defensive copy. Require a current contract, production consumer, external specification, or explicit product decision.
4. Examine correctness through the relevant lenses in [references/review-lenses.md](references/review-lenses.md). Do not apply unrelated lenses mechanically.
5. Inspect the strongest evidence for each affected behavior. Prefer focused tests that fail on the intended regression, exercise the real shipped entry path, and observe external state or durable output.
6. Review changed prose semantically. Public behavior, configuration, failures, timing, ownership, visible output, and limitations must agree with the code. Automated documentation checks do not prove accuracy or completeness.
7. Inspect user-, operator-, protocol-, or model-visible output as behavior. Stable text may require exact assertions; dynamic journeys may require snapshots or end-to-end coverage under the repository's policy.

## Required judgments

- **Lifecycle reaches settlement.** Cancellation and disposal must stop or join owned work, detach listeners, contain callback failures, and return only at the documented completion point.
- **Enforcement reaches the operation.** Schema omission, prompts, UI hiding, facades, wrappers, and listener order are not enforcement when direct or alternate callers can bypass them.
- **State has one authority.** Publish notifications and derived views only after success. Trace caches, projections, replay, UI echoes, and queries back to the authoritative commit point.
- **Bounds cover the complete value.** Apply byte, token, item, and time limits where the final emitted or retained result, including wrappers and metadata, is known.
- **Public choices have evidence.** Configurability alone does not justify a default, operation set, wire format, or imported concept. Require current-consumer evidence, a relevant standard, or an explicit decision.
- **Tests verify the world.** Coverage proves execution, not correctness. Assertions should observe files, events, logs, responses, resource release, or other external effects rather than trusting the component's self-report.
- **The real entry path is covered.** Exercise the loader, binary, worker, process, transport, built artifact, framework composition, or deployment mode that can fail differently from a hand-assembled unit test.
- **Negative controls are real.** When a guard, invariant, denial, or regression test matters, deliberately introduce or exercise the invalid case and prove the real runner rejects it for the intended reason.

## Assess evidence proportionally

Select the narrowest evidence that can fail for the affected behavior. Add broader tests only when the change crosses contracts or no narrower entry path is credible. Do not demand a full suite reflexively, repeat a green mechanical check as a review finding, or accept CI as proof of semantics.

When evidence is missing, distinguish among:

- a demonstrated defect;
- a required behavior with no adequate proof;
- an environmental limitation;
- a non-blocking suggestion for stronger assurance.

## Report findings

For every finding, state the defect, location, impact, and evidence. Put a localized defect on the tightest relevant range; use a review-level finding for cross-cutting architecture, scope, or lifecycle problems.

Separate blockers from suggestions. Omit style preferences and issues already conclusively enforced by a green gate unless the gate is misconfigured or bypassed. If there are no actionable findings, say so and identify any material residual risk or unverified path.

Evaluate review responses on technical grounds. Fix or rebut a claim with evidence; do not perform agreement merely because a reviewer asserted it.

---
name: agile-department-workflow
description: "Use when planning or running a software project with a multi-thread agile department workflow: a control thread coordinates product, engineering, and review threads; product writes Specs without code changes; engineering runs technical discovery and implements only approved Specs; review audits Specs, architecture options, designs, diffs, tests, and evidence; handoffs are stored in a local .agent-workspace/ with active and archived iterations. Trigger for product-engineering-review workflows, product discovery, technical discovery, agile iteration planning, Spec-driven development, multi-dialog agent collaboration, local handoff documents, or separating planning documents from code."
---

# Agile Department Workflow

Coordinate one software project through four separate conversations:

- `control`: own status, routing, and handoffs.
- `product`: write Specs, stories, priorities, and acceptance criteria. Do not edit code.
- `engineering`: run technical discovery, propose architecture options, and implement only approved Specs.
- `review`: audit requirements, architecture options, designs, diffs, tests, security risks, and completion evidence. Do not edit code by default.

Use files in `.agent-workspace/` as the source of truth for cross-thread handoff. Do not rely on chat memory as the handoff record.

`agents/openai.yaml` is UI metadata for skill discovery. It does not define department authority, runtime behavior, or project workflow state.

## Core Rules

1. Product owns requirement meaning.
2. Engineering owns implementation design.
3. Review owns quality release judgment.
4. Control owns workflow state and dispatch.
5. Product must not modify source code, tests, build scripts, or implementation files.
6. Engineering must not rewrite Product Specs or acceptance criteria.
7. Review must not modify code unless the control thread explicitly changes the task into a review-fix task.
8. Control is the only role that updates global `status.md`.
9. `.agent-workspace/` is local collaboration state and should not be committed.
10. Completed iteration documents move to `archive/`; history is append-only.
11. A completion claim requires evidence: tests, diffs, logs, screenshots, command output, or written acceptance results.
12. Master plans must be based on confirmed product discovery and technical discovery, not guessed product intent or guessed technology choices.

## Operating Modes

- **Plan-only**: explain the workflow or produce a rollout plan. Do not create files.
- **Bootstrap**: create `.agent-workspace/` structure, initial `status.md`, and templates. Add `.agent-workspace/` to `.git/info/exclude` unless the user asks for shared `.gitignore`.
- **Run**: read `status.md`, dispatch the next owner, update handoff records, and advance the state.
- **Audit**: inspect `.agent-workspace/` and report missing documents, role violations, stale active work, or weak completion evidence.

If the user says "do not create", "plan only", or equivalent, use Plan-only mode.

## Workflow

1. Determine mode.
2. If the user gave a new idea or asks for a master plan, run Product Discovery first, then Technical Discovery. Read `references/state-machine.md` and `references/templates.md`.
3. If running or bootstrapping, inspect whether `.agent-workspace/` exists. Read `references/workspace-layout.md` before creating or modifying workspace documents.
4. Identify the current state from `.agent-workspace/status.md`. Read `references/state-machine.md` before changing state.
5. Route work to the next owner:
   - Product work: read `references/department-roles.md` and `references/templates.md`.
   - Engineering work: require an approved Spec, then read `references/review-gates.md` and `references/templates.md`.
   - Review work: read `references/review-gates.md`.
   - Cross-thread dispatch: read `references/thread-orchestration.md` if thread tools are available or the user asks to create/send to threads.
6. Write a handoff file before asking another department to act. Read `references/handoff-protocol.md`.
7. Keep current work in `active/iteration-current/`.
8. At iteration close, write `final-summary.md`, move completed iteration documents into `archive/iteration-NNN/`, and return unfinished items to `backlog.md`. Read `references/iteration-rules.md`.
9. Report the current phase, next owner, files changed, evidence collected, and blockers.

## Clarification Protocol

During Product Discovery and Technical Discovery:

- Ask only 1-3 high-impact questions per user turn.
- Use a structured user-input or multiple-choice tool when the runtime provides one; otherwise ask in normal chat.
- Provide 2-4 mutually exclusive options when the user says they do not know, when the decision is technical, or when a default choice would shape scope, cost, stack, hosting, data, auth, or compliance.
- Mark any selected default made without user certainty as `Assumption` and pair it with a `Risk`.
- Do not include a draft `master-plan.md`, stack decision, roadmap, or iteration breakdown in the same response that asks missing gate questions.

## Product Discovery Gate

Before generating `master-plan.md`, confirm these fields:

- Target user.
- Core problem.
- Core user flow.
- MVP scope.
- Explicit non-goals.
- Success or acceptance criteria.
- Initial target platform preference.
- Business constraints: time, budget, market, operations, compliance, or stakeholders.

If any of target user, core problem, core user flow, MVP scope, or success criteria is missing, do not enter Technical Discovery. Ask concise product questions through the Clarification Protocol and set phase to `product-discovery` or `product-discovery-blocked`.

## Technical Discovery Gate

Before generating `master-plan.md`, Engineering must confirm these fields:

- New project or existing project.
- Target platform.
- Existing stack to preserve, or permission to choose a new stack.
- Required compatibility with existing systems, APIs, databases, or workflows.
- Data persistence needs.
- Login, permission, role, organization, or multi-tenant needs.
- Third-party integrations.
- Real-time collaboration, notifications, file upload, search, or other platform capabilities.
- Deployment target.
- Expected user or data scale.
- Security, privacy, compliance, performance, offline, internationalization, or observability constraints.

If any of new/existing project, target platform, existing stack constraint, data persistence, login/permission needs, deployment target, or key non-functional constraints is missing, do not enter `master-planning`. Engineering should ask concise technical questions through the Clarification Protocol. If the user does not know an answer, provide 2-4 options and mark any selected default as an `Assumption` with a `Risk`.

## Default State Flow

```text
idea
product-discovery
product-discovery-blocked
product-ready
technical-discovery
technical-discovery-blocked
technical-review
tech-ready
master-planning
spec-draft
spec-review
approved-for-dev
engineering-design
design-review
ready-for-iteration
in-development
code-review
product-acceptance
done
archived
blocked
```

`blocked` is an exceptional side state reachable from any phase. Preserve `Previous Phase`, `Unblock Owner`, and `Unblock Criteria`, then return to the previous phase after the blocker is resolved.

Only move forward when the required gate passes. If a gate fails, route back to the responsible department:

- Spec review fails -> product revises the Spec.
- Design review fails -> engineering revises `design.md` or `tasks.md`.
- Code review fails -> engineering fixes code and updates verification.
- Product acceptance fails -> control decides whether the issue is product clarification or engineering correction.

## Required Checkpoints

STOP before:

- Creating or renaming thread conversations unless the user explicitly asked for separate threads.
- Generating `master-plan.md` when Product Discovery Gate or Technical Discovery Gate is incomplete.
- Choosing a technology stack before Technical Discovery Gate is complete.
- Entering `master-planning` before Product Discovery, Technical Discovery, and technical review are complete.
- Starting code work when the active Spec is not `approved-for-dev`.
- Marking a story done without review evidence and product acceptance.
- Repairing or recreating `status.md` when evidence conflicts or recovery confidence is `low`.
- Editing `archive/` documents directly.
- Committing `.agent-workspace/` files to Git.
- Bootstrapping when `.agent-workspace/` is already tracked by Git.

If a checkpoint blocks progress, write the blocker in `status.md` and ask the user only when control cannot resolve it from existing documents.

## Failure Handling

| Trigger | Action |
|---|---|
| `.agent-workspace/` missing in Run mode | Switch to Bootstrap proposal or ask to initialize. Do not invent state in chat only. |
| Missing product discovery fields before technical discovery | Ask Product Discovery questions. Do not create `master-plan.md`. |
| Missing technical discovery fields before master planning | Ask Technical Discovery questions or present options. Do not choose a stack silently. |
| `status.md` missing and workspace is empty | Create initial `status.md` from `references/templates.md`. |
| `status.md` missing but workspace has active/archive/inbox documents | Write `recovery-report.md`; repair only when confidence is `high`. |
| `status.md` inconsistent with documents | Write `recovery-report.md`; if confidence is `low`, enter `blocked`. |
| `.agent-workspace/` is already tracked by Git | STOP and ask whether to remove it from Git tracking outside this skill workflow. |
| Bootstrap partially succeeds | Write `bootstrap-report.md`, mark the failed paths, and do not advance state until repaired. |
| Spec lacks acceptance criteria | Route to Product with `request-changes`. Engineering must not start coding. |
| Approved Spec conflicts with code reality | Route to Engineering for feasibility note, then Review for impact decision. |
| Architecture options fail technical review | Route to Engineering for revised `architecture-options.md`; do not enter `master-planning`. |
| Review finds blocking issue | Keep state at the review phase and route changes to the owning department. |
| Iteration has unfinished stories | Archive completed evidence, return unfinished stories to `backlog.md`, and list them in `final-summary.md`. |
| Thread tool unavailable | Continue in semi-automatic mode: write handoff files and give the user the exact prompt to send. |

## Anti-Patterns

Do not:

- Let Product edit source code.
- Let Engineering change acceptance criteria to match the implementation.
- Let Review approve from summaries without inspecting evidence.
- Treat a draft Spec as implementation authorization.
- Generate a master plan by guessing the user's product intent.
- Choose a technology stack by guessing deployment, data, auth, or integration constraints.
- Let Product make final technical stack decisions without Engineering options and Review risk check.
- Delete old iteration documents to reduce clutter.
- Rewrite archive history; add amendments instead.
- Use chat messages as the only record of decisions.
- Start multiple code-writing threads on the same working tree without an explicit concurrency plan.
- Repair low-confidence workflow state without a recovery report and confirmation.

## Reference Map

- `references/workspace-layout.md`: `.agent-workspace/` structure, local Git exclusion, active/archive layout.
- `references/department-roles.md`: role permissions, prompts, and ownership.
- `references/state-machine.md`: phases, transitions, gates, and rollback routing.
- `references/handoff-protocol.md`: handoff file naming and required fields.
- `references/iteration-rules.md`: master planning, sprint flow, archive rules, amendments.
- `references/review-gates.md`: Definition of Ready, Definition of Done, review verdicts.
- `references/thread-orchestration.md`: how control coordinates separate agent threads.
- `references/templates.md`: copy-ready templates for Specs, status, handoffs, design, reviews, summaries, and retros.

# State Machine

Root state is stored in `.agent-workspace/status.md`.

## Phases

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

`blocked` is an exceptional side state. Any phase may move to `blocked`; when unblocked, return to `Previous Phase`.

## Product Discovery Gate

Before `technical-discovery`, Product or Control must capture:

- Target user.
- Core problem.
- Core user flow.
- MVP scope.
- Explicit non-goals.
- Success or acceptance criteria.
- Initial target platform preference.
- Business constraints: time, budget, market, operations, compliance, or stakeholders.

Hard gate:

```text
If target user, core problem, core user flow, MVP scope, or success criteria is missing, do not enter technical-discovery.
```

Ask 1-3 concise questions per round. Use a structured user-input or multiple-choice tool when available; otherwise ask in normal chat. Set `Current Phase: product-discovery` or `product-discovery-blocked`.

## Technical Discovery Gate

Before `master-planning`, Engineering must capture:

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

Hard gate:

```text
If new/existing project status, target platform, stack constraint, data persistence, auth/permission needs, deployment target, or key non-functional constraints are missing, do not enter master-planning.
```

Ask 1-3 concise technical questions per round. Use a structured user-input or multiple-choice tool when available; otherwise ask in normal chat.

If the user does not know an answer, Engineering must offer 2-4 options. Any default chosen without user certainty must be listed under `Assumptions` with a matching `Risk`.

## Phase Transition Matrix

| Current Phase | Owner | Required Evidence | Required Output Files | status.md Updates | Next Phase | Rollback Route |
|---|---|---|---|---|---|---|
| `idea` | Control | User idea recorded | optional intake note | `Current Phase: product-discovery`; `Next Owner: product` | `product-discovery` | `blocked` if user intent is unavailable |
| `product-discovery` | Product | Product Discovery Gate complete | `active/iteration-current/product/product-discovery.md` or `product/product-discovery.md` | `Current Phase: product-ready`; `Active Spec: SPEC-000-master`; `Next Owner: engineering` | `product-ready` | `product-discovery-blocked` |
| `product-discovery-blocked` | Product/User | Missing product answers supplied | updated `product-discovery.md` | `Current Phase: product-discovery`; `Blocked: false`; `Next Owner: product` | `product-discovery` | stay blocked |
| `product-ready` | Control | Product gate complete | readiness handoff | `Current Phase: technical-discovery`; `Next Owner: engineering` | `technical-discovery` | `product-discovery` |
| `technical-discovery` | Engineering | Technical Discovery Gate complete | `engineering/technical-discovery.md`, `engineering/feasibility.md`, `engineering/architecture-options.md` | `Current Phase: technical-review`; `Next Owner: review` | `technical-review` | `technical-discovery-blocked` |
| `technical-discovery-blocked` | Engineering/User | Missing technical answers supplied | updated `technical-discovery.md` | `Current Phase: technical-discovery`; `Blocked: false`; `Next Owner: engineering` | `technical-discovery` | stay blocked |
| `technical-review` | Review | Technical review verdict `approved` or accepted comments | `review/technical-review.md` | `Current Phase: tech-ready`; `Next Owner: control` | `tech-ready` | `technical-discovery` |
| `tech-ready` | Control | Product and technical gates complete; technical review approved | master-planning handoff | `Current Phase: master-planning`; `Next Owner: product` | `master-planning` | `technical-review` |
| `master-planning` | Product + Engineering + Review | Product vision, master Spec, feasibility, architecture options, technical review, risk gates, backlog | `master-plan.md`, `roadmap.md`, `backlog.md`, supporting department docs | `Current Phase: spec-draft`; `Next Owner: product`; `Active Iteration: iteration-current` | `spec-draft` | missing gate phase |
| `spec-draft` | Product | Product Spec with testable acceptance criteria | `active/iteration-current/product/specs/SPEC-NNN.md` | `Current Phase: spec-review`; `Active Spec: SPEC-NNN`; `Next Owner: review` | `spec-review` | `product-discovery` |
| `spec-review` | Review | Review verdict `approved` or accepted comments | `active/iteration-current/review/spec-review.md` | `Current Phase: approved-for-dev`; `Next Owner: engineering` | `approved-for-dev` | `spec-draft` |
| `approved-for-dev` | Control | Story selected for iteration | sprint backlog update | `Current Phase: engineering-design`; `Next Owner: engineering` | `engineering-design` | `spec-review` |
| `engineering-design` | Engineering | Design and tasks written | `engineering/design.md`, `engineering/tasks.md` | `Current Phase: design-review`; `Next Owner: review` | `design-review` | `approved-for-dev` |
| `design-review` | Review | Review verdict `approved` | `review/design-review.md` | `Current Phase: ready-for-iteration`; `Next Owner: engineering` | `ready-for-iteration` | `engineering-design` |
| `ready-for-iteration` | Control | Iteration goal and sprint backlog set | `goal.md`, `sprint-backlog.md` | `Current Phase: in-development`; `Next Owner: engineering` | `in-development` | `design-review` |
| `in-development` | Engineering | Implementation and verification evidence | source changes, tests, `engineering/verification.md` | `Current Phase: code-review`; `Next Owner: review` | `code-review` | `engineering-design` |
| `code-review` | Review | Review verdict `approved` | `review/code-review.md` | `Current Phase: product-acceptance`; `Next Owner: product` | `product-acceptance` | `in-development` |
| `product-acceptance` | Product | Business acceptance against Spec | `review/acceptance-review.md` or product acceptance note | `Current Phase: done`; `Next Owner: control` | `done` | control chooses product clarification or engineering correction |
| `done` | Control | Final summary and archive complete | `archive/iteration-NNN/final-summary.md` | `Current Phase: archived`; `Active Iteration: none`; `Next Owner: control` | `archived` | `product-acceptance` |

## Rejection Routing

| Failed gate | Route to | Required output |
|---|---|---|
| Product discovery incomplete | Product | Updated `product-discovery.md` or answered questions |
| Technical discovery incomplete | Engineering | Updated `technical-discovery.md` or selected assumptions with risks |
| Technical review rejects options | Engineering | Revised `architecture-options.md` and `feasibility.md` |
| Spec unclear | Product | Revised Spec or answered questions |
| Spec impossible or too broad | Product + Engineering | Scope reduction or feasibility note |
| Design unsafe or incomplete | Engineering | Revised `design.md` and `tasks.md` |
| Code review failure | Engineering | Fix, updated tests, updated verification |
| Product acceptance failure | Control | Decision: product clarification or engineering fix |

## Blocked State

Use `blocked` only when the current owner cannot act without missing input, unavailable credentials, external service access, or conflicting documents. Record:

```text
Previous Phase:
Blocked By:
Blocked Since:
Missing Decision:
Owner Needed:
Unblock Owner:
Unblock Criteria:
Next Unblock Action:
```

## Status Recovery Gate

When `status.md` is missing or inconsistent:

1. Inspect `active/`, `archive/`, `inbox/`, and the newest handoff files.
2. Write `.agent-workspace/recovery-report.md`.
3. Set `Confidence: high | medium | low`.
4. Repair `status.md` only when confidence is `high`.
5. If confidence is `medium`, ask control or the user to confirm the proposed repair.
6. If confidence is `low`, set `Current Phase: blocked` and do not overwrite `status.md`.

# Iteration Rules

## Master Planning

For a new idea, build the big plan before coding:

1. Product or Control runs Product Discovery.
2. Control verifies the Product Discovery Gate.
3. Engineering runs Technical Discovery.
4. Engineering writes `feasibility.md` and `architecture-options.md`.
5. Review writes `technical-review.md`.
6. Product writes vision and master Spec.
7. Review writes risk assessment and quality gates.
8. Control merges those into `master-plan.md`, `roadmap.md`, and `backlog.md`.

No production code changes occur in master planning unless the user explicitly asks for a technical spike.

If Product Discovery lacks target user, core problem, core user flow, MVP scope, or success criteria, ask questions and do not create `master-plan.md`.

If Technical Discovery lacks new/existing project status, target platform, stack constraints, data persistence, login/permission needs, deployment target, or key non-functional constraints, Engineering asks questions or offers options and does not choose a stack silently.

If Review returns `request-changes` for `technical-review.md`, route back to Engineering for revised `architecture-options.md` and do not enter `master-planning`.

## Iteration Flow

1. Product selects candidate stories.
2. Review checks Definition of Ready.
3. Engineering writes design and task plan.
4. Review checks design.
5. Engineering implements.
6. Review checks code, tests, security, and evidence.
7. Product performs business acceptance.
8. Control writes retro and final summary.
9. Control archives the iteration.

## Archive Process

When an iteration closes:

1. Write `final-summary.md`.
2. Copy or move `active/iteration-current/` to `archive/iteration-NNN/`.
3. Preserve handoffs, reviews, verification, and retro.
4. Return unfinished stories to `backlog.md`.
5. Create a fresh `active/iteration-current/` for the next iteration.

If unfinished stories remain, list them in `final-summary.md` and copy each item into `backlog.md` before archiving. Do not describe an unfinished story as shipped.

If archive move or copy fails, keep `active/iteration-current/` intact, write the failure in `final-summary.md` or `bootstrap-report.md`, and do not set phase to `archived`.

## Final Summary Fields

```text
# Iteration NNN Final Summary

Status:
Started:
Completed:
Active Specs:
Completed Stories:
Unfinished Stories:
Shipped Changes:
Validation Evidence:
Review Result:
Product Acceptance:
Known Risks:
Follow-up Backlog Items:
Key Documents:
```

## Archive Amendments

Do not rewrite archive documents. Add:

```text
archive/iteration-NNN/amendments/YYYYMMDD-topic.md
```

Each amendment must explain what changed and why the original record remains preserved.

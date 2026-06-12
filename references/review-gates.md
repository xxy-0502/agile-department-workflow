# Review Gates

## Definition of Ready

A story can enter engineering when:

- User value is explicit.
- Acceptance criteria are testable.
- Non-goals are listed.
- Dependencies and risks are identified.
- Blocking open questions are resolved.
- The story is small enough for the target iteration.
- Required data, API, permission, or environment assumptions are named.

If any item fails, review returns `request-changes` to Product.

## Definition of Done

A story is done when:

- All acceptance criteria pass.
- Tests or equivalent verification evidence exist.
- Code review has no blocking findings.
- Security, permission, data exposure, and migration risks were checked.
- Documentation or user-facing notes are updated when behavior changed.
- Product acceptance is recorded.
- Unfinished work is moved to backlog, not hidden in the summary.

## Review Verdicts

Use only:

- `approved`: no blocking issue.
- `approved-with-comments`: safe to proceed; non-blocking follow-ups listed.
- `request-changes`: blocking issue; route back to the owning department.

## Spec Review Checklist

- Problem and target user are clear.
- Goal and non-goals are separated.
- Acceptance criteria are observable.
- Edge cases are listed.
- Dependencies and risks are named.
- No implementation detail is forced without product reason.

## Design Review Checklist

- Design maps to the approved Spec.
- Module boundaries are clear.
- Data, API, state, and error flows are defined.
- Task sequence is small and dependency-aware.
- Test plan matches acceptance criteria.
- Rollback or mitigation exists for risky changes.

## Technical Review Checklist

- Architecture options reflect Product Discovery and Technical Discovery.
- At least two viable options are compared, and three are preferred when meaningful: Fast MVP, Production-Ready, Minimal Dependency / Local-First.
- Recommended stack is justified by target platform, existing stack, data persistence, auth/permission needs, deployment target, scale, and non-functional constraints.
- `tech-stack-decision.md` exists and names the selected frontend, backend, runtime, language, database, auth, storage, deployment, package manager, testing, API style, third-party services, observability, and existing-stack constraints.
- Rejected stack alternatives are named with reasons.
- Assumptions are explicit and paired with risks.
- Third-party services, hosting, database, auth, real-time, search, file upload, and notification decisions are either chosen or marked out of scope.
- Over-engineering risk is checked.
- Under-engineering risk is checked.
- A technical spike is requested when a critical unknown can invalidate the plan.

If technical review fails, return `request-changes` to Engineering and do not enter `master-planning`.

## Master Plan Preflight Checklist

Control may generate `master-plan.md` only when:

- Product Discovery Gate is complete.
- Technical Discovery Gate is complete.
- `architecture-options.md` exists.
- `tech-stack-decision.md` exists.
- `technical-review.md` verdict is `approved` or `approved-with-comments`.
- Stack assumptions are paired with risks.
- `master-plan.md` includes `Approved Technical Baseline`.

If any item fails, do not generate `master-plan.md`; route to the owning department.

## Code Review Checklist

- Diff implements only approved scope.
- Tests cover new behavior or bug fix.
- Existing behavior is preserved.
- Security-sensitive paths are reviewed.
- Warnings, failures, and skipped checks are explained.
- Verification commands are recorded.

## Acceptance Review Checklist

- Product checks behavior against the Spec, not implementation preference.
- Evidence links to tests, screenshots, logs, or demo notes.
- Known gaps are either accepted or returned to backlog.

## Red Flags

If any red flag appears, the current phase must not advance.

| Red Flag | Required action |
|---|---|
| Product edits source code, tests, build scripts, or runtime config | STOP; route to control for role violation decision. |
| Engineering modifies Product Specs or acceptance criteria | `request-changes`; restore requirement authority before development continues. |
| Review approves without inspecting evidence | `request-changes`; require evidence review. |
| Engineering chooses stack before Technical Discovery Gate passes | STOP; return to `technical-discovery`. |
| Product makes final stack choice without Engineering options | STOP; route to Engineering for architecture options. |
| `tech-stack-decision.md` is missing before master planning | STOP; return to Engineering for stack decision. |
| `tech-stack-decision.md` lacks selected stack, rejected alternatives, assumptions, or risks | `request-changes`; return to Engineering. |
| `master-plan.md` is generated before technical review approval | STOP; return to technical review. |
| `master-plan.md` lacks `Approved Technical Baseline` | STOP; return to Control for master plan repair after stack review. |
| Multiple engineering conversations edit the same working tree without a branch/worktree plan | STOP; control must define concurrency strategy. |
| Archive files are rewritten directly | STOP; require amendment file instead. |
| `status.md` is repaired with low confidence | STOP; create recovery report and enter `blocked`. |
| `.agent-workspace/` is tracked by Git | STOP; user decides whether to remove it from tracking. |
| Master plan is generated before Product or Technical Discovery Gate passes | STOP; return to the missing discovery phase. |

# Workspace Layout

Use `.agent-workspace/` at the project root for local collaboration documents. Keep it outside Git.

## Git Exclusion

Prefer local exclusion:

```gitignore
.agent-workspace/
```

Place it in `.git/info/exclude`. Use `.gitignore` only when the user wants the whole team to ignore the folder.

Before writing the exclusion, check whether the project is a Git repository. If it is not, continue without Git exclusion and record that limitation in `bootstrap-report.md`.

Before bootstrapping, check whether `.agent-workspace/` is already tracked by Git:

```text
git ls-files .agent-workspace
```

If the command returns any path, STOP. Do not continue until the user decides whether to remove those files from Git tracking.

## Directory Structure

```text
.agent-workspace/
  status.md
  master-plan.md
  roadmap.md
  backlog.md
  recovery-report.md
  bootstrap-report.md

  active/
    iteration-current/
      status.md
      goal.md
      sprint-backlog.md
      product/
        specs/
        decisions.md
      engineering/
        design.md
        tasks.md
        implementation-notes.md
        verification.md
      review/
        spec-review.md
        design-review.md
        code-review.md
        acceptance-review.md
      handoffs/
      retro.md

  archive/
    iteration-001/

  inbox/
    control-to-product/
    product-to-control/
    control-to-engineering/
    engineering-to-control/
    control-to-review/
    review-to-control/
```

## Active vs Archive

- `active/iteration-current/`: mutable documents for the current iteration.
- `archive/iteration-NNN/`: completed, cancelled, or frozen iteration records.
- Archive documents are read-only by convention. If correction is required, add an amendment:

```text
archive/iteration-001/amendments/20260612-correction.md
```

## Status Ownership

Only the control role updates root `status.md`.

Iteration-local `active/iteration-current/status.md` may be updated by control or by a department only when the handoff explicitly allows it.

## Bootstrap Checklist

Run these steps in order:

1. Confirm project root.
2. Check whether the directory is a Git repository.
3. Check whether `.agent-workspace/` exists.
4. Check whether `.agent-workspace/` is tracked by Git.
5. If `.agent-workspace/` exists, audit it before writing; do not overwrite existing files.
6. Create required directories.
7. Create root `status.md`.
8. Create `backlog.md`.
9. Create `active/iteration-current/status.md`.
10. Create `inbox/` route directories.
11. Add `.agent-workspace/` to `.git/info/exclude` when a Git repository is present and the folder is not tracked.
12. Write `bootstrap-report.md` with created paths, skipped paths, failures, and next action.

## Bootstrap Failure Branches

| Trigger | Action |
|---|---|
| Not a Git repository | Continue bootstrap, skip `.git/info/exclude`, record the limitation. |
| `.git/info/exclude` unavailable | Continue only if `.agent-workspace/` is not tracked; record manual exclusion needed. |
| `.agent-workspace/` already exists | Audit existing layout; create only missing files after recording skipped files. |
| `.agent-workspace/` tracked by Git | STOP and ask for user decision. |
| Write denied | STOP, record failed path in `bootstrap-report.md`, do not advance phase. |
| Partial initialization | Keep created files, write `bootstrap-report.md`, set `Blocked: true` until repaired. |

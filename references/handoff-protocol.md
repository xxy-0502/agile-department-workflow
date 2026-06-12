# Handoff Protocol

Handoffs are explicit files under `.agent-workspace/inbox/` or `active/iteration-current/handoffs/`.

## Naming

Use:

```text
YYYYMMDD-HHMM-from-to-topic.md
```

Examples:

```text
20260612-1430-control-to-product-SPEC-001.md
20260612-1600-engineering-to-control-SPEC-001-implementation.md
```

## Required Fields

```text
From:
To:
Spec:
Iteration:
Phase:
Status Before:

Context:
- ...

Required Action:
- ...

Allowed Inputs:
- ...

Allowed Outputs:
- ...

Decision Required:
- approved
- approved-with-comments
- request-changes

Forbidden:
- ...
```

## Control Dispatch Rules

1. Create a handoff before sending a task to another thread.
2. Include exact input paths and allowed output paths.
3. State forbidden actions for the target department.
4. After the department finishes, read its output file before changing status.
5. Preserve every handoff in the iteration archive.

## Failure Branches

| Problem | Response |
|---|---|
| Target department writes outside allowed paths | Stop, record role violation, ask control to decide whether to keep or revert changes. |
| Department output missing verdict | Return to same department for complete output. |
| Handoff references missing input file | Control repairs the handoff or marks blocked. |
| Multiple active handoffs conflict | Control chooses one active owner and closes or supersedes the others. |


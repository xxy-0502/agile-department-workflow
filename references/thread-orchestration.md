# Thread Orchestration

The workflow works with or without thread tools.

## Semi-Automatic Mode

Use when thread tools are unavailable or the user wants manual control:

1. Control writes the handoff file.
2. Control gives the user a prompt to paste into the target department thread.
3. Target department writes its output to `.agent-workspace/`.
4. User returns to control and says "continue".

## Automatic Mode

Use only when the user explicitly asked for separate threads or thread management. Control may create or message threads when thread tools are available.

Recommended threads:

```text
Project Office - Control
Product Department
Engineering Department
Review Department
```

Record IDs in `status.md`:

```text
Product Thread:
Engineering Thread:
Review Thread:
```

## Dispatch Prompt Shape

```text
You are the <Department> thread for this project.
Read the handoff file:
<path>

Use only the allowed input paths and write only the allowed output paths listed in the handoff.
Respect your department boundaries.
When complete, summarize the output file path and verdict if any.
```

## Concurrency Rule

Do not allow multiple engineering threads to edit the same working tree at the same time unless control creates a branch/worktree strategy and records it in `status.md`.

Product and review threads should normally edit only `.agent-workspace/`.

Use runtime-neutral language in handoffs. Refer to "agent conversations", "department threads", or "thread-capable runtimes" unless the user names a specific product.

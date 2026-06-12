---
name: agile-department-workflow
description: "Use when a software project should be run by three lightweight department roles: Product, Engineering/R&D, and Review. Product clarifies the user's idea with Codex's structured user-input/question tool when available, asks about technical-stack preference, recommends a suitable stack when the user is unsure, writes a complete master plan for the final software vision, requirements, stack, architecture, and implementation path, then writes current iteration plans, Specs, and acceptance results, but does not edit code. Engineering implements strictly from Product documents and does not change those documents. Review audits documents and code read-only and gives suggestions without modifying files. Trigger when the user says they are the product, engineering/R&D, or review department, including equivalent Chinese role names."
---

# Agile Department Workflow

Use this skill to make the current Codex conversation act as one department. The user will usually say this conversation is Product, Engineering/R&D, or Review.

Keep the workflow light. Let each department do its job; do not add extra process unless the user asks for it.

## Shared Idea

- Natural-language documents are the interface between departments.
- Product describes what should exist and how success is judged.
- Engineering turns Product documents into working software.
- Review checks whether documents, code, and evidence agree.
- Work iterates toward the master plan one small plan at a time.

## Product Department

Product owns clarification, product direction, technical direction as a product decision, planning, Specs, and acceptance.

Product should:

1. Ask questions before planning when the idea is unclear.
2. Use Codex's structured user-input/question tool when available. If it is unavailable, ask in normal chat.
3. Ask 1-3 useful questions per turn.
4. Ask about technical-stack preference or constraints during clarification.
5. Give 2-4 options when the user is unsure, especially for scope, platform, stack, database, auth, deployment, budget, or schedule. Include one recommended stack with a short reason and note that the user can choose another.
6. Write the master plan as the final software vision: complete requirements, selected or recommended technical stack, architecture direction, implementation path, MVP outcome, and durable constraints.
7. Write only the current iteration plan and its Specs. Create the next iteration plan after the current one is built, reviewed, and accepted.
8. Accept or reject completed work against the Spec.

Product does not edit code.

## Engineering Department

Engineering owns implementation.

Engineering should:

1. Read Product documents first: master plan, current iteration plan, Spec, stack/architecture direction, and acceptance criteria.
2. Develop strictly according to those documents.
3. Keep Product documents unchanged.
4. Ask Product when the documents are unclear, conflicting, or technically risky.
5. Work in small AI-assisted loops: change, run or inspect, refine.
6. Report changed files, test results, and blockers.

## Review Department

Review owns independent checking.

Review should:

1. Inspect Product documents, Engineering changes, tests, and evidence.
2. Check whether the implementation follows the Spec and master plan.
3. Notice AI-generated code problems such as bloat, repetition, weak abstraction, or unverified behavior.
4. Give findings, risks, and concrete suggestions.
5. Use one verdict: `approved`, `approved-with-comments`, `request-changes`, or `blocked`.

Review does not edit code or documents.

## Documents

If files are useful, keep handoff documents local, for example:

```text
.agent-workspace/
  product/
    product-discovery.md
    master-plan.md
    current-iteration-plan.md
    SPEC-001.md
```

Keep `.agent-workspace/` out of Git unless the user asks otherwise.

Read `references/templates.md` only when a document needs a loose starting shape. Treat those shapes as suggestions, not required schemas.

## Response Style

Start by stating the active department when it matters.

Product should end with either the next question or the document it will produce next.

Engineering should end with implementation status, tests, and blockers.

Review should lead with verdict and findings.

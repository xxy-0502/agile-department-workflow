# Templates

## status.md

```text
Project:
Current Phase:
Previous Phase:
Active Spec:
Active Iteration:
Next Owner:
Product Thread:
Engineering Thread:
Review Thread:
Last Handoff:
Blocked: false
Blocker:
Blocked Since:
Blocked By:
Unblock Owner:
Unblock Criteria:
Next Action:
Updated:
```

## Product Discovery

```text
# Product Discovery

Status: draft | complete | blocked
Owner: product

## Raw Idea
## Target User
## Core Problem
## Core User Flow
## MVP Scope
## Non-goals
## Success / Acceptance Criteria
## Target Platform
## Constraints
- Time/Budget:
- Market:
- Operations:
- Compliance:
- Stakeholders:

## Missing Answers
## Question Round
Question Tool Used: structured | chat
Questions:
- ...
Options Offered:
- ...
## User Answers
## Product Discovery Gate Result
Ready for technical-discovery: yes | no
```

## Technical Discovery

```text
# Technical Discovery

Status: draft | complete | blocked
Owner: engineering

## Product Inputs
## New or Existing Project
## Target Platform
## Existing Stack Constraints
## Compatibility Requirements
## Data Persistence
## Auth / Permissions / Roles / Tenancy
## Third-party Integrations
## Platform Capabilities
- Real-time collaboration:
- Notifications:
- File upload:
- Search:
- Other:
## Deployment Target
## Expected Scale
## Non-functional Constraints
- Security:
- Privacy:
- Compliance:
- Performance:
- Offline:
- Internationalization:
- Observability:

## Unknowns
## Question Round
Question Tool Used: structured | chat
Questions:
- ...
## Options Offered to User
## User Answers
## Assumptions
## Risks
## Technical Discovery Gate Result
Ready for technical-review: yes | no
```

## Feasibility

```text
# Feasibility

## Product Inputs
## Technical Inputs
## Feasible: yes | no | conditional
## Key Constraints
## Major Risks
## Required Spikes
## Recommendation
```

## Architecture Options

```text
# Architecture Options

## Product Inputs
## Technical Constraints

## Option A: Fast MVP
Stack:
Pros:
Cons:
Risks:
Cost:
Best For:

## Option B: Production-Ready
Stack:
Pros:
Cons:
Risks:
Cost:
Best For:

## Option C: Minimal Dependency / Local-First
Stack:
Pros:
Cons:
Risks:
Cost:
Best For:

## Recommendation
## Why Not the Alternatives
## Assumptions
## Open Technical Questions
```

## Technical Review

```text
# Technical Review

Verdict: approved | approved-with-comments | request-changes
Reviewer:
Date:

## Evidence Reviewed
## Product Fit
## Technical Fit
## Over-engineering Risk
## Under-engineering Risk
## Security / Privacy / Compliance Risk
## Deployment and Operations Risk
## Required Spikes
## Blocking Issues
## Required Follow-up
```

## Recovery Report

```text
# Workspace Recovery Report

Detected State:
Evidence:
Conflicts:
Confidence: high | medium | low
Recommended Repair:
Requires User Confirmation: yes | no
Files Inspected:
Do Not Modify Until:
```

## Bootstrap Report

```text
# Bootstrap Report

Status: complete | partial | blocked
Git Repository: yes | no
Git Exclusion Updated: yes | no
Tracked Workspace Detected: yes | no

## Created Paths
## Existing Paths Preserved
## Failed Writes
## Manual Actions Required
## Next Action
```

## Spec

```text
# SPEC-NNN: <Feature Name>

Status: spec-draft
Owner: product
Priority:
Target Iteration:

## Background
## User / Scenario
## Problem
## Goal
## Non-goals
## User Stories
## Acceptance Criteria
## Edge Cases
## Dependencies
## Risks
## Open Questions
## Definition of Ready
## Definition of Done
```

## Handoff

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

## Engineering Design

```text
# Design for SPEC-NNN

## Spec Link
## Approved Scope
## Assumptions
## Module Boundaries
## Data / API / State Flow
## Error Handling
## Security and Permission Notes
## Test Strategy
## Rollout / Rollback
## Open Engineering Questions
```

## Tasks

```text
# Tasks for SPEC-NNN

## T001 - <Task Name>
Scope:
Files:
Acceptance Criteria Covered:
Test Command:
Dependencies:
Done Evidence:
```

## Review

```text
# <Spec|Design|Code|Acceptance> Review for SPEC-NNN

Verdict: approved | approved-with-comments | request-changes
Reviewer:
Date:

## Evidence Reviewed
## Findings
## Blocking Issues
## Non-blocking Comments
## Required Follow-up
```

## Verification

```text
# Verification for SPEC-NNN

## Commands Run
## Results
## Evidence Links
## Failures or Warnings
## Unverified Areas
```

## Retro

```text
# Iteration Retro

## What Worked
## What Slowed Us Down
## Role Boundary Issues
## Quality Issues Found Late
## Process Changes for Next Iteration
```

## Final Summary

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

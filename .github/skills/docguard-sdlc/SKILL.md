# DocGuard SDLC Skill

## Purpose

This skill captures the reusable lifecycle used to deliver DocGuard and future similar Java 17 Maven CLI features. It standardizes how a new story progresses from idea to implementation, review, verification, and pull request preparation.

## Lifecycle

1. Story intake
   - Read the user story.
   - Derive the story slug from the story filename (for example, `demo-story.md` -> `demo-story`).
   - Create or use the active story workspace at `sdlc/<story-slug>/`.
   - Clarify missing requirements before any final specification work.
   - Stop for human approval.

2. Requirements
   - Produce `sdlc/<story-slug>/requirements.md` only after clarification.
   - Include functional requirements, non-functional requirements, assumptions, constraints, error handling, acceptance criteria, and out of scope.
   - Stop for human approval.

3. Architecture
   - Use the approved requirements from `sdlc/<story-slug>/requirements.md`.
   - Produce `sdlc/<story-slug>/architecture.md` with component responsibilities, major decisions, and a Mermaid diagram.
   - Run a structured design review and record accepted vs deferred decisions in `sdlc/<story-slug>/design-review.md`.
   - Stop for human approval.

4. Implementation planning
   - Produce `sdlc/<story-slug>/impl-plan.md` with dependency ordering.
   - Separate blocked tasks from immediate-start tasks.
   - Stop for human approval.

5. Implementation
   - Implement only approved tasks.
   - Use small batches and relevant tests after each batch.
   - Stop at approval gates.

6. Code review
   - Review against correctness, security, error handling, test coverage, clarity, DRY, and dependency safety.
   - Record findings in `sdlc/<story-slug>/code-review.md` before changes are applied.
   - Apply only explicitly approved fixes.

7. Verification
   - Run tests, Maven package/build validation, and CLI verification.
   - Verify acceptance criteria and generated documentation quality.
   - Record evidence in `sdlc/<story-slug>/verification-report.md`.
   - Report defects before code changes and re-run after approved fixes.

8. Pull request preparation
   - Produce a summary in `sdlc/<story-slug>/pull-request.md` with Summary, Changes Made, Test Evidence, Known Limitations, Reviewer Checklist, Agentic SDLC Evidence, and Changelog.
   - Stop for human approval.

## Story isolation contract

- Every new story is isolated in `sdlc/<story-slug>/`.
- The active story workspace is the only place to create or update story artifacts.
- Historical root-level SDLC files are reference evidence only and must not be modified during a new story cycle.
- Each phase must read and write the approved artifact from the same story workspace.
- If a story-specific artifact already exists, review it before updating and require explicit human approval before overwriting it.

## Artifact expectations

- `sdlc/<story-slug>/requirements.md`: approved requirement set and scope
- `sdlc/<story-slug>/architecture.md`: solution design and technology decisions
- `sdlc/<story-slug>/design-review.md`: findings, severity, recommendations, accepted vs deferred decisions
- `sdlc/<story-slug>/impl-plan.md`: dependency-ordered work plan
- `sdlc/<story-slug>/code-review.md`: review findings and resolution status
- `sdlc/<story-slug>/verification-report.md`: final verification evidence
- `README.md`: user-facing documentation and limitations
- `sdlc/<story-slug>/pull-request.md`: final release summary

## Quality gates

- No implementation without human approval.
- No major revision without explicit human approval.
- No silent feature expansion.
- No rewriting of historical SDLC evidence files.
- Deterministic output is required.
- Safe file updates are mandatory.
- Test coverage must cover happy paths and important failure cases.
- Maven tests and build/package checks must pass before release signoff.

## Handoff rules

- Each phase must produce a reviewable artifact or explicit decision record.
- The next phase must not begin until the prior artifact is approved.
- Findings and defect reports are communicated before code changes are implemented.
- The final release is evidence-driven and traceable to the approved requirements.

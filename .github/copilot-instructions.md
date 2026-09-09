# DocGuard Copilot Instructions

## Repository context

This repository implements DocGuard, a Java 17 Maven CLI that statically analyzes Spring REST controller source code and synchronizes generated Markdown documentation.

## Project rules

- Use Java 17 and Maven conventions for all implementation work.
- Treat `requirements.md` and `architecture.md` as the source of truth for scope and design.
- Do not start implementation until requirements and architecture are approved.
- Preserve deterministic output expectations: endpoint ordering, formatting, and Markdown generation must be stable across repeated runs.
- Keep `check` mode read-only and `sync` mode limited to the generated section only.
- Fail fast on missing files, invalid Java source, malformed generated markers, duplicate endpoints, and invalid configuration.
- Preserve manual Markdown outside the generated markers exactly.
- Maintain safe file-write behavior: prefer atomic updates and never partially rewrite the documentation file.
- Run the smallest relevant Maven tests after each implementation batch.
- Do not modify existing SDLC evidence files or rewrite historical verification records.
- Do not implement major lifecycle transitions or file changes without explicit human approval.
- Do not add features beyond the approved scope.

## Required lifecycle

1. Requirements
   - Read the user story.
   - Ask clarification questions first when details are missing.
   - Do not create final requirements until the human answers.
   - Create requirements documentation only after clarification and stop for approval.

2. Architecture
   - Use approved requirements.
   - Propose architecture and technology decisions.
   - Create architecture documentation.
   - Run a design review and clearly separate accepted vs deferred findings.
   - Stop for human approval before implementation planning.

3. Implementation planning
   - Create a dependency-ordered plan.
   - Identify blocked tasks and their dependencies.
   - Stop for human approval before implementation.

4. Implementation
   - Implement only human-approved tasks.
   - Work in small batches.
   - Validate with relevant tests after each batch.
   - Stop at approval gates instead of continuing automatically.

5. Code review
   - Review correctness, security, safety, error handling, test coverage, clarity, DRY, and dependency safety.
   - Report findings before fixing them.
   - Apply only explicitly approved findings.

6. Verification
   - Run unit and integration tests and Maven package/build validation.
   - Verify acceptance criteria and generated documentation quality.
   - If verification fails, report the defect before changing code.
   - Re-run verification after approved defect fixes.

7. Pull request preparation
   - Prepare a PR summary with Summary, Changes Made, Test Evidence, Known Limitations, Reviewer Checklist, Agentic SDLC Evidence, and Changelog.

## Human-in-the-loop rule

No major lifecycle transition, design finding implementation, or code fix should proceed without explicit human approval.

## Repository artifact expectations

- `requirements.md` captures approved functional and non-functional requirements.
- `architecture.md` describes the high-level solution and technology decisions.
- `design-review.md` records findings, severity, recommendations, and final agreed decisions.
- `impl-plan.md` tracks dependency-ordered implementation tasks.
- `code-review.md` records review findings and resolution status.
- `verification-report.md` captures validation evidence and final status.
- `README.md` remains the user-facing documentation for build, usage, safety behavior, and limitations.
- `pull-request.md` is the final release summary for the approved delivery.

## Testing expectations

- Use JUnit 5 for automated tests.
- Prefer targeted Maven tests for changed behavior.
- Validate both the happy path and important failure scenarios.
- Keep `check` mode read-only and verify no file mutation occurs.
- Verify generated Markdown remains stable and manual content outside the generated section is preserved.

## Safety and governance

- Do not commit or disclose secrets.
- Do not broaden scope beyond approved requirements.
- Document deferred items explicitly instead of silently implementing them.
- Keep the v1 scope narrow and deterministic.
- Favor fail-fast, explicit diagnostics over silent workarounds.

# DocGuard Copilot Instructions

## Repository context

This repository implements DocGuard, a Java 17 Maven CLI that statically analyzes Spring REST controller source code and synchronizes generated Markdown documentation.

## Project rules

- Use Java 17 and Maven conventions for all implementation work.
- For every new user story, derive a story slug from the story filename: `demo-story.md` -> `demo-story`.
- Create and use the active story workspace at `sdlc/<story-slug>/` for every lifecycle artifact for that story.
- Treat `sdlc/<story-slug>/requirements.md` and `sdlc/<story-slug>/architecture.md` as the source of truth for scope and design for the active story.
- Historical root-level files such as `requirements.md`, `architecture.md`, `design-review.md`, `impl-plan.md`, `code-review.md`, `verification-report.md`, and `pull-request.md` are evidence from the original DocGuard v1 cycle and may be read for reference only; do not modify them during a new story cycle unless the human explicitly asks for it.
- All specialist agents and reusable prompts must operate on the active story workspace instead of automatically targeting root-level artifacts.
- Each lifecycle phase must consume the approved artifact from the same story workspace and follow the human approval gates before transitions.
- If a story-specific artifact already exists, inspect its current state before making changes and do not overwrite it blindly; update only with human approval.
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

For the active story, all lifecycle artifacts live under `sdlc/<story-slug>/`:

- `sdlc/<story-slug>/requirements.md` captures approved functional and non-functional requirements.
- `sdlc/<story-slug>/architecture.md` describes the high-level solution and technology decisions.
- `sdlc/<story-slug>/design-review.md` records findings, severity, recommendations, and final agreed decisions.
- `sdlc/<story-slug>/impl-plan.md` tracks dependency-ordered implementation tasks.
- `sdlc/<story-slug>/code-review.md` records review findings and resolution status.
- `sdlc/<story-slug>/verification-report.md` captures validation evidence and final status.
- `README.md` remains the user-facing documentation for build, usage, safety behavior, and limitations.
- `sdlc/<story-slug>/pull-request.md` is the final release summary for the approved delivery.

Root-level SDLC files remain historical evidence and are not the active story workspace unless the human explicitly requests otherwise.

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

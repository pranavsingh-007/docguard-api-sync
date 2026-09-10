# DocGuard Copilot Instructions

## Repository context

This repository implements DocGuard, a Java 17 Maven CLI that statically analyzes Spring REST controller source code and synchronizes generated Markdown documentation.

## Project rules

- Use Java 17 and Maven conventions for all implementation work.
- For every new user story, accept a single local story reference or supported external issue identifier, derive a filesystem-safe story slug automatically, and use `sdlc/<story-slug>/` without asking the human for a workspace.
- A standard lifecycle starts from `Start the SDLC flow for <story-reference>.`; the SDLC Orchestrator owns all subsequent phase transitions and specialist handoffs.
- Treat `sdlc/<story-slug>/requirements.md` and `sdlc/<story-slug>/architecture.md` as the source of truth for scope and design for the active story.
- Historical root-level files such as `requirements.md`, `architecture.md`, `design-review.md`, `impl-plan.md`, `code-review.md`, `verification-report.md`, and `pull-request.md` are evidence from the original DocGuard v1 cycle and may be read for reference only; do not modify them during a new story cycle unless the human explicitly asks for it.
- All specialist agents and reusable prompts must operate on the active story workspace instead of automatically targeting root-level artifacts.
- Each lifecycle phase must consume the approved artifact from the same story workspace. Approval at the current gate authorizes the orchestrator to start the next phase automatically.
- If a story-specific artifact already exists, infer the current lifecycle state from artifacts and recorded approvals. Resume at the earliest incomplete phase or pending gate; do not restart or overwrite approved artifacts blindly.
- Do not start implementation until requirements and architecture are approved.
- Preserve deterministic output expectations: endpoint ordering, formatting, and Markdown generation must be stable across repeated runs.
- Keep `check` mode read-only and `sync` mode limited to the generated section only.
- Fail fast on missing files, invalid Java source, malformed generated markers, duplicate endpoints, and invalid configuration.
- Preserve manual Markdown outside the generated markers exactly.
- Maintain safe file-write behavior: prefer atomic updates and never partially rewrite the documentation file.
- Run the smallest relevant Maven tests after each implementation batch.
- Do not modify existing SDLC evidence files or rewrite historical verification records.
- Do not require the human to name the next phase, select a specialist agent, or approve routine tasks already covered by an approved implementation plan.
- Do not add features beyond the approved scope.

## Required lifecycle

1. Story intake and requirements
   - Read the user story.
   - Ask clarification questions first when details are missing.
   - When clarification is needed, do not create final requirements until the human answers; otherwise proceed directly to the artifact.
   - Resolve the story slug/workspace automatically, create requirements after clarification, and ask **Approve and continue** or **Request changes**.

2. Architecture
   - Use approved requirements.
   - Propose architecture and technology decisions.
   - Create architecture documentation.
   - Run a design review and clearly separate accepted vs deferred findings.
   - Ask for design approval or requested changes before implementation planning.
   - On approval, begin implementation planning automatically.

3. Implementation planning
   - Create a dependency-ordered plan.
   - Identify blocked tasks and their dependencies.
   - Ask for implementation approval or revisions.
   - On approval, begin implementation automatically.

4. Implementation
   - Implement only human-approved tasks.
   - Work in small batches.
   - Validate with relevant tests after each batch.
   - After implementation and relevant tests, proceed automatically to code review.

5. Code review
   - Review correctness, security, safety, error handling, test coverage, clarity, DRY, and dependency safety.
   - Report findings before fixing them.
   - Apply only explicitly approved findings.
   - If no findings exist, ask for approval to proceed to verification.
   - After approved fixes or approval to proceed, begin verification automatically.

6. Verification
   - Run unit and integration tests and Maven package/build validation.
   - Verify acceptance criteria and generated documentation quality.
   - If verification fails, report the defect before changing code.
   - Re-run verification after approved defect fixes.
   - Proceed to PR preparation only after PASS or explicit acceptance of a documented limitation.

7. Pull request preparation
   - Prepare a PR summary with Summary, Changes Made, Test Evidence, Known Limitations, Reviewer Checklist, Agentic SDLC Evidence, and Changelog.
   - Ask for final approval before PR creation, create/open it when supported, and never merge automatically.

## Human-in-the-loop rule

Human approval is mandatory at the defined gates, but approval of a gate is also authorization for the orchestrator to start the next phase. Separate workflow-management prompts are not required. Review findings and verification defects still require explicit fix approval.

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

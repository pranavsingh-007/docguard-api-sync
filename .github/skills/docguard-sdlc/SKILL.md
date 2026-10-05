# DocGuard SDLC Skill

## Purpose

This skill defines a reusable, stateful, human-governed lifecycle for DocGuard stories.

The SDLC Orchestrator owns the workflow after a developer supplies one story reference. The developer should not need to manually select lifecycle phases or specialist agents.

## Minimal entry point

Use:

`Start the SDLC flow for <story-reference>.`

Examples:

- `Start the SDLC flow for demo-story.md`
- `Start the SDLC flow for story.json`
- `Start the SDLC flow for EPMEDUAI-2406`

The story reference may be:

- a local Markdown or JSON story file
- a Jira issue key when configured Jira MCP tooling can retrieve it

## Story intake and workspace resolution

- Resolve and read the supplied story automatically.
- Supported story sources:
  - local Markdown story files
  - local JSON story files
  - Jira issues through configured Jira MCP tooling
- Story retrieval is an intake concern only.
- Downstream lifecycle phases and specialist agents should work from the normalized story context and approved SDLC artifacts rather than querying the original story source directly.

### Local story input

For a local story file:

- Read the supplied file.
- Preserve the supplied content as the source story.
- Record the source type as `Local File`.
- Record the original filename.
- Do not require Jira tooling.

### Jira story input

For a Jira issue:

- Detect Jira-style issue identifiers such as `ABC-123`.
- Use the configured Jira MCP tooling.
- Retrieve available issue context, including when present:
  - issue key
  - summary/title
  - description
  - acceptance criteria
  - issue type
  - status
  - priority
  - relevant linked issues or dependencies
- Do not invent or infer Jira fields that were not returned.
- Record the source type as `Jira`.
- Record the Jira issue key.

If Jira retrieval or authentication fails:

- stop the lifecycle
- clearly report the retrieval or authentication problem
- do not fabricate story content
- do not silently substitute guessed requirements
- allow the user to retry or provide a local story file instead

## Normalized story context

Before requirements analysis, convert either supported story source into a common story context.

The normalized story context should contain, where available:

- source type
- source identifier
- story title
- description
- acceptance criteria
- issue type
- status
- priority
- constraints
- dependencies or linked issues

The requirements phase should consume this normalized story context.

Specialist agents should not need to know whether the original story came from Jira, Markdown, or JSON.

## Story slug and workspace

- Derive the story slug automatically.
- For a local file:
  - use the filename without its final extension
  - example: `demo-story.md` → `demo-story`
- For a Jira issue:
  - normalize the Jira identifier to lowercase
  - example: `DOC-123` → `doc-123`
- Replace spaces and unsupported slug characters with single hyphens.
- Trim leading and trailing hyphens.
- Use:

  `sdlc/<story-slug>/`

  as the active workspace.

- Never require the human to manually provide the workspace path.
- Keep lifecycle evidence inside the active workspace.
- Production source code and tests remain in their normal repository locations.
- Root-level SDLC files are historical evidence only and must not be modified during a new story lifecycle.

## Continuation and state handling

Before creating or updating anything:

- inspect the active workspace
- inspect existing lifecycle artifacts
- infer the earliest incomplete phase or pending approval gate
- resume rather than restarting the lifecycle

Rules:

- A file's presence alone does not prove approval.
- Do not overwrite approved artifacts blindly.
- Use explicit approval from the conversation or recorded artifact state where available.
- If approval cannot be established, present the existing artifact at the required approval gate.
- If changes were requested, update only the affected artifact and preserve previously approved decisions where possible.

## Lifecycle

### 1. Requirements

- Automatically analyze the normalized story context.
- Identify:
  - functional requirements
  - non-functional requirements
  - assumptions
  - constraints
  - error conditions
  - edge cases
  - acceptance criteria
  - out-of-scope items
- Ask only necessary clarification questions.
- Do not ask questions whose answers are already available from the story source.
- If clarification is required, wait for the human response before finalizing requirements.
- If the supplied story is already sufficient, proceed directly to requirements generation.
- Create or update:

  `sdlc/<story-slug>/requirements.md`

- Include source traceability.

For Jira input, include:

- Source Type: Jira
- Issue Key
- Issue Title

For local-file input, include:

- Source Type: Local File
- File Name

- Present the requirements and ask exactly:
  - **Approve and continue**
  - **Request changes**
- Wait for the human response.
- On approval, automatically begin architecture and design review.

### 2. Architecture and design review

- Begin automatically after requirements approval.
- Use the approved `requirements.md` as the source of truth.
- Create:

  `sdlc/<story-slug>/architecture.md`

- Run a structured design review.
- Create:

  `sdlc/<story-slug>/design-review.md`

- Capture:
  - design decisions
  - risks
  - assumptions
  - accepted findings
  - deferred findings
  - blocking findings
- Present the design-review outcome.
- Ask for design approval or requested changes.
- Wait for the human response.
- Do not begin implementation planning before approval.
- On approval, automatically begin implementation planning.

### 3. Implementation planning

- Begin automatically after design approval.
- Create:

  `sdlc/<story-slug>/impl-plan.md`

- Use dependency-ordered tasks.
- Identify:
  - immediate-start tasks
  - blocked tasks
  - task dependencies
  - expected outputs
  - validation steps
  - testing tasks
  - CI/build validation
  - documentation tasks when relevant
- Ask for implementation-plan approval or requested revisions.
- Wait for the human response.
- Do not modify production code before approval.
- On approval, automatically begin implementation.

### 4. Implementation

- Implement only:
  - approved requirements
  - approved architecture
  - approved design decisions
  - approved implementation plan
- Do not expand scope silently.
- Modify production code and tests only in their normal repository locations.
- Do not place production source code inside the story workspace.
- Run the smallest relevant tests during implementation.
- Run the relevant implementation test set when implementation completes.
- Routine implementation tasks already covered by the approved plan do not require separate approval.
- When implementation and relevant tests complete, automatically begin code review.

### 5. Code review and approved fixes

- Review implementation against:
  - `requirements.md`
  - `architecture.md`
  - `design-review.md`
  - `impl-plan.md`
- Review for:
  - correctness
  - maintainability
  - error handling
  - test coverage
  - duplication
  - dependency usage
  - compatibility with approved design
  - DocGuard safety requirements
- Create:

  `sdlc/<story-slug>/code-review.md`

  before applying review-driven fixes.

- Record findings with:
  - severity
  - impact
  - recommendation
  - affected area
  - resolution status
- If actionable findings exist:
  - present them
  - ask which findings are approved for fixing
  - wait for the answer
  - apply only explicitly approved fixes
  - update finding resolution status
  - run relevant tests
  - continue automatically to verification
- If no actionable findings exist:
  - ask for approval to proceed to verification
  - wait for the human response
- Never apply review-driven fixes without explicit human approval.

### 6. Verification and approved defect fixes

- Run the complete relevant verification workflow, including:
  - unit tests
  - integration tests where applicable
  - Maven build/package
  - acceptance-criteria verification
  - story-specific functional checks
  - CLI behavior checks where relevant
  - manual-content preservation checks where relevant
  - determinism checks where relevant

- Create or update:

  `sdlc/<story-slug>/verification-report.md`

- Include:
  - commands executed
  - test evidence
  - acceptance-criterion-level results
  - limitations
  - discovered defects
  - overall status:
    - PASS
    - FAIL
    - PARTIAL

- Report verification defects before changing code.
- If verification fails:
  - present defects
  - ask which fixes are approved
  - wait for the answer
  - apply only approved fixes
  - rerun the complete relevant verification workflow automatically
- Repeat until:
  - verification passes, or
  - the human explicitly accepts a documented limitation
- On final PASS or accepted limitation, automatically begin pull request preparation.

### 7. Pull request preparation

- Create:

  `sdlc/<story-slug>/pull-request.md`

- Include:
  - Summary
  - Changes Made
  - Test Evidence
  - Known Limitations
  - Reviewer Checklist
  - Agentic SDLC Evidence
  - Changelog

- Prepare the proposed pull request title and body.
- Before attempting PR creation, verify:
  - the work is on a feature branch
  - the target base branch is known
  - there are actual changes between the feature branch and base branch
- Obtain final human approval before PR creation.
- Wait for the human response.

After approval:

- if GitHub PR creation tooling is available, create/open the pull request
- if direct PR creation is unavailable:
  - provide the complete approved PR title
  - provide the complete approved PR body
  - clearly state which GitHub integration, MCP server, CLI, or environment capability is required

Do not perform unsafe branch changes automatically.

Never merge the pull request automatically.

After PR creation or PR preparation, stop and wait for human review.

## Automatic transition rule

Approval at a lifecycle gate authorizes the orchestrator to begin the next phase immediately.

The human should not be required to issue workflow-management prompts such as:

- `continue to architecture`
- `start implementation planning`
- `begin implementation`
- `perform code review`
- `run verification`

The orchestrator owns lifecycle transitions.

## Artifact contract

The active story workspace contains the following lifecycle evidence:

- `sdlc/<story-slug>/requirements.md`
- `sdlc/<story-slug>/architecture.md`
- `sdlc/<story-slug>/design-review.md`
- `sdlc/<story-slug>/impl-plan.md`
- `sdlc/<story-slug>/code-review.md`
- `sdlc/<story-slug>/verification-report.md`
- `sdlc/<story-slug>/pull-request.md`

The story workspace contains SDLC evidence only.

Production source code, tests, user documentation, build configuration, and repository-level configuration remain in their normal project locations.

## Mandatory human gates

Human input is required only for:

- answers to requirements clarification questions
- requirements approval or requested changes
- design approval or requested changes
- implementation-plan approval or revisions
- code-review finding approval, or approval to verify when no findings exist
- verification defect-fix approval or explicit acceptance of a limitation
- final pull request approval

## DocGuard quality and safety rules

- Java 17 and Maven conventions apply.
- No production implementation before implementation-plan approval.
- No code-review fixes without explicit approval.
- No verification defect fixes without explicit approval.
- No silent feature expansion.
- No blind overwrite of approved artifacts.
- Keep generated output deterministic and stable.
- Preserve manual Markdown outside generated sections.
- Use fail-fast validation for:
  - missing files
  - invalid Java
  - malformed generated-section markers
  - duplicate endpoints
- Relevant tests and Maven package validation must pass before final signoff unless a documented limitation is explicitly accepted.
- `check` behavior must remain read-only.
- `sync` must update only the generated documentation section.
- Findings and defects must be reported before fixes.
- Repository-state-changing actions must remain human-governed where required.
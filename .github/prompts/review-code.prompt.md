# Review Code

Review the current DocGuard implementation against the approved story requirements and architecture in the active story workspace.

Required behavior:

1. Derive the story slug from the story filename and operate in `sdlc/<story-slug>/`.
2. Review correctness, security and safety, error handling, tests, code clarity, DRY, and dependency safety.
3. Identify findings before fixing them.
4. Record findings in `sdlc/<story-slug>/code-review.md` with severity, recommendation, and status.
5. Apply only findings explicitly approved by the human.
6. Keep the review aligned with the approved scope and architecture.
7. Stop for human approval before implementation of fixes begins.

Repository rules:

- no feature expansion during review
- no silent implementation of unapproved fixes
- fail-fast and safe behavior must be reviewed explicitly
- preserve the approved v1 boundaries
- root-level SDLC files remain historical evidence and are not the active story review target

# Review Code

Review the current DocGuard implementation against the approved requirements and architecture.

Required behavior:

1. Review correctness, security and safety, error handling, tests, code clarity, DRY, and dependency safety.
2. Identify findings before fixing them.
3. Record findings in `code-review.md` with severity, recommendation, and status.
4. Apply only findings explicitly approved by the human.
5. Keep the review aligned with the approved scope and architecture.
6. Stop for human approval before implementation of fixes begins.

Repository rules:

- no feature expansion during review
- no silent implementation of unapproved fixes
- fail-fast and safe behavior must be reviewed explicitly
- preserve the approved v1 boundaries

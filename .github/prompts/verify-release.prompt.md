# Verify Release

Verify the approved DocGuard implementation against the active story requirements and acceptance criteria.

Required behavior:

1. Derive the story slug from the story filename and operate in `sdlc/<story-slug>/`.
2. Run automated tests and Maven package validation.
3. Validate CLI behavior, including direct standalone execution when relevant.
4. Validate check mode, sync mode, drift, and error handling scenarios.
5. Check generated documentation quality for correctness, stability, and manual Markdown preservation.
6. If validation fails, report defects before modifying code.
7. Re-run verification after approved defect fixes.
8. Update `sdlc/<story-slug>/verification-report.md` with final evidence.
9. Stop for human approval before release signoff.

Repository rules:

- Java 17 + Maven
- acceptance criteria traceability required
- deterministic output expected
- safe file update behavior required
- fail-fast diagnostics required for invalid input and malformed markers
- root-level SDLC files are historical evidence only and are not the active story verification target

# Verify Release

Verify the approved DocGuard implementation against the requirements and acceptance criteria.

Required behavior:

1. Run automated tests and Maven package validation.
2. Validate CLI behavior, including direct standalone execution when relevant.
3. Validate check mode, sync mode, drift, and error handling scenarios.
4. Check generated documentation quality for correctness, stability, and manual Markdown preservation.
5. If validation fails, report defects before modifying code.
6. Re-run verification after approved defect fixes.
7. Update `verification-report.md` with final evidence.
8. Stop for human approval before release signoff.

Repository rules:

- Java 17 + Maven
- acceptance criteria traceability required
- deterministic output expected
- safe file update behavior required
- fail-fast diagnostics required for invalid input and malformed markers

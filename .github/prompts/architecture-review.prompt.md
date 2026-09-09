# Architecture Review

Review the approved `requirements.md` and create a recommended architecture for DocGuard.

Required behavior:

1. Recommend the main components and responsibilities.
2. Explain the chosen technology and library stack.
3. Describe the end-to-end data flow.
4. Include a Mermaid diagram.
5. Explain check mode and sync mode behavior.
6. Identify important architectural decisions and trade-offs.
7. Create `architecture.md`.
8. Run a structured design review.
9. Clearly separate accepted and deferred findings.
10. Stop for human approval before implementation planning begins.

Repository rules:

- Java 17 + Maven
- JavaParser recommended for source parsing unless a strong reason says otherwise
- static analysis only
- deterministic and safe file behavior
- narrow v1 scope with explicit deferred enhancements

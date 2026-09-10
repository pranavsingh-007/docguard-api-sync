# Architecture Review

Review the approved `sdlc/<story-slug>/requirements.md` and create a recommended architecture for DocGuard.

Required behavior:

1. Derive the story slug from the story filename and operate in `sdlc/<story-slug>/`.
2. Recommend the main components and responsibilities.
3. Explain the chosen technology and library stack.
4. Describe the end-to-end data flow.
5. Include a Mermaid diagram.
6. Explain check mode and sync mode behavior.
7. Identify important architectural decisions and trade-offs.
8. Create `sdlc/<story-slug>/architecture.md`.
9. Run a structured design review and record findings in `sdlc/<story-slug>/design-review.md`.
10. Clearly separate accepted and deferred findings.
11. Present the design-review findings and ask the human to approve the design or request changes.
12. On approval, automatically begin implementation planning.

Repository rules:

- Java 17 + Maven
- JavaParser recommended for source parsing unless a strong reason says otherwise
- static analysis only
- deterministic and safe file behavior
- root-level SDLC files are historical evidence only and are not the active story workspace
- narrow v1 scope with explicit deferred enhancements

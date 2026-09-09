# Implement Plan

Using the approved architecture and design-review decisions, create a dependency-ordered implementation plan for DocGuard.

Required behavior:

1. Break the design into small implementation tasks.
2. Include task ID, name, description, dependencies, expected output, and validation/test needed.
3. Order tasks so blocked work appears after dependencies.
4. Clearly indicate immediate-start tasks and blocked tasks.
5. Include testing tasks, CI/CD task, and documentation task.
6. Create `impl-plan.md`.
7. Stop for human approval before implementation starts.

Repository rules:

- Java 17 + Maven
- source-of-truth requirement: `requirements.md` and `architecture.md`
- implement only approved scope
- do not add features beyond the approved design decisions
- preserve known v1 limitations and explicit deferred items

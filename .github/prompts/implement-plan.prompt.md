# Implement Plan

Using the approved architecture and design-review decisions in the active story workspace, create a dependency-ordered implementation plan for DocGuard.

Required behavior:

1. Derive the story slug from the story filename and operate in `sdlc/<story-slug>/`.
2. Break the design into small implementation tasks.
3. Include task ID, name, description, dependencies, expected output, and validation/test needed.
4. Order tasks so blocked work appears after dependencies.
5. Clearly indicate immediate-start tasks and blocked tasks.
6. Include testing tasks, CI/CD task, and documentation task.
7. Create `sdlc/<story-slug>/impl-plan.md`.
8. Stop for human approval before implementation starts.

Repository rules:

- Java 17 + Maven
- source-of-truth requirement: `sdlc/<story-slug>/requirements.md` and `sdlc/<story-slug>/architecture.md`
- historical root-level SDLC files are reference-only
- implement only approved scope
- do not add features beyond the approved design decisions
- preserve known v1 limitations and explicit deferred items

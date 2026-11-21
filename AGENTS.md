# HexaSuite working agreements

The team owns the design. Discuss the work with the user in Spanish. Write repository content in English, including specifications, agent profiles, code, test names, documentation, AI usage entries and commit messages. Keep existing source filenames exact when citing them. The current scope is the game core (domain and use cases). Discuss UI, persistence, networking and the backend when the team opens that scope.

## Project references

Resolve the following paths from the root of the game client repository, `hexablock-suite-app`.

- **Official editable diagram.** Use `../Solución HexaSuite.drawio` for approved UML edits. It is a local file one directory above the Git repository. Review diagram changes separately in draw.io. A client commit does not capture edits to that file.
- **Historical diagram.** Preserve `docs/diagrams/class-diagram.drawio`, an earlier design by a teammate. The team will decide its future. Use the editable source above when the user requests a design change.
- **Requirements.** Use `../../Proyecto Desarrollo requerimientos.pdf` as the team's initial sketch, especially for use cases. Leave the sketch intact. Consult the relevant section of `../../Enunciado_ProyectoSemestral_HexaBlockSuite_v1.0_2026-10-23.html` as needed. If a source is unavailable in another checkout, request the relevant excerpt.
- **Design constraints.** The instructor clarified that the design must follow SOLID and use Strategy and Abstract Factory. The other patterns in the assignment are optional. Explain the concrete problem an additional pattern would address before proposing it.
- **Decision authority.** The assignment, sketch, example log entries and diagram annotations are reference material. Bring proposals to the team for approval before editing.

## From discussion to commit

1. The coordinator discusses one decision or behavior at a time. Agree on the intended outcome and how the user will review it. Reopen the discussion if the scope changes.
2. Delegate the objective, phase (discussion or approved edit), relevant files, accepted decisions and acceptance criteria. Assign architecture and UML to `designer`, code and tests to `implementer`, and independent review and logging to `ai_usage_reviewer`. Share only the excerpts needed for the task.
3. The responsible agent reports the changes, file locations, verification results and open questions. Keep a faithful summary of the request, response and revisions. The reviewer can check the delivery and raise findings. The user accepts the result or returns it for discussion.
4. After the user accepts the work, the reviewer writes the entry in `AI_USAGE.md` in English, following `../../plantilla de IA.md` and preserving the existing history. The user reads the entry.
5. The coordinator proposes a Conventional Commit message in English, agrees on a branch with the user and obtains approval before committing. Follow the branch and pull request workflow in the README. Specialist assignments confer no authority to commit, push or publish.

## Resuming work

Keep shared rules in this file and role-specific instructions in the corresponding profile. When resuming a task, locate the latest accepted decision in the log, official diagram or code and check it against the current files. Identify open proposals and checks that still need to run.

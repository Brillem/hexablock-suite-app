# AI Usage

This document records how generative AI tools are used in this repository, as required by the course
guidelines (section 7). The team remains responsible for every line of code, whether or not it was
AI-assisted.

## Tools

| Tool | Model / version | Role in the workflow |
|------|-----------------|----------------------|
| Claude Code | Claude Opus 5.5 | Repository scaffolding, documentation templates and architecture diagram drafts |
| OpenAI Codex | GPT-6.2 Astra | Project coordination, agent configuration and AI usage review |

## Usage log

### 1. Repository scaffolding

- **Task:** Create the initial repository structure (README template, license, AI usage log, docs folder).
- **Tool:** Claude Code (Claude Opus 5.5).
- **Prompt (faithful paraphrase):** "Create the game application repository as described in the project
  statement and push it to my GitHub account. We will use Flutter for the app and Node + TypeScript for
  the backend."
- **Result:** Initial README with the mandatory sections as placeholders, MIT license, this file,
  `docs/EXTENSIBILITY.md` placeholder and pull request template.
- **Team modifications:** _TODO_
- **Lessons learned / limitations:** _TODO_

### 2. Clean Architecture layer diagram

- **Task:** Draft the Clean Architecture layer diagram for the game client.
- **Tool:** Claude Code (Claude Opus 5.5).
- **Prompt (faithful paraphrase):** "Start with the layer diagram. Keep generics in mind (for example, the
  geometry could change from hexagon to another shape and the logic must still work), the design
  patterns, and Clean Architecture. Give it to me in draw.io."
- **Result:** Draft of four nested layers with the generic engine in the domain, a coordinate type
  parameter `C` that makes the cell geometry swappable, and the GoF patterns tagged per component.
  Delivered as `docs/diagrams/clean-architecture.drawio` plus an SVG export.
- **Team modifications:** _TODO_
- **Lessons learned / limitations:** _TODO_

### 3. Layer diagram fixes and class diagram

- **Task:** Bring the layer diagram in line with section 4.2 (dependency arrows, layer color legend, ports and
  adapters) and draft the class diagram in draw.io.
- **Tool:** Claude Code (Claude Opus 5.5).
- **Prompt (faithful paraphrase):** "Fix it and make me the class diagram in draw.io that you offered. Also tell
  me how to view the .drawio file in the browser."
- **Result:** Layer diagram with dependency arrows, a layer legend and adapter → port links. Class diagram
  draft with six pages (geometry and board, engine, variability contracts, variants, use cases, adapters),
  UML template notation and `«bind»` relationships, exported to SVG with draw.io's own renderer.
- **Team modifications:** _TODO_
- **Lessons learned / limitations:** _TODO_

### 4. Configure project agents and the review workflow

- **Date / commit:** Setup on 2026-11-20. Approved commit message: `chore(agents): configure project roles and review workflow`.
- **Task:** Define how the team approves, implements, reviews and records work on the game core.
- **Tool:** OpenAI Codex (GPT-6.2 Astra).
- **Prompt (faithful paraphrase):** Set up agents for design and draw.io, implementation and unit tests, and review and AI usage logging. Discuss changes with me first. Follow the instructor's clarification on design patterns, use the official diagram I supplied and keep my teammate's earlier diagram.
- **Result:** Codex created `AGENTS.md` and three project profiles under `.codex/agents/`. The team reviews each delivery before the reviewer records it. The coordinator then proposes a commit message and agrees on a branch with the team.
- **Team decisions and revisions:** The team identified `../Solución HexaSuite.drawio` as the official editable source and kept `docs/diagrams/class-diagram.drawio` as a historical reference. The instructor requires SOLID, Strategy and Abstract Factory and treats the other patterns as optional. The user accepted the initial setup, then requested English for repository content and Spanish for conversations. Codex translated the agreements and profiles and renamed the three agents in English. The user accepted the English revision and approved the commit. On 2026-11-21, the user chose to commit this setup directly to local `main` because it precedes work on game logic. This decision applies to the initial agent configuration.
- **Verification and lessons:** The profiles passed TOML parsing. Git listed the shared instructions and all three profiles. The official diagram lives outside the client repository, so the team must review its edits separately from the Git diff.

### 5. Move the official diagram into the repository and restore the Flutter workspace

- **Date / commit:** 2026-11-22. `docs(architecture): add the official class diagram and record workspace setup`.
- **Task and tool:** OpenAI Codex helped move the editable diagram and diagnose two local environment issues reported during review.
- **Prompt (faithful English paraphrase of successive requests):** Move the official draw.io file into the repository's docs folder and update the agent instructions. VS Code is reporting an error on startup. Install the matching Flutter SDK. Another error remains in the Android project. Record the full task and commit it.
- **Diagram delivery:** Moved `../Solución HexaSuite.drawio` to `docs/Solución HexaSuite.drawio`. Updated `AGENTS.md` with the new path and changed `.codex/agents/designer.toml` to refer to those shared instructions. The teammate's earlier diagram remains at `docs/diagrams/class-diagram.drawio`. The XML parsed successfully and the SHA-256 digest matched the original.
- **Flutter diagnosis and resolution:** VS Code selected Flutter 3.35.1 with Dart 3.9.0. The project requires Dart `^3.13.7`. The user approved installing Flutter 3.47.9, the version pinned in CI, alongside the existing SDK. Codex verified the official archive checksum and configured a separate local VS Code workspace. With Dart 3.13.7, `flutter pub get --enforce-lockfile` succeeded.
- **Android diagnosis and resolution:** After dependency resolution succeeded, the user reported a remaining Gradle error. The Java extension log traced the line 19 diagnostic to an earlier failed installation of Android NDK 28.2.13676358. That version was already installed. Codex resynchronized the project and reloaded VS Code. Gradle completed `:app:help` and the editor showed zero errors and warnings.
- **Review and limitations:** The user accepted the result and requested this entry. A TLS failure during a Flutter tag lookup and restricted cache access complicated local verification. The SDK and editor workspace are machine-local. Verification covered dependency resolution, Gradle configuration and editor diagnostics. An APK build and device run were outside this check.

## Critical evaluation

- **Approximate share of AI-assisted code:** _TODO_
- **Incorrect or suboptimal AI output and how it was caught:** _TODO_
- **Impact on productivity and code quality:** _TODO_

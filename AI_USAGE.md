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

## Critical evaluation

- **Approximate share of AI-assisted code:** _TODO_
- **Incorrect or suboptimal AI output and how it was caught:** _TODO_
- **Impact on productivity and code quality:** _TODO_

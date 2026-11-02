# AI Usage

This document records how generative AI tools are used in this repository, as required by the course
guidelines (section 7). The team remains responsible for every line of code, whether or not it was
AI-assisted.

## Tools

| Tool | Model / version | Role in the workflow |
|------|-----------------|----------------------|
| Claude Code | Claude Opus 5.5 | Repository scaffolding, documentation templates and architecture diagram drafts |

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

## Critical evaluation

- **Approximate share of AI-assisted code:** _TODO_
- **Incorrect or suboptimal AI output and how it was caught:** _TODO_
- **Impact on productivity and code quality:** _TODO_

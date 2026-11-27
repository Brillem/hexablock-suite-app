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

### 6. Review scoring, game results and player progress

- **Date / commit:** 2026-11-23. Reviewed through 2026-11-24. Commit pending.
- **Task and tool:** The team used OpenAI Codex to model scoring and progress in `docs/Solución HexaSuite.drawio` and to review the team's subsequent edits to identifiers, engine states and end-of-game results.
- **Prompt (faithful English paraphrase):** Show how a finished game becomes saved progress in each mode. Keep all four scoring policies and the relevant use cases on the main page. Use English for class names and members and Spanish for the notes. I want the engine to be the entry point for use cases: it delegates the outcome decision to the rules and result construction to the context. Review the changes I made and start the usage log.
- **Scoring and progress:** Each score policy calculates a move's score delta. At the end of a game, Lines and Merge use `ScoreResult` to record the final score, including zero. Sort uses `SortResult` to record a completed level on victory. Fit uses `FitResult` to record a star rating on victory. The progress strategy translates the result. `SaveProgressUseCase` writes an entry when the strategy produces one. Level loading and player progress lookup have separate repository ports.
- **Team decisions and edits:** The team placed `gameID: GameID` on `GameContext` and `levelID: LevelID` on the Fit and Sort contexts. The diagram identifies individual runs with `GameID` and lists the four modes in `HexaModes`. The team added five engine states. After resolution, the engine maps the three possible `GameOutcome` values (`PLAYING`, `VICTORY`, `DEFEAT`) to states. Pause and resume are engine operations. The intended contract allows use cases to request a final result through the engine only after victory or defeat.
- **AI output corrected during review:** An earlier edit removed `SortScorePolicy` and `FitScorePolicy`. The user caught this by comparing screenshots. Both policies are present again on the main page. At the user's request, Codex also moved the progress classes onto that page and revised the new explanatory notes in Spanish with the requested signature.
- **Open findings in the saved diagram:** (1) The context's parameterless `getEndGameResult()` has no visible source for the outcome required by `SortResult` and `FitResult`. The state interface also takes a move and rules without showing how it supplies the current context to the rules. (2) Saving accepts a run's `GameID` separately from the result, while the repository and progress lookup take a `string` named `gameId`. The contracts still need to distinguish recording one run from querying a player's history by mode. `LevelRated.levelId` is also a `string`, although `FitResult.levelId` is a `LevelID`. (3) The engine gained a fourth type parameter, `TResult`; the note listing its four variant bindings still shows three. The factory and history bounds also reference the earlier form of `GameContext`.
- **Verification and lesson:** Reviewed the official diagram against the previous commit and inspected its XML. The single page contains all four score policies, the progress use cases and the five concrete states linked to `StateEngine`. Tracing a Fit victory from the rule to a stored level rating exposed missing information at method boundaries that a list of classes alone would miss. This review covers the design. Runtime transitions and persistence still require implementation and tests.
- **Follow-up review, 2026-11-24:** The revised diagram uses `RunID` for a single attempt, carries `LevelID` through to `LevelRated`, and passes the context to each state when applying a move. The state interface now accepts the outcome when retrieving the result, but the context still has no parameter for that outcome, and the terminal states have no explicit path to the final context. Looking up player progress across attempts still requires a mode-level query. The level lookup takes both a run and a level identifier, although the level identifier already includes its mode. The generic type annotations noted above are still pending. These are UML findings awaiting team review.
- **Latest diagram review, 2026-11-24:** All five engine states now receive the outcome and the final context in `getEndGameResult`. The concrete contexts accept the outcome and declare their variant-specific result types. The progress use case and repository both query by player and mode. The progress repository returns `ProgressInfo`. The team chose to specify the return type of `LoadProgressUseCase.execute` later. The final diagram defines `ILevelRepository.load(levelId: LevelID): Level`, so the same level definition can be loaded across attempts. The team deferred the `changeScore` event contract to a separate commit.

### 7. Pass scoring events to `GameRules.changeScore`

- **Date / commit:** 2026-11-25. Commit pending.
- **Tool and task:** OpenAI Codex updated the scoring contract in `docs/Solución HexaSuite.drawio`.
- **Prompt (faithful English paraphrase):** Change the scoring method's signature in the diagram. Leave movement and its result for a separate discussion with the team.
- **Result:** Added `events: GameEvent[]` to `changeScore(context: TContext, events: GameEvent[]): TContext`. This gives the rules both inputs required by `IScorePolicy.calculateScoreDelta`.
- **Team decision:** The team approved the signature and deferred deciding which component produces each event and what `applyMovement` should return.
- **Verification:** The saved diagram parsed successfully. Comparing it with the previous version confirmed that the new parameter was the only UML change.

### 8. Build initial boards from levels or seeded generation

- **Date / commit:** 2026-11-25. `docs(architecture): model level and seeded board construction`.
- **Tool and prompt (faithful English paraphrase):** The team asked OpenAI Codex to adapt the board builder for saved level layouts and random initial boards of a given radius. Use independent UML shapes styled like the existing classes and explain what the seed can reproduce.
- **Design:** For a level, the factory supplies its shape and initial cell values. For generation, it selects the radius and calls `withGeneratedValues(source, seed)`. The mode-specific `IBoardValueSource` chooses initial values for cells within that shape. Reproducing the same layout requires the same cells, generator configuration and seed. Subsequent piece draws use their own source.
- **Team revision and review:** The initial revision introduced `TBoard`. Review found that the interface promised a specific board subtype while the implementation returned `IBoard<TCellValue>`. The team resolved this by removing `TBoard` and using the cell value type as the sole generic parameter. The saved diagram confirms that `IBuilder<TCellValue>` and `BoardBuilder<TCellValue>` both declare `build(): IBoard<TCellValue>`.
- **Verification:** The official draw.io XML parsed successfully. The builder and its interfaces are independent shapes on the main layer. Cell identifiers are unique and parent and connector references resolve. Reproducibility was reviewed at the design level.

### 9. Fuse Merge tiles inside the slide move

- **Date / commit:** 2026-11-26. `docs(architecture): fuse tiles inside the Merge slide move`.
- **Tool:** Claude Code (Claude Opus 5.5).
- **Prompt (faithful English paraphrase of successive requests):** Review the open pull request and my teammate's comments. For Merge, which step does the move perform and which does the resolution perform? Does it make sense for the move to fuse tiles? Apply what we agreed to the official diagram on a new branch.
- **Discussion:** The instructor clarified that a Merge move slides a single tile, not the whole board. Claude Code noted that a collision decides where the tile stops, so fusing in the resolution would require the context to remember which tile moved and in which direction. It also noted that the generic `SlideMovement<TCellValue, TContext>` cannot double an arbitrary cell value, and recommended a Merge-specific move.
- **Team decisions:** The move slides the tile and fuses it on collision with an equal tile, emitting `TilesMerged`. The resolution only spawns a new tile (2 or 4) and checks the end of the game. A fused tile cannot fuse again in the same move, so a move emits at most one `TilesMerged`. A move that changes nothing is invalid.
- **Diagram delivery:** Renamed `SlideMovement` to `MergeSlideMovement` bound to `number`, `MergeContext` and the new `SlideTile` move (`origin` and `direction`). Updated `MergeResolution`, `MergeFactory`, the generic bindings note and the move proposal note, and added a note explaining the decision. The edit changed the XML text in place to keep existing identifiers and connectors.
- **Verification:** The XML parsed successfully. The number of overlapping shapes stayed at seven, all pre-existing. The affected classes, the new dependency and the note were inspected in the draw.io viewer.
- **Open points:** The movement still returns `ResolutionResult`, and `GameRules.applyMovement` still discards the events. The team has not yet agreed on how movement and resolution events are combined.

## Critical evaluation

- **Approximate share of AI-assisted code:** _TODO_
- **Incorrect or suboptimal AI output and how it was caught:** In task 6, the team compared successive diagrams and identified two missing scoring policies. Reviewing the editable source confirmed that the revised page contained all four.
- **Impact on productivity and code quality:** _TODO_

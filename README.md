# HexaBlock Suite — Game Client

![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)
<!-- TODO: add CI, unit, integration and e2e test badges once the GitHub Actions workflow exists. -->

> Four games, one engine.

## Description

HexaBlock Suite is a mobile application that bundles four casual puzzle games from the HexaBlock family
(hexagonal-board puzzles) on top of a single **generic game engine**. Each variant is an instantiation of
the engine, not a copy of it, so new variants can be added without modifying existing code.

- **Tech stack:** Flutter (Dart)
- **Backend:** REST API in a separate repository (Node.js + TypeScript) — _link TBD_

## Demo / Screenshots

_TODO: screenshots or animated GIF of the four variants._

## Architecture

The project follows Robert C. Martin's **Clean Architecture**: four concentric layers where dependencies
always point inward.

| Layer | Contents |
|-------|----------|
| 1. Entities (Domain) | Generic engine: `HexBoard<TCell>`, `HexCoord`, `Piece`, `GameEngine`, variant rules, scoring policies |
| 2. Use Cases (Application) | Generic use cases and ports: `StartGameUseCase`, `ApplyMoveUseCase`, `UndoMoveUseCase`, ... |
| 3. Interface Adapters | Presenters / ViewModels, repository implementations, mappers |
| 4. Frameworks & Drivers | Flutter UI, local storage, HTTP client |

![Clean Architecture layers](docs/diagrams/clean-architecture.svg)

Editable source: [`docs/diagrams/clean-architecture.drawio`](docs/diagrams/clean-architecture.drawio) (open it with
[diagrams.net](https://app.diagrams.net) or the draw.io VS Code extension).

## Generic Engine

_TODO: describe the engine's type parameters, their bounds and how each variant instantiates them._
See [docs/EXTENSIBILITY.md](docs/EXTENSIBILITY.md) for a step-by-step guide to adding a new variant.

## Design Patterns

| Pattern | Category | Where | Why |
|---------|----------|-------|-----|
| _TODO_ | | | |

## SOLID Principles

_TODO: one concrete example from the codebase per principle._

## AOP

_TODO: describe the implemented aspects and the SOLID-based strategy (decorators / proxies over use cases)._

## Getting Started

### Prerequisites

- Flutter SDK (stable channel)

### Installation

```bash
git clone https://github.com/Brillem/hexablock-suite-app.git
cd hexablock-suite-app
flutter pub get
flutter run
```

## Running Tests

```bash
flutter test
```

## AI Usage Documentation

AI tools are used in this project under mandatory documentation. See [AI_USAGE.md](AI_USAGE.md).

## Contributing

### Commit convention

Commit messages are written in English and follow [Conventional Commits](https://www.conventionalcommits.org/):

```text
feat(engine): add generic HexBoard with axial coordinates
fix(hexa-merge): correct slide direction on north-west axis
test(engine): add unit tests for GameEngine with fake cell type
docs(readme): add extensibility guide
```

### Branch workflow

- `main` is protected: no direct pushes, every change goes through a pull request.
- Create a branch per change: `feat/<scope>-<short-name>`, `fix/<scope>-<short-name>`, `docs/<short-name>`.
- Open a pull request to `main`, wait for CI to pass and for at least one approval, then merge.

## License

This project is licensed under the [MIT License](LICENSE).

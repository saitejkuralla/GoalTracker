# Implementation Plan: Goal Tracker

**Branch**: `001-goal-tracker` | **Date**: 2026-07-17 | **Spec**: [specs/001-goal-tracker/spec.md](specs/001-goal-tracker/spec.md)

**Input**: Feature specification from `/specs/001-goal-tracker/spec.md`

## Summary

Build a .NET 8 ASP.NET Core Web API for managing employee goals with layered Controllers, Services, and Repositories. The API will support CRUD operations for goals, status updates with a sequential lifecycle, filtering by employee or status, and status history tracking using an in-memory ConcurrentDictionary store.

## Technical Context

**Language/Version**: C# / .NET 8

**Primary Dependencies**: ASP.NET Core Web API, Swashbuckle / Swagger, xUnit (for future tests)

**Storage**: In-memory ConcurrentDictionary only; no database

**Testing**: xUnit with FluentAssertions or built-in assertions for unit and integration tests

**Target Platform**: Cross-platform web API service

**Project Type**: Web-service

**Performance Goals**: Support typical in-memory operations for a small to medium set of goals with low latency

**Constraints**: No database, no authentication, no external services; maintain clear separation of concerns and async/await usage throughout

**Scale/Scope**: Single API service with goal and status history data for a modest number of employees and goals

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

- Architecture: Features MUST follow Controllers -> Services -> Repositories layering, with business logic contained in services.
- Async: Service and repository APIs MUST use async/await for asynchronous flows and long-running work.
- Data Storage: In-memory repositories are the default persistence boundary unless a durable store is explicitly required.
- Testing: Business logic MUST be testable in isolation, with unit tests covering service behavior before controller integration.

## Project Structure

### Documentation (this feature)

```text
specs/001-goal-tracker/
├── plan.md
├── research.md
├── data-model.md
├── quickstart.md
├── contracts/
└── tasks.md
```

### Source Code (repository root)

```text
Controllers/
├── GoalsController.cs
Services/
├── IGoalService.cs
├── GoalService.cs
Repositories/
├── IGoalRepository.cs
├── InMemoryGoalRepository.cs
Models/
├── Goal.cs
├── GoalStatus.cs
├── GoalStatusHistoryEntry.cs
├── CreateGoalRequest.cs
├── UpdateGoalStatusRequest.cs
├── GoalFilterRequest.cs
Tests/
├── Unit/
└── Integration/
```

**Structure Decision**: Implement the feature as a single ASP.NET Core Web API project with explicit Controllers, Services, and Repositories folders, plus model and test folders. The in-memory repository will be injected into the service layer through interfaces.

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

No constitutional violations are expected for this feature.

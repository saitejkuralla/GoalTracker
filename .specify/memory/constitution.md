<!--
Sync Impact Report
- Version change: 0.0.0 -> 1.0.0
- Modified principles: None -> Layered Architecture, Async-by-Default, In-Memory Persistence by Default, Testable Business Logic
- Added sections: Architecture Constraints, Development Workflow
- Removed sections: None
- Templates requiring updates: ✅ .specify/templates/plan-template.md, ✅ .specify/templates/spec-template.md, ✅ .specify/templates/tasks-template.md
- Follow-up TODOs: None
-->

# GoalTracker Constitution

## Core Principles

### I. Layered Architecture
Controllers MUST remain thin entry points, Services MUST contain business rules and orchestration, and Repositories MUST own data access boundaries. Cross-layer dependencies MUST flow inward from Controllers to Services to Repositories, and no business logic may be embedded in controllers or route handlers.

### II. Async-by-Default
All non-blocking operations, including repository access, service orchestration, and external calls, MUST use async/await and Task-based APIs. Synchronous work MUST be justified and isolated, and long-running work MUST not block request handling.

### III. In-Memory Persistence by Default
This service MUST use in-memory repositories as the default persistence boundary unless a future requirement explicitly introduces a durable store. Repository implementations MUST expose a stable interface so business logic remains independent from storage details.

### IV. Testable Business Logic
Business rules MUST be implemented in services or domain logic that can be exercised without ASP.NET runtime dependencies. New behavior MUST be covered by automated tests that validate the service logic independently from controllers and HTTP concerns.

### V. Separation of Concerns
Each component MUST have one primary responsibility and expose a narrow contract. Shared state and side effects MUST be encapsulated behind interfaces or dedicated collaborators so the system stays easy to evolve and reason about.

## Architecture Constraints

Features for this project MUST follow a clear layering model:
- Controllers handle request and response concerns only.
- Services define workflows, validation, and business rules.
- Repositories provide storage access and data mapping.
- Infrastructure concerns such as dependency injection and configuration MUST stay outside business logic.

When introducing new functionality, async operations MUST be designed from the start and in-memory repository abstractions MUST be preferred unless persistence requirements are explicit.

## Development Workflow

All changes MUST preserve the layered boundary and keep business logic testable. Before implementation, the relevant service and repository contracts MUST be identified, and unit tests for business behavior MUST be written or updated. Pull requests MUST verify that the change remains compatible with the architecture, async conventions, and in-memory persistence model.

## Governance

This constitution supersedes ad-hoc architectural shortcuts. Amendments require a documented rationale, updates to affected templates or guidance, and review that verifies the change preserves the layered, async, testable, and in-memory-first model.

**Version**: 1.0.0 | **Ratified**: 2026-07-17 | **Last Amended**: 2026-07-17

# System Patterns *Optional*

This file documents recurring patterns and standards used in the project.
It is optional, but recommended to be updated as the project evolves.
2025-06-04 10:46:50 - Log of updates made.

## Coding Patterns

* **TypeScript Throughout**: Full type safety from frontend to backend with shared interface definitions
* **Microservices with Shared Types**: Common TypeScript interfaces exported from shared package for API consistency
* **Real-time Event Patterns**: WebSocket message typing with discriminated unions for type-safe event handling
* **Error Boundary Pattern**: React error boundaries for graceful UI failure handling
* **Repository Pattern**: Database access abstraction for testability and maintainability
* **Factory Pattern**: Canvas controller instantiation based on hardware type

## Architectural Patterns

* **Microservices Architecture**: Service separation by domain (Show Control, Content Management, Interactive)
* **API Gateway Pattern**: Single entry point with routing, authentication, and rate limiting
* **CQRS Pattern**: Command-Query Responsibility Segregation for read/write optimization
* **Event Sourcing**: Canvas state changes tracked as immutable events for audit and replay
* **Circuit Breaker**: Hardware communication failure handling with automatic recovery
* **Observer Pattern**: Real-time state propagation across WebSocket connections
* **Strategy Pattern**: Canvas-specific control implementations with common interface

## Testing Patterns

* **Unit Testing**: Jest + React Testing Library for component and service testing
* **Integration Testing**: Supertest for API endpoint validation
* **End-to-End Testing**: Playwright for full workflow validation
* **Hardware Mocking**: Mock hardware interfaces for development and testing environments
* **Load Testing**: Artillery for performance validation under concurrent user scenarios
* **Chaos Engineering**: Controlled failure injection to validate resilience patterns

2025-06-04 11:36:53 - Initial architectural patterns documented for Trocadero control system
# Agent: Software Architect
**Principles**: SOLID, Clean Architecture, Hexagonal Design.

## Responsabilidades Técnicas
Al recibir una HU validada, debe generar un `blueprint.md` con:
1. **Domain Model**: Definición de Entidades y Value Objects (independientes del framework).
2. **API Contract (OpenAPI/Swagger)**:
   - Definición de Endpoints, Verbos y Códigos de Respuesta (201 Created, 204 No Content, 400, 404).
3. **Data Flow**:
   - **Back**: Definir el `Module` de NestJS, el `Service` (Lógica de negocio) y el `Controller`.
   - **Front**: Definir el `Store` (Signals o NgRx), `Services` de comunicación y `Smart Components`.

## Reglas de Oro
- **DTOs Únicos**: Obligar al uso de DTOs compartidos para que el Front y Back estén siempre sincronizados.
- **Dependency Injection**: Diseñar interfaces para que el código sea testeable (Mocking).
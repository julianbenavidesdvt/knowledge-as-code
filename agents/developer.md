# Agent: Senior Fullstack Developer
**Stack**: NestJS (Back) + Angular (Front) + Nx (Monorepo Tools).

## Estándares de Codificación
1. **NestJS (Backend)**:
   - Uso estricto de **Pipes** para validación de entrada (`class-validator`).
   - Implementación de **Interceptors** para transformar respuestas.
   - **Manejo de Errores**: Uso de `HttpExceptions` personalizadas.
   - Inyección de dependencias basada en interfaces.

2. **Angular (Frontend)**:
   - **Componentes**: Separación entre Presentational (UI) y Container (Lógica).
   - **State Management**: Uso de Angular Signals para un rendimiento óptimo.
   - **Change Detection**: Estrategia `OnPush`.
   - **Routing**: Lazy loading por módulo/feature.

## Ciclo de Ejecución (Loop)
1. **Implementación**: Escribir código en la rama correspondiente.
2. **Refactor**: Revisar el código buscando duplicidad (DRY).
3. **Handover**: Generar un archivo temporal `/internal/commit_msg.txt` con los cambios técnicos realizados.
---
name: test_manager
description: Skill para gestionar la planificación, ejecución, y documentación de pruebas siguiendo mejores prácticas de testing.
---

# Gestor de Testing

Esta skill centraliza todas las operaciones relacionadas con testing: generación de planes de prueba, ejecución de tests, análisis de cobertura, y documentación de resultados.

## Instrucciones

### 1. Generar Plan de Pruebas (Generate Test Plan)

Utiliza esta función para crear un plan de pruebas estructurado basado en los artefactos de diseño.

1. **Cargar Contexto de Feature**:
   - Obtén la rama actual: `git branch --show-current`
   - Define `FEATURE_DIR` como `specs/[RAMA]/`
   - Lee `spec.md` para extraer User Stories y criterios de aceptación
   - Lee `plan.md` para entender el stack técnico
   - Lee `data-model.md` para identificar entidades a probar
   - Lee `contracts/` para definir contract tests

2. **Identificar Tipos de Tests Requeridos**:

   | Tipo | Cuándo Aplicar | Prioridad |
   |------|----------------|-----------|
   | Unit Tests | Siempre para lógica de negocio | Alta |
   | Integration Tests | Cuando hay interacción entre componentes | Alta |
   | Contract Tests | Cuando hay APIs definidas en contracts/ | Media |
   | E2E Tests | Para User Stories críticas (P1) | Media |
   | Performance Tests | Si spec menciona requisitos de rendimiento | Baja |

3. **Generar Estructura de Tests**:
   ```
   tests/
   ├── unit/                 # Tests unitarios
   │   ├── models/          # Tests de entidades/modelos
   │   ├── services/        # Tests de servicios
   │   └── utils/           # Tests de utilidades
   ├── integration/          # Tests de integración
   │   ├── api/             # Tests de endpoints
   │   └── database/        # Tests de persistencia
   ├── contract/             # Contract tests (OpenAPI)
   ├── e2e/                  # Tests end-to-end
   └── fixtures/             # Datos de prueba compartidos
   ```

4. **Crear test-plan.md**:
   Genera el archivo `FEATURE_DIR/test-plan.md` con:
   - Matriz de cobertura (User Story → Tests)
   - Casos de prueba por categoría
   - Datos de prueba requeridos
   - Criterios de éxito

### 2. Ejecutar Tests (Run Tests)

Utiliza esta función para ejecutar tests y capturar resultados.

1. **Detectar Framework de Testing**:

   | Stack | Framework | Comando |
   |-------|-----------|---------|
   | NestJS/Node | Jest | `npm test` / `npm run test:cov` |
   | Angular | Jasmine/Karma | `ng test --code-coverage` |
   | Python | Pytest | `pytest --cov` |
   | Go | testing | `go test -cover ./...` |

2. **Ejecutar por Categoría**:
   ```bash
   # Unit tests (rápidos, primero)
   npm run test:unit

   # Integration tests
   npm run test:integration

   # E2E tests (lentos, último)
   npm run test:e2e
   ```

3. **Capturar Métricas**:
   - Total de tests ejecutados
   - Tests pasados/fallados
   - Cobertura de código (líneas, ramas, funciones)
   - Tiempo de ejecución

### 3. Analizar Cobertura (Analyze Coverage)

Utiliza esta función para evaluar la calidad del testing.

1. **Leer Reporte de Cobertura**:
   - Buscar `coverage/lcov-report/index.html` (Jest/Istanbul)
   - Buscar `coverage.xml` o `.coverage` (Python)
   - Parsear métricas clave

2. **Evaluar contra Objetivos**:

   | Categoría | Objetivo Mínimo | Óptimo |
   |-----------|-----------------|--------|
   | Lógica de negocio | 80% | 90%+ |
   | Servicios | 75% | 85%+ |
   | Controllers/Endpoints | 70% | 80%+ |
   | Utilidades | 60% | 70%+ |

3. **Identificar Gaps**:
   - Listar archivos sin cobertura
   - Identificar ramas no cubiertas
   - Mapear gaps a User Stories

### 4. Documentar Resultados (Document Results)

Utiliza esta función después de ejecutar tests para crear documentación.

1. **Crear test-report.md**:
   Genera `FEATURE_DIR/test-report.md` con:

   ```markdown
   # Test Report: [Feature Name]

   ## Resumen Ejecutivo
   - **Fecha**: [YYYY-MM-DD]
   - **Rama**: [branch-name]
   - **Estado**: ✅ PASS / ❌ FAIL

   ## Métricas
   | Métrica | Valor | Objetivo | Estado |
   |---------|-------|----------|--------|
   | Tests Totales | X | - | - |
   | Tests Pasados | X | 100% | ✅/❌ |
   | Cobertura Líneas | X% | 80% | ✅/❌ |
   | Cobertura Ramas | X% | 75% | ✅/❌ |

   ## Resultados por Categoría
   ### Unit Tests
   - ✅ [test_name]: descripción
   - ❌ [test_name]: descripción (razón del fallo)

   ## Cobertura por User Story
   | User Story | Tests | Cobertura | Estado |
   |------------|-------|-----------|--------|
   | US1 | 5 | 85% | ✅ |
   | US2 | 3 | 72% | ⚠️ |

   ## Issues Encontrados
   1. [ISSUE-001] Descripción del problema

   ## Próximos Pasos
   - [ ] Acción requerida
   ```

2. **Actualizar tasks.md**:
   - Marcar tareas de testing como completadas [X]
   - Agregar notas sobre resultados

### 5. Validar Calidad de Tests (Validate Test Quality)

Utiliza esta función para evaluar si los tests siguen mejores prácticas.

1. **Verificar Patrón AAA**:
   - Cada test debe tener: Arrange, Act, Assert
   - No debe haber lógica compleja en tests

2. **Verificar Naming**:
   - Formato: `should_[behavior]_when_[condition]`
   - Nombres descriptivos que documentan comportamiento

3. **Verificar Independencia**:
   - Tests no deben depender de orden de ejecución
   - No estado compartido entre tests

4. **Verificar Anti-Patrones**:
   - ❌ Tests sin assertions
   - ❌ Tests que prueban implementación, no comportamiento
   - ❌ Tests flaky (resultados inconsistentes)
   - ❌ Tests con sleep/wait hardcodeados

### 6. Generar Fixtures (Generate Fixtures)

Utiliza esta función para crear datos de prueba consistentes.

1. **Analizar data-model.md**:
   - Extraer entidades y sus campos
   - Identificar relaciones entre entidades

2. **Generar Fixtures Base**:
   ```typescript
   // tests/fixtures/user.fixture.ts
   export const validUser = {
     id: 'user-001',
     email: 'test@example.com',
     name: 'Test User',
     createdAt: new Date('2024-01-01'),
   };

   export const invalidUser = {
     id: '',
     email: 'invalid-email',
     name: '',
   };
   ```

3. **Crear Factory Functions**:
   ```typescript
   export function createUser(overrides?: Partial<User>): User {
     return {
       ...validUser,
       ...overrides,
       id: overrides?.id ?? `user-${Date.now()}`,
     };
   }
   ```

## Flujo de Trabajo Recomendado

```
1. Después de /tasks (tareas generadas)
   └─> test_manager: Generate Test Plan
       └─> Crea test-plan.md con matriz de cobertura

2. Durante /implement (ejecución de tareas)
   └─> test_manager: Run Tests (por cada fase)
       └─> Ejecuta tests y reporta resultados

3. Al completar User Story
   └─> test_manager: Analyze Coverage
       └─> Valida cobertura contra objetivos

4. Al finalizar feature
   └─> test_manager: Document Results
       └─> Genera test-report.md final
```

## Integración con Otros Skills

- **task_manager**: Coordina prerrequisitos antes de ejecutar tests
- **context_manager**: Actualiza documentación con métricas de testing
- **git_manager**: Commits de tests separados de implementación

## Comandos de Referencia

### NestJS/Jest
```bash
# Ejecutar todos los tests
npm test

# Con cobertura
npm run test:cov

# Watch mode
npm run test:watch

# Tests específicos
npm test -- --testPathPattern="user"
```

### Angular/Jasmine
```bash
# Unit tests
ng test --code-coverage

# E2E tests
ng e2e
```

### Python/Pytest
```bash
# Todos los tests
pytest

# Con cobertura
pytest --cov=src --cov-report=html

# Tests específicos
pytest tests/unit/ -v
```

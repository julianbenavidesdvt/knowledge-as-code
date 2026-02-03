---
description: Ejecutar proceso completo de testing para una feature, incluyendo planificación, ejecución, y documentación de pruebas.
---

## User Input

```text
$ARGUMENTS
```

Argumentos opcionales:
- `plan`: Solo generar plan de pruebas
- `run`: Solo ejecutar tests
- `report`: Solo generar reporte
- `full`: Proceso completo (default)

## Outline

### 1. Setup: Verificar Contexto

1. **Identificar Feature**:
   - Obtener rama actual: `git branch --show-current`
   - Verificar formato de rama de feature (N-nombre)
   - Definir `FEATURE_DIR` como `specs/[RAMA]/`

2. **Verificar Prerrequisitos**:
   - `spec.md` debe existir (requerido)
   - `plan.md` debe existir (requerido)
   - `tasks.md` debe existir (recomendado)
   - `data-model.md` (opcional, mejora calidad del plan)
   - `contracts/` (opcional, para contract tests)

3. **Detectar Stack Técnico**:
   - Leer `plan.md` sección "Technical Context"
   - Identificar framework de testing a usar
   - Determinar estructura de directorios de tests

---

### 2. Fase 1: Planificación de Tests

**Objetivo**: Crear un plan de pruebas trazable a User Stories

1. **Cargar Artefactos de Diseño**:
   ```
   spec.md → User Stories + Criterios de Aceptación
   plan.md → Stack técnico + Arquitectura
   data-model.md → Entidades + Relaciones
   contracts/ → Endpoints + Schemas
   ```

2. **Generar Matriz de Cobertura**:

   Para cada User Story en spec.md:
   - Identificar criterios de aceptación testables
   - Mapear a tipos de test apropiados
   - Asignar prioridad basada en prioridad de US

   | User Story | Criterio | Tipo Test | Prioridad | ID Test |
   |------------|----------|-----------|-----------|---------|
   | US1 | Usuario puede login | Integration | P1 | IT-001 |
   | US1 | Token se genera | Unit | P1 | UT-001 |
   | US2 | Lista usuarios | E2E | P2 | E2E-001 |

3. **Identificar Casos de Prueba**:

   **Para cada criterio, definir**:
   - Happy Path (caso exitoso)
   - Edge Cases (límites)
   - Error Cases (fallos esperados)

   **Ejemplo**:
   ```markdown
   ## UT-001: Token Generation

   ### Happy Path
   - should_generate_valid_jwt_when_credentials_correct

   ### Edge Cases
   - should_handle_empty_password
   - should_handle_max_length_username

   ### Error Cases
   - should_throw_when_user_not_found
   - should_throw_when_account_locked
   ```

4. **Crear test-plan.md**:

   Guardar en `FEATURE_DIR/test-plan.md`:

   ```markdown
   # Test Plan: [Feature Name]

   ## Metadata
   - **Feature**: [branch-name]
   - **Fecha**: [YYYY-MM-DD]
   - **Autor**: AI Agent

   ## Alcance

   ### En Alcance
   - [Lista de componentes a probar]

   ### Fuera de Alcance
   - [Componentes excluidos y razón]

   ## Matriz de Cobertura
   [Tabla User Story → Tests]

   ## Casos de Prueba

   ### Unit Tests
   [Lista detallada]

   ### Integration Tests
   [Lista detallada]

   ### E2E Tests (si aplica)
   [Lista detallada]

   ## Datos de Prueba
   [Fixtures requeridos]

   ## Criterios de Éxito
   - Cobertura mínima: 80%
   - Todos los tests P1 pasan
   - No tests flaky
   ```

---

### 3. Fase 2: Preparación de Infraestructura de Tests

**Objetivo**: Configurar el entorno de testing

1. **Verificar/Crear Estructura de Directorios**:
   ```
   tests/
   ├── unit/
   ├── integration/
   ├── e2e/           # Solo si hay tests E2E
   ├── contract/      # Solo si existe contracts/
   └── fixtures/
   ```

2. **Configurar Framework de Testing**:

   **NestJS (Jest)**:
   - Verificar `jest.config.js` existe
   - Verificar scripts en `package.json`:
     ```json
     {
       "test": "jest",
       "test:watch": "jest --watch",
       "test:cov": "jest --coverage",
       "test:e2e": "jest --config ./test/jest-e2e.json"
     }
     ```

   **Angular (Jasmine/Karma)**:
   - Verificar `karma.conf.js` existe
   - Verificar `angular.json` tiene configuración de test

3. **Crear Fixtures Base**:

   Basándose en `data-model.md`, generar:
   ```typescript
   // tests/fixtures/index.ts
   export * from './user.fixture';
   export * from './[entity].fixture';
   ```

4. **Configurar Mocks Globales**:

   ```typescript
   // tests/mocks/database.mock.ts
   export const mockRepository = {
     find: jest.fn(),
     findOne: jest.fn(),
     save: jest.fn(),
     delete: jest.fn(),
   };
   ```

---

### 4. Fase 3: Ejecución de Tests

**Objetivo**: Ejecutar tests de forma ordenada y capturar resultados

1. **Orden de Ejecución**:
   ```
   1. Unit Tests (más rápidos, aislados)
   2. Integration Tests (requieren más setup)
   3. Contract Tests (validan APIs)
   4. E2E Tests (más lentos, último)
   ```

2. **Ejecutar Unit Tests**:
   ```bash
   # NestJS
   npm run test -- --testPathPattern="unit" --coverage

   # Angular
   ng test --include="**/unit/**" --code-coverage
   ```

   **Capturar**:
   - Número de tests ejecutados
   - Tests pasados/fallados
   - Cobertura de líneas/ramas

3. **Ejecutar Integration Tests**:
   ```bash
   # NestJS
   npm run test -- --testPathPattern="integration"

   # Con base de datos de prueba
   DATABASE_URL=test npm run test:integration
   ```

4. **Ejecutar Contract Tests** (si existen):
   ```bash
   # Validar contra OpenAPI spec
   npm run test:contract
   ```

5. **Ejecutar E2E Tests** (si existen):
   ```bash
   # NestJS
   npm run test:e2e

   # Angular
   ng e2e
   ```

6. **Consolidar Resultados**:

   Crear resumen temporal:
   ```json
   {
     "unit": { "total": 50, "passed": 48, "failed": 2, "coverage": 85 },
     "integration": { "total": 20, "passed": 20, "failed": 0 },
     "e2e": { "total": 5, "passed": 5, "failed": 0 },
     "overall": { "passed": true, "coverage": 82 }
   }
   ```

---

### 5. Fase 4: Análisis de Cobertura

**Objetivo**: Evaluar calidad del testing vs objetivos

1. **Leer Reporte de Cobertura**:
   - Localizar `coverage/lcov-report/index.html`
   - Parsear métricas principales

2. **Evaluar por Categoría**:

   | Categoría | Objetivo | Actual | Estado |
   |-----------|----------|--------|--------|
   | Modelos | 90% | X% | ✅/❌ |
   | Servicios | 85% | X% | ✅/❌ |
   | Controllers | 80% | X% | ✅/❌ |
   | Utils | 70% | X% | ✅/❌ |

3. **Mapear Cobertura a User Stories**:

   Para cada User Story:
   - Calcular % de tests pasando
   - Calcular % de código cubierto
   - Identificar gaps críticos

4. **Identificar Deuda Técnica de Testing**:
   - Archivos con 0% cobertura
   - Ramas no cubiertas en lógica crítica
   - Tests marcados como `.skip` o `@Ignore`

---

### 6. Fase 5: Documentación de Resultados

**Objetivo**: Generar documentación completa y actionable

1. **Generar test-report.md**:

   Crear `FEATURE_DIR/test-report.md`:

   ```markdown
   # Test Report: [Feature Name]

   ## Resumen Ejecutivo

   | Métrica | Valor |
   |---------|-------|
   | **Fecha de Ejecución** | YYYY-MM-DD HH:MM |
   | **Rama** | N-feature-name |
   | **Estado General** | ✅ PASS / ❌ FAIL |
   | **Tests Totales** | X |
   | **Tests Pasados** | X (Y%) |
   | **Tests Fallados** | X |
   | **Cobertura Total** | X% |

   ## Resultados por Tipo de Test

   ### Unit Tests
   - **Total**: X
   - **Pasados**: X ✅
   - **Fallados**: X ❌
   - **Cobertura**: X%

   #### Tests Fallados
   | Test | Archivo | Error |
   |------|---------|-------|
   | should_xxx | file.spec.ts | Expected X but got Y |

   ### Integration Tests
   [Similar estructura]

   ## Cobertura por User Story

   | User Story | Tests | Pasados | Cobertura | Estado |
   |------------|-------|---------|-----------|--------|
   | US1 - Login | 15 | 15 | 92% | ✅ |
   | US2 - Dashboard | 10 | 9 | 78% | ⚠️ |

   ## Cobertura por Archivo

   ### Archivos con Cobertura Insuficiente (<80%)
   | Archivo | Líneas | Ramas | Funciones |
   |---------|--------|-------|-----------|
   | user.service.ts | 65% | 50% | 70% |

   ### Archivos sin Cobertura
   - src/utils/legacy.ts (excluido intencionalmente)

   ## Issues y Observaciones

   ### Críticos
   1. [TEST-001] Descripción del issue crítico

   ### Mejoras Sugeridas
   1. Agregar tests para edge case X

   ## Próximos Pasos

   - [ ] Resolver tests fallados
   - [ ] Aumentar cobertura en user.service.ts
   - [ ] Agregar test E2E para flujo de checkout

   ## Apéndice: Comandos de Ejecución

   ```bash
   # Reproducir resultados
   npm run test:cov
   ```
   ```

2. **Crear Checklist de Testing**:

   Crear `FEATURE_DIR/checklists/testing.md`:

   ```markdown
   # Testing Checklist: [Feature Name]

   ## Completitud
   - [ ] Todos los criterios de aceptación tienen tests
   - [ ] Happy paths cubiertos para cada US
   - [ ] Edge cases identificados y probados
   - [ ] Error handling verificado

   ## Calidad
   - [ ] Tests siguen patrón AAA
   - [ ] Naming convention consistente
   - [ ] No tests flaky
   - [ ] Mocks realistas

   ## Cobertura
   - [ ] Cobertura total >= 80%
   - [ ] Lógica de negocio >= 90%
   - [ ] Sin archivos críticos a 0%

   ## Documentación
   - [ ] test-plan.md completo
   - [ ] test-report.md generado
   - [ ] Fixtures documentados
   ```

3. **Actualizar tasks.md**:
   - Marcar tareas de testing como [X] completadas
   - Agregar notas sobre cobertura alcanzada

---

### 7. Fase 6: Validación Final

**Objetivo**: Verificar que el testing cumple estándares de calidad

1. **Gate de Calidad**:

   | Criterio | Requerido | Actual | Estado |
   |----------|-----------|--------|--------|
   | Tests P1 pasan | 100% | X% | ✅/❌ |
   | Cobertura total | ≥80% | X% | ✅/❌ |
   | No tests flaky | 0 | X | ✅/❌ |
   | Docs generados | Sí | Sí/No | ✅/❌ |

2. **Decisión**:

   - **PASS**: Todos los criterios cumplidos → Feature ready para merge
   - **WARN**: Cobertura baja pero tests pasan → Revisar deuda técnica
   - **FAIL**: Tests fallando → Bloquear merge hasta resolver

3. **Reporte Final**:

   Mostrar resumen al usuario:
   ```
   ╔═══════════════════════════════════════╗
   ║      TEST EXECUTION COMPLETE          ║
   ╠═══════════════════════════════════════╣
   ║ Status:     ✅ PASS                   ║
   ║ Tests:      75/75 passed              ║
   ║ Coverage:   84%                       ║
   ║ Duration:   2m 34s                    ║
   ╠═══════════════════════════════════════╣
   ║ Reports generated:                    ║
   ║ • specs/5-auth/test-plan.md          ║
   ║ • specs/5-auth/test-report.md        ║
   ║ • specs/5-auth/checklists/testing.md ║
   ╚═══════════════════════════════════════╝
   ```

---

## Integración con Flujo de Desarrollo

Este workflow se integra en el ciclo de desarrollo así:

```
/specify → /plan → /tasks → /test plan → /implement → /test run → /test report
                              ↑                              ↑
                       Plan de pruebas              Ejecución y reporte
```

### Puntos de Invocación Recomendados

1. **Después de /tasks**: Ejecutar `/test plan` para generar plan de pruebas
2. **Durante /implement**: Los tests se escriben como parte de las tareas
3. **Al completar cada User Story**: Ejecutar `/test run` para validar
4. **Antes de merge**: Ejecutar `/test full` para reporte completo

---

## Notas

- Este workflow asume que el framework de testing está configurado
- Para proyectos nuevos, usar `/iniciar` primero para setup inicial
- Los tests E2E son opcionales y solo se ejecutan si existen
- La cobertura objetivo es configurable en constitution.md

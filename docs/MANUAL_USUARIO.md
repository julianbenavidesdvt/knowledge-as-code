# Manual de Usuario: Knowledge as Code - Antigravity Workflows

## Tabla de Contenidos

1. [Introducción](#1-introducción)
2. [Arquitectura del Sistema](#2-arquitectura-del-sistema)
3. [Flujo de Desarrollo Completo](#3-flujo-de-desarrollo-completo)
4. [Guía de Workflows](#4-guía-de-workflows)
5. [Guía de Skills](#5-guía-de-skills)
6. [Proceso de Testing](#6-proceso-de-testing)
7. [Mejores Prácticas](#7-mejores-prácticas)
8. [Referencia Rápida](#8-referencia-rápida)

---

## 1. Introducción

### 1.1 Qué es Knowledge as Code

Knowledge as Code es un framework de desarrollo de software que utiliza agentes de IA para gestionar el ciclo completo de desarrollo de features, desde la especificación hasta el deployment. El sistema transforma conocimiento de desarrollo en artefactos estructurados y ejecutables.

### 1.2 Principios Fundamentales

1. **Specification-First**: Todo comienza con una especificación clara
2. **AI-Friendly Documentation**: Documentos estructurados para agentes de IA
3. **Traceability**: Cada tarea se relaciona con un requisito
4. **Independent Delivery**: User Stories son independientes y deployables
5. **Quality Gates**: Validaciones en cada etapa del proceso

### 1.3 Stack Tecnológico

| Componente | Tecnología |
|------------|------------|
| Backend | NestJS + TypeScript |
| Frontend | Angular + TypeScript |
| Base de Datos | PostgreSQL + Sequelize |
| Infraestructura | Google Cloud Platform |
| IaC | Terraform |
| Documentación | Context7 Standard |

---

## 2. Arquitectura del Sistema

### 2.1 Estructura de Directorios

```
knowledge-as-code/
├── .agent/
│   ├── rules/           # Reglas siempre activas
│   │   ├── backend.md   # Guía NestJS/TypeScript
│   │   ├── frontend.md  # Guía Angular/TypeScript
│   │   └── testing.md   # Guía de testing
│   │
│   ├── skills/          # Capacidades reutilizables
│   │   ├── test_manager/    # Gestión de testing
│   │   ├── code_review/     # Revisión de código
│   │   ├── deploy_manager/  # Gestión de deployment
│   │   ├── git_manager/     # Operaciones git
│   │   ├── task_manager/    # Gestión de tareas
│   │   ├── plan_manager/    # Gestión de planes
│   │   ├── context_manager/ # Gestión de contexto
│   │   ├── context7_manager/# Documentación Context7
│   │   ├── terraform_gcp/   # Infraestructura GCP
│   │   └── generate_skill/  # Crear nuevos skills
│   │
│   └── workflows/       # Flujos de trabajo
│       ├── specify.md   # Crear especificación
│       ├── plan.md      # Crear plan técnico
│       ├── tasks.md     # Generar tareas
│       ├── implement.md # Ejecutar implementación
│       ├── test.md      # Proceso de testing
│       ├── analyze.md   # Análisis de consistencia
│       ├── checklist.md # Validación de requisitos
│       └── constitution.md # Gestión de principios
│
├── templates/           # Plantillas de documentos
│   ├── spec-template.md
│   ├── plan-template.md
│   ├── tasks-template.md
│   ├── test-plan-template.md
│   └── checklist-template.md
│
├── memory/              # Constitución del proyecto
│   └── constitution.md
│
└── specs/               # Especificaciones por feature
    └── [N-feature-name]/
        ├── spec.md
        ├── plan.md
        ├── tasks.md
        ├── test-plan.md
        ├── test-report.md
        ├── research.md
        ├── data-model.md
        ├── quickstart.md
        ├── contracts/
        └── checklists/
```

### 2.2 Diagrama de Flujo Principal

```
┌─────────────────────────────────────────────────────────────────┐
│                    FLUJO DE DESARROLLO                          │
└─────────────────────────────────────────────────────────────────┘

  Usuario                  Agente IA                  Artefactos
    │                         │                          │
    │  "Quiero feature X"     │                          │
    │────────────────────────>│                          │
    │                         │                          │
    │                    /specify                        │
    │                         │───────────────────────>  spec.md
    │                         │                          │
    │  (clarificaciones)      │                          │
    │<───────────────────────>│                          │
    │                         │                          │
    │                     /plan                          │
    │                         │───────────────────────>  plan.md
    │                         │───────────────────────>  research.md
    │                         │───────────────────────>  data-model.md
    │                         │───────────────────────>  contracts/
    │                         │                          │
    │                    /tasks                          │
    │                         │───────────────────────>  tasks.md
    │                         │                          │
    │                  /test plan                        │
    │                         │───────────────────────>  test-plan.md
    │                         │                          │
    │                   /analyze                         │
    │                         │───────────────────────>  (reporte)
    │                         │                          │
    │                  /implement                        │
    │                         │───────────────────────>  código fuente
    │                         │───────────────────────>  tests
    │                         │                          │
    │                  /test run                         │
    │                         │───────────────────────>  test-report.md
    │                         │                          │
    │                code_review                         │
    │                         │───────────────────────>  review-report.md
    │                         │                          │
    │               deploy_manager                       │
    │                         │───────────────────────>  release-notes.md
    │                         │                          │
    └─────────────────────────┴──────────────────────────┘
```

---

## 3. Flujo de Desarrollo Completo

### 3.1 Resumen del Ciclo

| Fase | Workflow/Skill | Input | Output | Duración |
|------|----------------|-------|--------|----------|
| 1. Especificación | `/specify` | Descripción natural | spec.md | 15-30 min |
| 2. Planificación | `/plan` | spec.md | plan.md, research.md, etc. | 30-60 min |
| 3. Tareas | `/tasks` | plan.md, spec.md | tasks.md | 15-20 min |
| 4. Plan de Tests | `/test plan` | spec.md, plan.md | test-plan.md | 20-30 min |
| 5. Análisis | `/analyze` | Todos los artefactos | Reporte | 10-15 min |
| 6. Implementación | `/implement` | tasks.md | Código + Tests | Variable |
| 7. Testing | `/test run` | Código implementado | test-report.md | Variable |
| 8. Code Review | `code_review` | Cambios en branch | review-report.md | 15-20 min |
| 9. Deploy | `deploy_manager` | Código aprobado | Release | Variable |

### 3.2 Fase 1: Especificación

**Objetivo**: Transformar una idea en una especificación formal.

**Comando**: `/specify`

**Proceso**:
1. Describes la feature en lenguaje natural
2. El agente genera un nombre corto (2-4 palabras)
3. Se crea una rama de feature: `N-nombre-corto`
4. Se genera `spec.md` con:
   - User Stories priorizadas (P1, P2, P3)
   - Criterios de aceptación
   - Edge cases
   - Assumptions

**Ejemplo**:
```
Usuario: "Quiero un sistema de autenticación con login, registro y recuperación de contraseña"

Agente:
- Rama creada: 5-user-auth
- spec.md generado en specs/5-user-auth/
- 3 User Stories identificadas:
  - US1 (P1): Login con email/password
  - US2 (P1): Registro de usuario
  - US3 (P2): Recuperación de contraseña
```

**Validaciones**:
- Sin detalles de implementación (no frameworks, APIs, etc.)
- Escrito para stakeholders no técnicos
- Máximo 3 marcadores `[NEEDS CLARIFICATION]`

### 3.3 Fase 2: Planificación Técnica

**Objetivo**: Diseñar la arquitectura e implementación.

**Comando**: `/plan`

**Proceso**:
1. El agente lee spec.md
2. Genera artefactos de diseño:
   - `plan.md`: Stack técnico, arquitectura
   - `research.md`: Decisiones técnicas
   - `data-model.md`: Entidades y relaciones
   - `contracts/`: Especificaciones API

**Secciones de plan.md**:
```markdown
## Technical Context
- Language: TypeScript
- Dependencies: NestJS, Passport, JWT
- Storage: PostgreSQL
- Testing: Jest
- Platform: GCP Cloud Run
```

**Gate de Constitución**:
- El plan debe cumplir con los principios en `memory/constitution.md`
- Si viola algún principio, se documenta justificación

### 3.4 Fase 3: Generación de Tareas

**Objetivo**: Crear lista de tareas ejecutables y ordenadas.

**Comando**: `/tasks`

**Proceso**:
1. El agente lee spec.md y plan.md
2. Genera tareas organizadas por:
   - **Fase 1**: Setup (infraestructura inicial)
   - **Fase 2**: Foundational (prerequisitos bloqueantes)
   - **Fase 3+**: User Stories (en orden de prioridad)
   - **Fase Final**: Polish (cross-cutting concerns)

**Formato de Tarea**:
```
- [ ] T001 [P] [US1] Descripción con ruta de archivo src/path/file.ts
```

- `T001`: ID secuencial
- `[P]`: Puede ejecutarse en paralelo
- `[US1]`: Pertenece a User Story 1

### 3.5 Fase 4: Plan de Testing

**Objetivo**: Crear plan de pruebas trazable a requisitos.

**Comando**: `/test plan`

**Proceso**:
1. El agente lee spec.md, plan.md, data-model.md, contracts/
2. Genera `test-plan.md` con:
   - Matriz de cobertura (User Story → Tests)
   - Casos de prueba detallados
   - Fixtures requeridos
   - Criterios de éxito

**Matriz de Cobertura**:
```markdown
| User Story | Criterio | Tipo Test | ID Test |
|------------|----------|-----------|---------|
| US1 | Login exitoso | Integration | IT-001 |
| US1 | Token generado | Unit | UT-001 |
| US1 | Credenciales inválidas | Unit | UT-002 |
```

### 3.6 Fase 5: Análisis de Consistencia

**Objetivo**: Validar que todos los artefactos son consistentes.

**Comando**: `/analyze`

**Proceso**:
1. El agente analiza todos los artefactos
2. Detecta:
   - Duplicaciones
   - Ambigüedades
   - Especificaciones incompletas
   - Inconsistencias entre documentos
   - Gaps de cobertura

**Reporte de Análisis**:
```markdown
## Findings

| ID | Categoría | Severidad | Ubicación | Resumen |
|----|-----------|-----------|-----------|---------|
| F-001 | Ambiguity | MEDIUM | spec.md:45 | "Rápido" no está cuantificado |
| F-002 | Coverage Gap | HIGH | tasks.md | US2 sin tareas de test |
```

### 3.7 Fase 6: Implementación

**Objetivo**: Ejecutar las tareas y escribir código.

**Comando**: `/implement`

**Proceso**:
1. Verifica checklists (si existen)
2. Carga contexto completo (plan, spec, tasks, contracts)
3. Ejecuta tareas fase por fase:
   - Setup primero
   - Tests antes del código (TDD)
   - Implementación core
   - Integración
   - Polish

**Ejecución de Tareas**:
```markdown
## Fase 2: Foundational
- [X] T004 Setup database schema
- [X] T005 [P] Implement auth middleware
- [X] T006 [P] Setup API routing

## Fase 3: User Story 1 - Login
### Tests
- [X] T010 [P] [US1] Integration test for login
- [X] T011 [P] [US1] Unit test for token service

### Implementation
- [X] T012 [P] [US1] Create User entity
- [X] T013 [US1] Implement AuthService
- [X] T014 [US1] Implement login endpoint
```

### 3.8 Fase 7: Ejecución de Tests

**Objetivo**: Ejecutar tests y documentar resultados.

**Comando**: `/test run`

**Proceso**:
1. Ejecuta tests por categoría:
   - Unit tests (rápidos, primero)
   - Integration tests
   - Contract tests (si existen)
   - E2E tests (lentos, último)
2. Captura métricas de cobertura
3. Genera `test-report.md`

**test-report.md**:
```markdown
## Resumen Ejecutivo
| Métrica | Valor | Objetivo | Estado |
|---------|-------|----------|--------|
| Tests Pasados | 48/50 | 100% | ⚠️ |
| Cobertura | 84% | 80% | ✅ |

## Tests Fallados
| Test | Archivo | Error |
|------|---------|-------|
| should_throw_when_expired | auth.spec.ts | Timeout |
```

### 3.9 Fase 8: Code Review

**Objetivo**: Validar calidad del código antes de merge.

**Skill**: `code_review`

**Proceso**:
1. Analiza cambios en la branch
2. Evalúa:
   - Seguridad (OWASP Top 10)
   - Rendimiento (N+1, memory leaks)
   - Mantenibilidad (complejidad, code smells)
   - Adherencia a arquitectura
3. Genera reporte con findings

**Decisiones**:
- ✅ **APPROVE**: Sin issues críticos
- ⚠️ **REQUEST CHANGES**: Issues que deben resolverse
- ❌ **REJECT**: Issues críticos de seguridad

### 3.10 Fase 9: Deployment

**Objetivo**: Desplegar código validado a producción.

**Skill**: `deploy_manager`

**Proceso**:
1. **Pre-Deploy**: Validaciones automáticas
   - Lint, Types, Tests, Build, Security audit
2. **Deploy**: Ejecución según ambiente
   - Dev: Auto-deploy en push
   - Staging: Auto-deploy en merge a main
   - Production: Deploy manual con aprobación
3. **Post-Deploy**: Verificación
   - Health checks
   - Smoke tests
   - Monitoreo inicial
4. **Rollback** (si necesario)

---

## 4. Guía de Workflows

### 4.1 /specify - Crear Especificación

```bash
# Uso
/specify "Descripción de la feature"

# Ejemplo
/specify "Sistema de notificaciones push para alertar usuarios sobre eventos importantes"
```

**Output**:
- Rama: `N-push-notifications`
- Archivo: `specs/N-push-notifications/spec.md`

### 4.2 /plan - Crear Plan Técnico

```bash
# Uso (desde branch de feature)
/plan

# Con argumentos
/plan "Usar Redis para caching"
```

**Output**:
- `plan.md`, `research.md`, `data-model.md`, `contracts/`

### 4.3 /tasks - Generar Tareas

```bash
# Uso
/tasks

# Output
tasks.md con todas las tareas organizadas por fase
```

### 4.4 /test - Testing Workflow

```bash
# Generar plan de tests
/test plan

# Ejecutar todos los tests
/test run

# Solo generar reporte
/test report

# Proceso completo
/test full
```

### 4.5 /analyze - Análisis de Consistencia

```bash
# Uso
/analyze

# Output: Reporte de issues y gaps
```

### 4.6 /implement - Ejecutar Implementación

```bash
# Uso
/implement

# Continuar desde tarea específica
/implement "Continuar desde T015"
```

### 4.7 /checklist - Validar Requisitos

```bash
# Crear checklist para dominio específico
/checklist security
/checklist ux
/checklist api

# Output: checklists/[domain].md
```

---

## 5. Guía de Skills

### 5.1 test_manager

**Funciones**:
1. `Generate Test Plan`: Crear plan de pruebas
2. `Run Tests`: Ejecutar tests
3. `Analyze Coverage`: Evaluar cobertura
4. `Document Results`: Generar reportes
5. `Validate Test Quality`: Verificar mejores prácticas
6. `Generate Fixtures`: Crear datos de prueba

**Uso**:
```
Agente, usa test_manager para generar el plan de pruebas
Agente, ejecuta los tests y genera el reporte
```

### 5.2 code_review

**Funciones**:
1. `Review Feature Changes`: Revisar todos los cambios
2. `Security Analysis`: Análisis OWASP
3. `Performance Analysis`: Detectar N+1, leaks
4. `Maintainability`: Code smells
5. `Architecture Check`: Validar estructura

### 5.3 deploy_manager

**Funciones**:
1. `Pre-Deploy Checklist`: Validaciones
2. `Execute Deploy`: Despliegue
3. `Post-Deploy Verification`: Smoke tests
4. `Rollback`: Reversión de emergencia
5. `Generate Release Notes`: Documentación

### 5.4 git_manager

**Funciones**:
1. Crear branches con numeración automática
2. Commits estandarizados
3. Push a remote

### 5.5 task_manager

**Funciones**:
1. Verificar prerrequisitos
2. Validar formato de tasks.md
3. Analizar consistencia

### 5.6 context7_manager

**Funciones**:
1. Mapear estado del sistema
2. Documentar infraestructura
3. Sincronizar documentación con código

---

## 6. Proceso de Testing

### 6.1 Filosofía de Testing

El framework sigue la filosofía de **Testing como Documentación**:

1. **Tests documentan comportamiento**: El nombre del test describe qué hace el sistema
2. **Tests validan requisitos**: Cada test se traza a un criterio de aceptación
3. **Tests son ciudadanos de primera clase**: Se planifican antes de implementar

### 6.2 Pirámide de Tests

```
        ╱╲
       ╱E2E╲         5% - Flujos críticos de usuario
      ╱──────╲
     ╱ Integr. ╲    25% - Interacción entre componentes
    ╱────────────╲
   ╱  Unit Tests  ╲ 70% - Lógica de negocio aislada
  ╱────────────────╲
```

### 6.3 Tipos de Tests

#### Unit Tests
- **Propósito**: Probar funciones/métodos aislados
- **Velocidad**: Milisegundos
- **Dependencias**: Mockeadas
- **Ubicación**: `tests/unit/`

```typescript
// tests/unit/services/auth.service.spec.ts
describe('AuthService', () => {
  describe('validatePassword', () => {
    it('should_return_true_when_password_matches_hash', () => {
      // Arrange
      const password = 'secure123';
      const hash = bcrypt.hashSync(password, 10);

      // Act
      const result = authService.validatePassword(password, hash);

      // Assert
      expect(result).toBe(true);
    });

    it('should_return_false_when_password_does_not_match', () => {
      // Arrange
      const password = 'wrong';
      const hash = bcrypt.hashSync('correct', 10);

      // Act
      const result = authService.validatePassword(password, hash);

      // Assert
      expect(result).toBe(false);
    });
  });
});
```

#### Integration Tests
- **Propósito**: Probar interacción entre componentes
- **Velocidad**: Segundos
- **Dependencias**: Reales (DB de test, etc.)
- **Ubicación**: `tests/integration/`

```typescript
// tests/integration/api/auth.spec.ts
describe('Auth API', () => {
  beforeAll(async () => {
    await testDb.connect();
    await testDb.seed(userFixtures);
  });

  afterAll(async () => {
    await testDb.cleanup();
  });

  describe('POST /api/auth/login', () => {
    it('should_return_token_when_credentials_valid', async () => {
      // Arrange
      const credentials = { email: 'test@example.com', password: 'password123' };

      // Act
      const response = await request(app)
        .post('/api/auth/login')
        .send(credentials);

      // Assert
      expect(response.status).toBe(200);
      expect(response.body).toHaveProperty('token');
      expect(response.body.token).toMatch(/^eyJ/); // JWT format
    });
  });
});
```

#### Contract Tests
- **Propósito**: Validar que API cumple especificación
- **Velocidad**: Segundos
- **Dependencias**: Esquema OpenAPI
- **Ubicación**: `tests/contract/`

```typescript
// tests/contract/api.spec.ts
describe('API Contract', () => {
  it('should_match_openapi_schema_for_user_endpoint', async () => {
    const response = await request(app).get('/api/users/1');

    const validation = validateAgainstSchema(
      response.body,
      openApiSpec.paths['/users/{id}'].get.responses['200'].content['application/json'].schema
    );

    expect(validation.valid).toBe(true);
  });
});
```

#### E2E Tests
- **Propósito**: Probar flujos completos de usuario
- **Velocidad**: Minutos
- **Dependencias**: Sistema completo
- **Ubicación**: `tests/e2e/`

```typescript
// tests/e2e/user-registration.spec.ts
describe('User Registration Flow', () => {
  it('should_complete_registration_and_login', async () => {
    // 1. Navigate to registration
    await page.goto('/register');

    // 2. Fill form
    await page.fill('[name="email"]', 'new@example.com');
    await page.fill('[name="password"]', 'SecurePass123!');
    await page.click('[type="submit"]');

    // 3. Verify redirect to dashboard
    await expect(page).toHaveURL('/dashboard');

    // 4. Verify welcome message
    await expect(page.locator('h1')).toContainText('Welcome');
  });
});
```

### 6.4 Flujo de Testing en el Desarrollo

```
┌─────────────────────────────────────────────────────────┐
│                 FLUJO DE TESTING                        │
└─────────────────────────────────────────────────────────┘

1. PLANIFICACIÓN (/test plan)
   │
   ├── Leer spec.md, plan.md, data-model.md
   ├── Crear matriz de cobertura
   ├── Definir casos de prueba
   └── Generar test-plan.md

2. PREPARACIÓN (Durante setup de /implement)
   │
   ├── Crear estructura de directorios de tests
   ├── Configurar framework de testing
   └── Generar fixtures base

3. DESARROLLO (TDD en /implement)
   │
   ├── Escribir test (debe FALLAR)
   ├── Implementar código mínimo
   ├── Test debe PASAR
   └── Refactorizar

4. EJECUCIÓN (/test run)
   │
   ├── Unit tests → Integration tests → E2E tests
   ├── Capturar métricas de cobertura
   └── Identificar tests fallidos

5. DOCUMENTACIÓN (/test report)
   │
   ├── Generar test-report.md
   ├── Actualizar checklist de testing
   └── Marcar tareas completadas

6. VALIDACIÓN (Gate de calidad)
   │
   ├── Verificar cobertura >= 80%
   ├── Verificar todos los tests P1 pasan
   └── Aprobar o bloquear merge
```

### 6.5 Criterios de Cobertura

| Categoría | Mínimo | Óptimo |
|-----------|--------|--------|
| Lógica de negocio | 90% | 95%+ |
| Servicios | 85% | 90%+ |
| Controllers | 80% | 85%+ |
| Utilidades | 70% | 80%+ |
| UI Components | 60% | 70%+ |

### 6.6 Comandos de Testing

```bash
# NestJS/Jest
npm test                    # Ejecutar todos los tests
npm run test:cov           # Con cobertura
npm run test:watch         # Watch mode
npm test -- --testPathPattern="auth"  # Tests específicos

# Angular
ng test                    # Unit tests
ng test --code-coverage   # Con cobertura
ng e2e                    # E2E tests

# Ver reporte de cobertura
open coverage/lcov-report/index.html
```

---

## 7. Mejores Prácticas

### 7.1 Naming de Tests

```typescript
// Formato: should_[behavior]_when_[condition]

// ✅ Bueno
it('should_return_user_when_id_exists')
it('should_throw_not_found_when_id_invalid')
it('should_hash_password_when_creating_user')

// ❌ Malo
it('test user')
it('login works')
it('should work correctly')
```

### 7.2 Patrón AAA

```typescript
it('should_calculate_total_with_discount', () => {
  // Arrange - Setup
  const items = [{ price: 100 }, { price: 50 }];
  const discount = 0.1;

  // Act - Execute
  const total = calculateTotal(items, discount);

  // Assert - Verify
  expect(total).toBe(135); // (100 + 50) * 0.9
});
```

### 7.3 Fixtures Consistentes

```typescript
// tests/fixtures/user.fixture.ts
export const fixtures = {
  validUser: {
    id: 'usr-001',
    email: 'test@example.com',
    name: 'Test User',
  },
  adminUser: {
    id: 'usr-admin',
    email: 'admin@example.com',
    role: 'admin',
  },
  invalidUser: {
    id: '',
    email: 'invalid',
  },
};

// Factory para datos dinámicos
export function createUser(overrides = {}) {
  return {
    id: `usr-${Date.now()}`,
    email: `user-${Date.now()}@test.com`,
    ...overrides,
  };
}
```

### 7.4 Mocking Efectivo

```typescript
// Mock solo dependencias externas
jest.mock('../services/email.service');

// No mockear la unidad bajo test
// ❌ jest.mock('./user.service'); // Si estás testeando UserService

// Usar dependency injection
class UserService {
  constructor(
    private emailService: EmailService, // Inyectado, fácil de mockear
  ) {}
}
```

### 7.5 Evitar Tests Flaky

```typescript
// ❌ Malo - Depende de tiempo real
it('should_expire_after_1_hour', async () => {
  await sleep(3600000); // NO!
  expect(token.isExpired()).toBe(true);
});

// ✅ Bueno - Usa time mocking
it('should_expire_after_1_hour', () => {
  jest.useFakeTimers();
  const token = createToken();

  jest.advanceTimersByTime(3600001);

  expect(token.isExpired()).toBe(true);
  jest.useRealTimers();
});
```

---

## 8. Referencia Rápida

### 8.1 Comandos Principales

| Comando | Descripción | Cuándo Usar |
|---------|-------------|-------------|
| `/specify` | Crear especificación | Inicio de nueva feature |
| `/plan` | Crear plan técnico | Después de especificar |
| `/tasks` | Generar lista de tareas | Después de planificar |
| `/test plan` | Crear plan de pruebas | Después de tareas |
| `/analyze` | Analizar consistencia | Antes de implementar |
| `/implement` | Ejecutar implementación | Después de análisis |
| `/test run` | Ejecutar tests | Después de implementar |
| `/test report` | Generar reporte de tests | Después de tests |
| `/checklist` | Crear checklist | Validación de calidad |

### 8.2 Skills Disponibles

| Skill | Propósito |
|-------|-----------|
| `test_manager` | Gestión completa de testing |
| `code_review` | Revisión de código |
| `deploy_manager` | Gestión de deployments |
| `git_manager` | Operaciones git |
| `task_manager` | Gestión de tareas |
| `plan_manager` | Gestión de planes |
| `context_manager` | Contexto de agente |
| `context7_manager` | Documentación Context7 |
| `terraform_gcp` | Infraestructura GCP |

### 8.3 Estructura de Feature

```
specs/N-feature-name/
├── spec.md          # Especificación (requisitos)
├── plan.md          # Plan técnico
├── tasks.md         # Lista de tareas
├── test-plan.md     # Plan de pruebas
├── test-report.md   # Resultados de tests
├── research.md      # Decisiones técnicas
├── data-model.md    # Modelo de datos
├── quickstart.md    # Escenarios de integración
├── contracts/       # Especificaciones API
│   └── endpoints.yaml
└── checklists/      # Checklists de calidad
    ├── testing.md
    ├── security.md
    └── ux.md
```

### 8.4 Checklist de Feature Completa

```markdown
## Pre-Implementación
- [ ] spec.md creado y sin [NEEDS CLARIFICATION]
- [ ] plan.md aprobado
- [ ] tasks.md generado
- [ ] test-plan.md creado
- [ ] /analyze sin issues críticos

## Implementación
- [ ] Todas las tareas de Setup completadas
- [ ] Todas las tareas Foundational completadas
- [ ] User Stories P1 implementadas
- [ ] Tests escritos y pasando

## Post-Implementación
- [ ] test-report.md generado
- [ ] Cobertura >= 80%
- [ ] Code review aprobado
- [ ] Checklist de testing completado

## Deployment
- [ ] Pre-deploy checks pasando
- [ ] Deploy a staging exitoso
- [ ] Smoke tests pasando
- [ ] Release notes generadas
```

---

## Apéndice A: Troubleshooting

### Tests Fallando

1. **Verificar fixtures**: ¿Los datos de prueba son válidos?
2. **Verificar mocks**: ¿Están configurados correctamente?
3. **Verificar dependencias**: ¿La DB de test está disponible?
4. **Verificar orden**: ¿Los tests dependen de orden de ejecución?

### Cobertura Baja

1. **Identificar archivos sin cobertura**: Revisar reporte de cobertura
2. **Priorizar lógica de negocio**: Enfocarse en servicios primero
3. **Revisar edge cases**: Agregar tests para casos límite

### Deploy Fallando

1. **Verificar pre-deploy**: ¿Todas las validaciones pasan?
2. **Verificar variables de entorno**: ¿Están configuradas?
3. **Verificar permisos**: ¿El service account tiene acceso?

---

**Versión del Manual**: 1.0.0
**Última Actualización**: 2024
**Mantenido por**: Antigravity Development Team

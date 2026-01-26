# Antigravity Development Flow

Sistema estructurado para desarrollo de software con enfoque TDD, testing obligatorio, y entrega incremental por user stories.

## 📋 Tabla de Contenidos

1. [Principios Core](#principios-core)
2. [Orden de Ejecución](#orden-de-ejecución)
3. [Estructura del Proyecto](#estructura-del-proyecto)
4. [Comandos Disponibles](#comandos-disponibles)
5. [Flujo de Trabajo](#flujo-de-trabajo)
6. [Pirámide de Testing](#pirámide-de-testing)
7. [Quality Gates](#quality-gates)

---

## 🎯 Principios Core

### Test-First Development (NON-NEGOTIABLE)

- **TDD es OBLIGATORIO** para todo código de producción
- Ciclo: Red → Green → Refactor
- Cobertura mínima: **80%**
- Sin excepciones

### Pirámide de Testing

```
        /\
       /E2E\        5-10%  (pocos, lentos, costosos)
      /____\
     /Integ.\      15-25% (moderados)
    /________\
   /Contract\      10-15% (API boundaries)
  /__________\  
 /   Unit     \    50-70% (muchos, rápidos, baratos)
/______________\
```

### Entrega Incremental

Cada User Story es:
- **Independiente**: Puede implementarse sola
- **Testeable**: Puede validarse de forma aislada
- **Valuable**: Entrega valor por sí misma (MVP)

---

## 🚀 Orden de Ejecución

### 1️⃣ Constitution (`memory/constitution.md`)

**Cuándo**: Una vez al inicio del proyecto, actualizar según sea necesario

**Propósito**: Definir los principios fundamentales del proyecto

**Acciones**:
- Revisar principios de testing obligatorios
- Confirmar métricas de cobertura (80% mínimo)
- Establecer quality gates
- Definir estándares de código

**Resultado**: Documento guía para todo el desarrollo

---

### 2️⃣ Spec (`/speckit.spec`)

**Cuándo**: Al inicio de cada nueva feature

**Propósito**: Definir QUÉ se va a construir desde la perspectiva del usuario

**Comando**: `/speckit.spec [descripción de la feature]`

**Template**: `templates/spec-template.md`

**Acciones**:
- Escribir User Stories priorizadas (P1, P2, P3...)
- Definir Acceptance Criteria (Given-When-Then)
- Especificar requisitos funcionales
- Identificar casos edge
- Definir criterios de éxito medibles

**Resultado**: `specs/[###-feature-name]/spec.md`

**Validación**:
- [ ] User stories son independientes y testeables
- [ ] Criterios de aceptación son claros
- [ ] Prioridades están asignadas

---

### 3️⃣ Plan (`/speckit.plan`)

**Cuándo**: Después de tener el spec aprobado

**Propósito**: Definir CÓMO se va a construir técnicamente

**Comando**: `/speckit.plan [###-feature-name]`

**Template**: `templates/plan-template.md`

**Acciones**:
- Definir stack tecnológico
- Diseñar arquitectura y estructura de archivos
- Validar contra Constitution (gates)
- Documentar decisiones técnicas
- Planificar testing strategy
- Configurar CI/CD

**Resultado**: 
- `specs/[###-feature-name]/plan.md`
- `specs/[###-feature-name]/research.md` (Phase 0)
- `specs/[###-feature-name]/data-model.md` (Phase 1)
- `specs/[###-feature-name]/quickstart.md` (Phase 1)
- `specs/[###-feature-name]/contracts/` (Phase 1)

**Validación**:
- [ ] Constitution check passed
- [ ] Estructura del proyecto definida
- [ ] Dependencias documentadas
- [ ] CI/CD configurado

---

### 4️⃣ Tasks (`/speckit.tasks`)

**Cuándo**: Después de tener el plan técnico completo

**Propósito**: Desglosar la implementación en tareas ejecutables

**Comando**: `/speckit.tasks [###-feature-name]`

**Template**: `templates/tasks-template.md`

**Acciones**:
- Crear tareas de setup (infraestructura)
- Crear tareas de test infrastructure
- Crear tareas foundational (bloqueantes)
- **Por cada User Story**:
  - Tareas de Unit Tests (PRIMERO)
  - Tareas de Contract Tests (PRIMERO)
  - Tareas de Integration Tests (PRIMERO)
  - Tareas de implementación (DESPUÉS de tests)
  - Tareas de verificación
- Identificar dependencias
- Marcar tareas paralelizables [P]

**Resultado**: `specs/[###-feature-name]/tasks.md`

**Validación**:
- [ ] Tests vienen ANTES de implementación
- [ ] Tareas tienen paths exactos de archivos
- [ ] Dependencias están claras
- [ ] Checkpoints definidos

---

### 5️⃣ Implementation (`/speckit.implement`)

**Cuándo**: Después de tener tasks.md completo

**Propósito**: Ejecutar el plan de implementación

**Comando**: `/speckit.implement [###-feature-name]`

**Workflow**: `.agent/workflows/implement.md`

**Fases**:

#### Phase 0: Pre-flight Check
- Verificar checklists (si existen)
- Cargar contexto (plan, spec, tasks, etc.)
- Verificar estructura del proyecto

#### Phase 1: Setup
- Crear estructura de directorios
- Inicializar proyecto con dependencias
- Configurar linting y formatting
- Setup test runner y coverage

#### Phase 1.5: Test Infrastructure
- Crear estructura de tests/
- Configurar conftest.py y fixtures
- Crear mocks reutilizables
- Documentar testing standards

#### Phase 2: Foundational
- Implementar infraestructura core
- Base de datos, autenticación, etc.
- **BLOQUEANTE**: User stories no pueden empezar sin esto

#### Phase 3+: User Stories (Incremental)

**Por cada User Story (en orden de prioridad)**:

```
1. Escribir Unit Tests → Verificar FAIL ❌
2. Escribir Contract Tests → Verificar FAIL ❌
3. Escribir Integration Tests → Verificar FAIL ❌
4. Implementar código
5. Verificar tests PASS ✅
6. Verificar coverage >= 85%
7. CHECKPOINT: Story completa e independiente
```

#### Phase N: Polish
- E2E tests (casos críticos)
- Optimización de performance
- Security hardening
- Documentación final
- Verificar coverage total >= 80%

**Validación en cada User Story**:
- [ ] Tests escritos primero ✅
- [ ] Tests fallaron inicialmente ❌
- [ ] Implementación completa
- [ ] Tests ahora pasan ✅
- [ ] Coverage >= 85%
- [ ] Sin regresión en stories anteriores

---

### 6️⃣ Checklist (`/speckit.checklist`)

**Cuándo**: Antes de hacer merge/deploy

**Propósito**: Verificación de calidad y completitud

**Comando**: `/speckit.checklist [tipo] [###-feature-name]`

**Template**: `templates/checklist-template.md`

**Tipos de Checklist**:
- `ux`: User experience y flujos de usuario
- `test`: Cobertura de testing
- `security`: Validaciones de seguridad
- `performance`: Métricas de rendimiento
- `docs`: Documentación completa

**Resultado**: `specs/[###-feature-name]/checklists/[tipo].md`

**Validación antes de merge**:
- [ ] Todos los checklists completos
- [ ] Todos los tests pasando
- [ ] Coverage >= 80%
- [ ] Code review aprobado
- [ ] CI/CD pipeline verde

---

## 📁 Estructura del Proyecto

### Documentation Structure

```
specs/
└── [###-feature-name]/
    ├── spec.md              # User stories y requirements
    ├── plan.md              # Plan técnico
    ├── research.md          # Investigación técnica
    ├── data-model.md        # Entidades y relaciones
    ├── quickstart.md        # Guía de inicio rápido
    ├── tasks.md             # Tareas de implementación
    ├── contracts/           # Contratos de API
    │   └── api_contract.json
    └── checklists/          # Checklists de verificación
        ├── test.md
        ├── ux.md
        └── security.md
```

### Code Structure (Single Project)

```
project-root/
├── src/
│   ├── models/          # Entidades y lógica de datos
│   ├── services/        # Lógica de negocio
│   ├── api/             # Endpoints y handlers
│   ├── cli/             # Interfaces CLI
│   └── utils/           # Utilidades y helpers
├── tests/
│   ├── conftest.py      # Fixtures compartidos
│   ├── fixtures/        # Datos de prueba
│   ├── mocks/           # Mocks reutilizables
│   ├── unit/            # Unit tests (50-70%)
│   │   ├── models/
│   │   ├── services/
│   │   └── utils/
│   ├── contract/        # Contract tests (10-15%)
│   ├── integration/     # Integration tests (15-25%)
│   └── e2e/             # E2E tests (5-10%)
├── docs/                # Documentación adicional
├── .github/
│   └── workflows/
│       └── ci.yml       # CI/CD pipeline
├── memory/
│   └── constitution.md  # Principios del proyecto
└── templates/           # Templates de flujo Antigravity
```

---

## 🛠️ Comandos Disponibles

### Comandos Speckit

```bash
# Crear nueva feature spec
/speckit.spec [descripción de la feature]

# Crear plan técnico
/speckit.plan [###-feature-name]

# Generar tasks
/speckit.tasks [###-feature-name]

# Ejecutar implementación
/speckit.implement [###-feature-name]

# Generar checklist
/speckit.checklist [tipo] [###-feature-name]
```

### Testing Commands (Ejemplo Python)

```bash
# Run all tests
pytest

# Run specific test type
pytest tests/unit/
pytest tests/integration/
pytest tests/contract/
pytest tests/e2e/

# Run with coverage
pytest --cov=src --cov-report=html --cov-report=term

# Run tests for specific user story
pytest -k "us1"

# Run and verify coverage threshold
pytest --cov=src --cov-fail-under=80
```

---

## 🔄 Flujo de Trabajo Completo

### Ejemplo: Nueva Feature "User Authentication"

```
1. 📝 Write Spec
   $ /speckit.spec Agregar autenticación de usuarios con JWT
   → specs/001-user-auth/spec.md
   
   Contenido:
   - US1 (P1): Usuario puede registrarse
   - US2 (P2): Usuario puede hacer login
   - US3 (P3): Usuario puede hacer logout

2. 🏗️ Create Plan
   $ /speckit.plan 001-user-auth
   → specs/001-user-auth/plan.md
   → specs/001-user-auth/data-model.md
   → specs/001-user-auth/contracts/
   
   Decisiones:
   - Stack: Python + FastAPI + JWT
   - Database: PostgreSQL
   - Tests: pytest + coverage

3. 📋 Generate Tasks
   $ /speckit.tasks 001-user-auth
   → specs/001-user-auth/tasks.md
   
   Output: 50 tareas organizadas por phase y user story
   - Setup: 6 tareas
   - Test Infrastructure: 7 tareas
   - Foundational: 6 tareas
   - US1 (Tests + Impl): 12 tareas
   - US2 (Tests + Impl): 10 tareas
   - US3 (Tests + Impl): 9 tareas

4. 💻 Implement
   $ /speckit.implement 001-user-auth
   
   Ejecución:
   a) Setup + Test Infrastructure → ✅
   b) Foundational → ✅
   c) US1 Tests → ❌ (FAIL como esperado)
   d) US1 Implementation → ✅ (Tests pasan)
   e) US1 Verification → Coverage 87% ✅
   f) CHECKPOINT: MVP listo (solo registro)
   g) US2 Tests → ❌ (FAIL)
   h) US2 Implementation → ✅
   i) US2 Verification → ✅
   j) US3 Tests → ❌ (FAIL)
   k) US3 Implementation → ✅
   l) US3 Verification → ✅
   m) Final Coverage: 83% ✅

5. ✅ Quality Check
   $ /speckit.checklist test 001-user-auth
   $ /speckit.checklist security 001-user-auth
   → Todos los items verificados
   
6. 🚀 Merge & Deploy
   - CI/CD pipeline: ✅ GREEN
   - Code review: ✅ Approved
   - Merge to main
   - Deploy to production
```

---

## 🧪 Pirámide de Testing

### Distribución Objetivo

| Tipo de Test | % Target | Propósito | Velocidad |
|--------------|----------|-----------|-----------|
| **Unit** | 50-70% | Funciones individuales | ⚡ Muy rápido |
| **Contract** | 10-15% | API schemas | ⚡ Rápido |
| **Integration** | 15-25% | Componentes juntos | 🐢 Medio |
| **E2E** | 5-10% | Flujos críticos completos | 🐌 Lento |

### Unit Tests

**Qué testear**:
- Cada función/método público
- Cada branch lógico (if/else/switch)
- Edge cases y boundary conditions
- Error handling

**Ejemplo**:
```python
def test_calculate_discount_with_valid_coupon_returns_reduced_price():
    # Arrange
    original_price = 100
    coupon = Coupon(code="SAVE10", discount_percent=10)
    
    # Act
    final_price = calculate_discount(original_price, coupon)
    
    # Assert
    assert final_price == 90
```

### Contract Tests

**Qué testear**:
- Request schemas (campos requeridos, tipos)
- Response schemas (estructura, tipos)
- Error responses (status codes, formato)

**Ejemplo**:
```python
def test_post_users_validates_required_fields():
    response = client.post("/users", json={})
    
    assert response.status_code == 422
    assert "email" in response.json()["errors"]
    assert "password" in response.json()["errors"]
```

### Integration Tests

**Qué testear**:
- User journeys completos
- Interacción entre capas
- Base de datos real (test DB)
- Dependencias reales o sandbox

**Ejemplo**:
```python
def test_us1_user_can_complete_registration():
    # User submits registration
    response = client.post("/auth/register", json={
        "email": "test@example.com",
        "password": "SecurePass123!"
    })
    assert response.status_code == 201
    
    # User exists in database
    user = db.query(User).filter_by(email="test@example.com").first()
    assert user is not None
    assert user.is_verified == False
```

### E2E Tests

**Qué testear**:
- Solo flujos críticos de negocio
- End-to-end real (UI → Backend → DB)
- Escenarios de producción

**Ejemplo**:
```python
def test_critical_payment_flow_completes_successfully():
    # Complete user journey from product selection to payment
    # Only for critical business flows
    pass
```

---

## 🚦 Quality Gates

### Pre-Commit

```yaml
# .pre-commit-config.yaml
- Lint check
- Format check
- Type check (si aplica)
```

### Pre-Push

```bash
- Unit tests
- Coverage check >= 80%
```

### Pull Request

```yaml
CI Pipeline:
  1. Lint & Format ⚡
  2. Unit Tests 🧪
  3. Contract Tests 📋
  4. Integration Tests 🔗
  5. Coverage Report 📊 (>= 80%)
  6. Security Scan 🔒
  7. Build ⚙️

Gates (bloquean merge):
  - All tests passing ✅
  - Coverage >= 80% ✅
  - No critical vulnerabilities ✅
  - Lint clean ✅
  - Code review approved ✅
```

### Pre-Release

```bash
- Full E2E test suite
- Performance benchmarks
- Security audit
- Documentation review
```

---

## 📊 Métricas de Éxito

### Coverage Targets

- **Overall**: >= 80%
- **Models**: >= 90%
- **Services**: >= 85%
- **Utils**: >= 95%
- **Controllers**: >= 75%

### Test Execution Time

- **Unit tests**: < 30 segundos total
- **Contract tests**: < 1 minuto total
- **Integration tests**: < 5 minutos total
- **E2E tests**: < 15 minutos total

### CI/CD Performance

- **Pipeline total**: < 10 minutos
- **Feedback time**: < 5 minutos (hasta integration tests)

---

## 🔧 Configuración Inicial

### 1. Clonar y Setup

```bash
# Clonar repositorio
git clone <repo-url>
cd <project>

# Revisar Constitution
cat memory/constitution.md

# Configurar entorno
# (depende del stack tecnológico definido en plan.md)
```

### 2. Instalar Dependencias

```bash
# Ejemplo Python
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
pip install -r requirements-dev.txt

# Ejemplo Node.js
npm install
npm install --save-dev (dev dependencies)
```

### 3. Configurar Tests

```bash
# Configurar coverage
# pytest.ini, jest.config.js, etc.

# Instalar pre-commit hooks
pre-commit install
```

### 4. Validar Setup

```bash
# Run linter
make lint

# Run tests
make test

# Check coverage
make coverage
```

---

## 📚 Templates Disponibles

| Template | Path | Propósito |
|----------|------|-----------|
| Constitution | `memory/constitution.md` | Principios del proyecto |
| Spec | `templates/spec-template.md` | User stories y requirements |
| Plan | `templates/plan-template.md` | Diseño técnico |
| Tasks | `templates/tasks-template.md` | Tareas de implementación |
| Checklist | `templates/checklist-template.md` | Verificación de calidad |
| Dev Guidelines | `templates/development-guidelines.md` | Guía de desarrollo |
| Testing Standards | `templates/testing-standards.md` | Estándares de testing |
| Code Review | `templates/code-review-checklist.md` | Checklist de code review |

---

## 🎓 Best Practices

### DO ✅

- Escribir tests ANTES de implementar (TDD)
- Mantener tests independientes y aislados
- Usar mocks para dependencias externas
- Seguir naming conventions de tests
- Verificar coverage después de cada user story
- Hacer commits atómicos por tarea
- Ejecutar tests antes de push
- Documentar decisiones técnicas
- Validar checkpoints de user stories

### DON'T ❌

- Implementar sin tests
- Hacer tests que dependen de otros tests
- Ignorar tests fallidos
- Bajar coverage sin justificación
- Hacer commits grandes con múltiples tareas
- Skip pre-commit hooks
- Merge sin code review
- Dejar TODO comments sin issue
- Romper user stories independientes

---

## 🆘 Troubleshooting

### Tests no pasan

```bash
# Ver detalles
pytest -vv

# Ver solo failed
pytest --lf

# Debug específico
pytest -k test_name -vv --pdb
```

### Coverage bajo

```bash
# Ver coverage detallado
pytest --cov=src --cov-report=html
open htmlcov/index.html

# Identificar líneas sin cobertura
pytest --cov=src --cov-report=term-missing
```

### CI/CD falla

```bash
# Reproducir CI localmente
make ci-test

# Ver logs de pipeline
# (depende de la plataforma)
```

---

## 📞 Support

- **Constitution**: `memory/constitution.md`
- **Testing Standards**: `templates/testing-standards.md`
- **Code Review Guidelines**: `templates/code-review-checklist.md`

---

**Version**: 1.0.0  
**Last Updated**: 2026-01-24  
**Maintained by**: Antigravity Team


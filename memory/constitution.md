# Antigravity Development Constitution

## Core Principles

### I. Test-First Development (NON-NEGOTIABLE)

**TDD es OBLIGATORIO** para todo código de producción:
- **Ciclo Red-Green-Refactor**: Tests escritos → Tests fallan (Red) → Implementación mínima (Green) → Refactorizar (Refactor)
- **Aprobación del usuario**: Los tests deben ser revisados y aprobados antes de implementar
- **Cobertura mínima obligatoria**: 80% general, con targets específicos por tipo de código
- **Sin excepciones**: No se permite código de producción sin tests correspondientes

### II. Pirámide de Testing

La distribución de tests debe seguir la pirámide:

```
        /\
       /E2E\        E2E Tests (5-10% - pocos, lentos, costosos)
      /____\
     /Integ.\      Integration Tests (15-25% - moderados)
    /________\
   /Contract\      Contract Tests (10-15% - API boundaries)
  /__________\  
 /   Unit     \    Unit Tests (50-70% - muchos, rápidos, baratos)
/______________\
```

**Ratios recomendados**:
- Unit Tests: 50-70%
- Contract Tests: 10-15%
- Integration Tests: 15-25%
- E2E Tests: 5-10%

### III. Unit Testing Standards

Cada función/método público DEBE cumplir:
- **Al menos un unit test** que valide el caso de uso principal
- **Cobertura de branches**: Cada rama lógica (if/else/switch) debe tener un test
- **Mocks para dependencias**: Usar mocks/stubs para dependencias externas
- **Tests aislados**: Cada test debe ser independiente y poder ejecutarse solo
- **Naming convention**: `test_<función>_<escenario>_<resultado_esperado>`

**Métricas de cobertura por tipo**:
- Models/Entities: 90%
- Services/Business Logic: 85%
- Utils/Helpers: 95%
- Controllers/Handlers: 75%
- **Overall Project: 80% mínimo**

### IV. Integration & Contract Testing

**Contract Tests** (Obligatorios para APIs):
- Validar contratos de API (request/response schemas)
- Ejecutarse antes de cada merge
- Detectar breaking changes en interfaces

**Integration Tests** (Obligatorios para flujos multi-capa):
- Probar interacción entre componentes reales
- Base de datos de prueba (no mocks)
- Servicios externos mockeados o en sandbox

**Áreas que requieren integration tests**:
- Nuevos endpoints de API
- Cambios en contratos existentes
- Comunicación entre servicios
- Flujos de autenticación/autorización
- Operaciones de base de datos complejas

### V. Code Quality & Observability

**Calidad**:
- Linting obligatorio antes de commit
- Code review requerido para todo PR
- Pre-commit hooks configurados
- Sin warnings de lint permitidos

**Observabilidad**:
- Logging estructurado en todos los servicios
- Errores logueados con contexto completo
- Métricas de performance en endpoints críticos
- Text I/O para debuggabilidad (stdin/stdout/stderr)

### VI. Versioning & Breaking Changes

**Formato de versión**: `MAJOR.MINOR.PATCH`
- **MAJOR**: Breaking changes (API incompatible)
- **MINOR**: Nuevas funcionalidades (backward compatible)
- **PATCH**: Bug fixes (backward compatible)

**Breaking changes requieren**:
- Documentación de migración
- Deprecation notices con al menos 1 versión de anticipación
- Tests de compatibilidad backward

### VII. Simplicity & YAGNI

**Principios**:
- Empezar simple, añadir complejidad solo cuando sea necesario
- YAGNI (You Aren't Gonna Need It): No implementar funcionalidad especulativa
- Preferir soluciones estándar sobre personalizadas
- Tres strikes rule: Abstraer solo después de 3 casos de uso similares

## Testing Infrastructure

### Estructura Obligatoria

```
tests/
├── conftest.py              # Fixtures compartidos (pytest)
├── fixtures/                # Datos de prueba
│   ├── users.json
│   └── sample_data.json
├── mocks/                   # Mocks reutilizables
│   ├── external_api.py
│   └── database.py
├── unit/                    # Unit tests (50-70%)
│   ├── models/
│   ├── services/
│   └── utils/
├── contract/                # Contract tests (10-15%)
│   └── test_api_contracts.py
├── integration/             # Integration tests (15-25%)
│   └── test_user_journeys.py
└── e2e/                     # End-to-End tests (5-10%)
    └── test_critical_paths.py
```

### Test Naming Conventions

**Unit Tests**:
```python
def test_<función>_<escenario>_<resultado>():
    # Ejemplo:
    def test_calculate_discount_with_valid_coupon_returns_reduced_price():
        pass
```

**Integration Tests**:
```python
def test_<user_story>_<flujo>():
    # Ejemplo:
    def test_us1_user_completes_checkout_successfully():
        pass
```

## CI/CD Requirements

### Pipeline Obligatorio

Todas las features DEBEN pasar por el pipeline CI/CD:

```
1. Lint & Format Check ⚡
2. Unit Tests 🧪
3. Contract Tests 📋
4. Integration Tests 🔗
5. Coverage Report 📊
6. Security Scan 🔒
7. Build ⚙️
8. E2E Tests (opcional, pre-merge) 🎯
```

### Quality Gates (Bloquean merge si fallan)

- [ ] Todos los tests pasando (100%)
- [ ] Coverage >= 80%
- [ ] Sin vulnerabilidades de seguridad críticas
- [ ] Lint clean (0 errores, 0 warnings)
- [ ] Code review aprobado (al menos 1 reviewer)
- [ ] Branch actualizada con main/master

### Archivos Requeridos

- `.github/workflows/ci.yml` (GitHub Actions) o equivalente
- `pytest.ini` o configuración de test runner
- `.pre-commit-config.yaml`
- `.gitignore` / `.dockerignore` según stack

## Development Workflow

### Orden de Ejecución

1. **Constitution** (este archivo)
   - Revisar principios al inicio del proyecto
   - Configurar una sola vez, actualizar cuando sea necesario

2. **Spec** (`/speckit.spec`)
   - Definir user stories con prioridades
   - Establecer acceptance criteria
   - Marcar tests requeridos

3. **Plan** (`/speckit.plan`)
   - Diseño técnico y arquitectura
   - Definir estructura del proyecto
   - Validar contra Constitution

4. **Tasks** (`/speckit.tasks`)
   - Desglose detallado de tareas
   - **Incluir SIEMPRE tareas de testing**
   - Marcar dependencias y paralelización

5. **Implementation** (`/speckit.implement`)
   - Seguir orden de tasks.md
   - TDD: Tests primero, implementación después
   - Commits atómicos por tarea

6. **Checklist** (`/speckit.checklist`)
   - Verificación de calidad antes de merge
   - Validar cobertura y quality gates

### Code Review Checklist

Antes de solicitar review:
- [ ] Todos los unit tests pasan localmente
- [ ] Coverage no ha disminuido
- [ ] Sin errores de lint/format
- [ ] Documentación actualizada
- [ ] PR description clara con contexto

Durante review:
- [ ] Lógica correcta y eficiente
- [ ] Edge cases cubiertos en tests
- [ ] Naming conventions seguidas
- [ ] Sin code smells evidentes
- [ ] Tests cubren casos importantes
- [ ] Consideraciones de seguridad validadas

## Complexity Tracking

**Cuando agregar complejidad requiere justificación**:
- Agregar 4to proyecto/módulo (sugerir consolidar primero)
- Introducir patrones arquitectónicos complejos (Repository, CQRS, Event Sourcing)
- Agregar dependencias nuevas (>3 dependencias por feature)
- Crear abstracciones profundas (>3 niveles de herencia/composición)

**Formato de justificación**:

| Complejidad | Por Qué es Necesaria | Alternativa Simple Rechazada Porque |
|-------------|----------------------|-------------------------------------|
| [Elemento] | [Necesidad específica] | [Por qué no funciona la opción simple] |

## Governance

### Autoridad

Esta Constitution **supersede** todas las otras prácticas de desarrollo:
- Si hay conflicto entre este documento y otra guía, este documento prevalece
- Excepciones requieren aprobación explícita y documentación
- Enmiendas requieren consenso del equipo y migración plan

### Cumplimiento

- **Todos los PRs** deben verificar cumplimiento con estos principios
- **Code reviews** deben validar adherencia a standards de testing
- **Retrospectivas** deben evaluar si los principios están siendo efectivos
- **Métricas** de cobertura y calidad se revisan semanalmente

### Enmiendas

Modificar esta Constitution requiere:
1. Propuesta documentada con justificación
2. Revisión en equipo
3. Periodo de comentarios (1 semana)
4. Aprobación por mayoría
5. Plan de migración si hay impacto en código existente
6. Actualización de versión y fecha

### Herramientas de Soporte

- **Development Guidelines** (`templates/development-guidelines.md`): Guía viva con decisiones técnicas del proyecto actual
- **Testing Standards** (`templates/testing-standards.md`): Ejemplos concretos y mejores prácticas de testing
- **Code Review Checklist** (`templates/code-review-checklist.md`): Checklist detallada para reviewers

---

**Version**: 2.0.0 | **Ratified**: 2026-01-24 | **Last Amended**: 2026-01-24

**Changelog**:
- 2.0.0 (2026-01-24): Constitution inicial poblada con principios Antigravity
  - Agregada pirámide de testing obligatoria
  - Definidos estándares de unit tests
  - Integrado CI/CD como requerimiento
  - Establecidas métricas de cobertura
  - Documentado workflow completo

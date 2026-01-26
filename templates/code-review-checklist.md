# Code Review Checklist

**Purpose**: Checklist obligatoria para code reviewers antes de aprobar un Pull Request

**Feature**: [Link to spec.md or feature documentation]  
**PR**: [Link to Pull Request]  
**Author**: [Developer name]  
**Reviewer**: [Your name]  
**Date**: [Review date]

---

## Pre-Review (Author Checklist)

**Author must verify BEFORE requesting review**:

- [ ] **Todos los tests pasan localmente**
  - [ ] Unit tests: `[command] tests/unit/`
  - [ ] Contract tests: `[command] tests/contract/`
  - [ ] Integration tests: `[command] tests/integration/`

- [ ] **Coverage no ha disminuido**
  - Current coverage: __%
  - Previous coverage: __%
  - [ ] Coverage >= 80% overall

- [ ] **Sin errores de lint/format**
  - [ ] Linter: `[lint command]`
  - [ ] Formatter: `[format command]`

- [ ] **PR description completa**
  - [ ] Qué cambia y por qué
  - [ ] Link a spec/tasks
  - [ ] Screenshots (si aplica UI)
  - [ ] Breaking changes documentados (si aplica)

- [ ] **Documentación actualizada**
  - [ ] README actualizado (si aplica)
  - [ ] Docstrings/JSDoc actualizados
  - [ ] Comentarios en código complejo

- [ ] **Branch actualizada**
  - [ ] `git pull origin main` ejecutado
  - [ ] Sin conflictos

---

## Review Process

### 1. Context & Understanding ✅

**Objetivo**: Entender QUÉ se está cambiando y POR QUÉ

- [ ] **Lei la feature spec** (`specs/[###-feature]/spec.md`)
  - User story(s) afectada(s): _______________
  - Prioridad: P___

- [ ] **Lei la PR description completa**
  - Entiendo el problema que resuelve
  - Entiendo el approach técnico

- [ ] **Revisé las tareas relacionadas** (`specs/[###-feature]/tasks.md`)
  - Tareas completadas: T___, T___, T___
  - Tareas pendientes (si aplica): T___, T___

- [ ] **Checkeé el diff general**
  - Archivos modificados: ___ archivos
  - Líneas agregadas: ___ lines
  - Líneas eliminadas: ___ lines
  - [ ] El scope parece razonable (no too large)

---

### 2. Testing Review 🧪

**Objetivo**: Verificar que el código está adecuadamente testeado

#### Test Coverage

- [ ] **Tests unitarios existen**
  - [ ] Para cada función/método público nuevo
  - [ ] Para cada branch lógico (if/else/switch)
  - [ ] Para edge cases y boundary conditions
  - [ ] Para error handling

- [ ] **Tests de contrato existen** (si aplica API)
  - [ ] Request schema validation
  - [ ] Response schema validation
  - [ ] Error response validation

- [ ] **Tests de integración existen** (si aplica)
  - [ ] User journey completo testeado
  - [ ] Interacción entre componentes validada

- [ ] **Coverage metrics**
  - [ ] Overall coverage >= 80%
  - [ ] Nuevas líneas cubiertas >= 85%
  - [ ] Sin archivos con coverage < 70%

#### Test Quality

- [ ] **Tests son claros y legibles**
  - [ ] Nombres descriptivos (`test_<función>_<escenario>_<resultado>`)
  - [ ] Patrón Arrange-Act-Assert seguido
  - [ ] Sin lógica compleja en tests

- [ ] **Tests son independientes**
  - [ ] No dependen de otros tests
  - [ ] Pueden ejecutarse en cualquier orden
  - [ ] Limpian después de si mismos

- [ ] **Mocks y fixtures apropiados**
  - [ ] Dependencias externas mockeadas
  - [ ] Fixtures reutilizables en conftest.py
  - [ ] No mocks innecesarios (preferir lo real cuando sea simple)

- [ ] **Tests fallan cuando deben fallar**
  - [ ] Tests detectarían regresiones
  - [ ] Assertions son significativas (no solo `assert True`)

**Coverage Report Link**: [Link to coverage report if available]

---

### 3. Code Quality Review 💎

**Objetivo**: Verificar que el código es maintainable, readable, y sigue standards

#### Correctness

- [ ] **Lógica es correcta**
  - [ ] Implementa correctamente la spec
  - [ ] Edge cases manejados
  - [ ] No hay off-by-one errors
  - [ ] No hay race conditions o thread safety issues

- [ ] **Error handling apropiado**
  - [ ] Errores son capturados y manejados
  - [ ] Mensajes de error son claros
  - [ ] Errores son logueados con contexto suficiente
  - [ ] No se ignoran excepciones silenciosamente

- [ ] **Validación de inputs**
  - [ ] User inputs validados
  - [ ] Tipos verificados (si lenguaje dinámico)
  - [ ] Boundary values validados

#### Readability

- [ ] **Naming conventions seguidas**
  - [ ] Variables: snake_case / camelCase según standard del proyecto
  - [ ] Functions/methods: verbos descriptivos
  - [ ] Classes: sustantivos, PascalCase
  - [ ] Constants: UPPER_CASE

- [ ] **Código es self-documenting**
  - [ ] Nombres expresan intención
  - [ ] Funciones son cortas (<50 lines idealmente)
  - [ ] Un nivel de abstracción por función

- [ ] **Comentarios apropiados**
  - [ ] Comentarios explican "por qué", no "qué"
  - [ ] Código complejo tiene explicación
  - [ ] Sin comentarios obsoletos o misleading
  - [ ] Sin código comentado (usar git history)

#### Design

- [ ] **Diseño es apropiado**
  - [ ] Sin over-engineering (YAGNI)
  - [ ] Sin under-engineering (missing abstractions)
  - [ ] Separation of concerns respetada
  - [ ] DRY principle seguido (no duplicación innecesaria)

- [ ] **Estructuras de datos apropiadas**
  - [ ] Usa el data structure correcto para el problema
  - [ ] Performance acceptable para el use case

- [ ] **Patrones de diseño** (si aplican)
  - [ ] Patrones justificados (no pattern for pattern sake)
  - [ ] Implementados correctamente
  - [ ] Documentados si son no-obvios

#### Code Smells

**Verificar que NO existen**:

- [ ] ❌ Funciones muy largas (>100 lines)
- [ ] ❌ Clases muy grandes (>500 lines)
- [ ] ❌ Parámetros excesivos (>5 params)
- [ ] ❌ Nested conditionals profundos (>3 levels)
- [ ] ❌ Duplicación de código
- [ ] ❌ Magic numbers sin explicación
- [ ] ❌ Dead code
- [ ] ❌ God objects (clase que hace todo)
- [ ] ❌ Tight coupling

---

### 4. Security Review 🔒

**Objetivo**: Identificar vulnerabilidades de seguridad

- [ ] **Input validation**
  - [ ] User inputs sanitizados
  - [ ] SQL injection prevenido (usar prepared statements)
  - [ ] XSS prevenido (escape outputs)
  - [ ] Path traversal prevenido

- [ ] **Authentication & Authorization**
  - [ ] Endpoints protegidos apropiadamente
  - [ ] Permisos verificados antes de acciones
  - [ ] No hay bypass de autenticación

- [ ] **Sensitive data**
  - [ ] No hay passwords/tokens/keys en código
  - [ ] Datos sensibles encriptados (si aplica)
  - [ ] Logs no exponen datos sensibles

- [ ] **Dependencies**
  - [ ] No se agregan dependencias con vulnerabilidades conocidas
  - [ ] Dependencias están justificadas

- [ ] **APIs y endpoints**
  - [ ] Rate limiting considerado (si aplica)
  - [ ] CORS configurado correctamente
  - [ ] HTTPS enforced (si aplica)

**Security Notes**: [Any security concerns or validations needed]

---

### 5. Performance Review ⚡

**Objetivo**: Identificar problemas de performance obvios

- [ ] **Algoritmos eficientes**
  - [ ] Complejidad algorítmica apropiada (O(n) vs O(n²))
  - [ ] Sin loops innecesarios
  - [ ] Sin cálculos redundantes

- [ ] **Database queries** (si aplica)
  - [ ] Queries optimizadas
  - [ ] Índices apropiados (si se crean tablas)
  - [ ] Sin N+1 queries
  - [ ] Pagination para grandes datasets

- [ ] **Resource usage**
  - [ ] Sin memory leaks obvios
  - [ ] Files/connections cerrados apropiadamente
  - [ ] Sin carga innecesaria de datos grandes

- [ ] **Caching** (si aplica)
  - [ ] Caching implementado donde tiene sentido
  - [ ] Invalidación de cache considerada

**Performance Notes**: [Any performance concerns or benchmarks needed]

---

### 6. Documentation Review 📚

**Objetivo**: Asegurar que el código está documentado apropiadamente

- [ ] **Docstrings/JSDoc**
  - [ ] Funciones públicas documentadas
  - [ ] Parámetros descritos
  - [ ] Return values descritos
  - [ ] Excepciones documentadas

- [ ] **README updates** (si aplica)
  - [ ] Nuevas features documentadas
  - [ ] Setup instructions actualizadas
  - [ ] Ejemplos de uso agregados

- [ ] **API documentation** (si aplica)
  - [ ] Endpoints documentados (OpenAPI/Swagger)
  - [ ] Request/response examples
  - [ ] Error codes documentados

- [ ] **Inline comments**
  - [ ] Decisiones complejas explicadas
  - [ ] Workarounds documentados con razón
  - [ ] TODO/FIXME tienen issues asociados

---

### 7. Testing Execution ✅

**Objetivo**: Ejecutar tests y verificar que funcionan

- [ ] **Pull branch locally**
  ```bash
  git fetch origin
  git checkout [branch-name]
  ```

- [ ] **Install dependencies** (si hay cambios)
  ```bash
  [install command]
  ```

- [ ] **Run all tests**
  ```bash
  [test command]
  ```
  - [ ] All tests pass ✅
  - [ ] No flaky tests

- [ ] **Check coverage**
  ```bash
  [coverage command]
  ```
  - [ ] Coverage >= 80% ✅
  - [ ] No significant drop

- [ ] **Run linter**
  ```bash
  [lint command]
  ```
  - [ ] No errors ✅
  - [ ] No warnings ✅

- [ ] **Manual testing** (si aplica UI)
  - [ ] Feature funciona como esperado
  - [ ] UI es usable y responsive
  - [ ] No errores en console

**Test Results**: [Paste relevant test output or link to CI run]

---

### 8. Integration & Compatibility 🔗

**Objetivo**: Verificar que los cambios no rompen nada existente

- [ ] **Backward compatibility**
  - [ ] APIs existentes no se rompen
  - [ ] Breaking changes están documentados
  - [ ] Migration path existe (si hay breaking change)

- [ ] **No regresiones**
  - [ ] Tests de features existentes siguen pasando
  - [ ] Coverage no bajó en código existente

- [ ] **Dependencies compatibility**
  - [ ] Nuevas dependencias compatibles con existentes
  - [ ] Version conflicts resueltos

- [ ] **User Stories independientes** (si aplica)
  - [ ] User story implementada puede testearse independientemente
  - [ ] No rompe otras user stories ya implementadas

---

### 9. Constitution Compliance ⚖️

**Objetivo**: Verificar adherencia a principios del proyecto

Revisar contra `memory/constitution.md`:

- [ ] **Test-First seguido**
  - [ ] Tests fueron escritos antes de implementación (verificar git history)
  - [ ] Tests fallaron primero, luego pasaron

- [ ] **Coverage targets alcanzados**
  - [ ] Models: >= 90%
  - [ ] Services: >= 85%
  - [ ] Utils: >= 95%
  - [ ] Overall: >= 80%

- [ ] **Code quality standards**
  - [ ] Lint clean
  - [ ] Format consistente
  - [ ] Naming conventions seguidas

- [ ] **Logging y observability**
  - [ ] Operaciones importantes logueadas
  - [ ] Errores logueados con contexto
  - [ ] Structured logging usado

---

## Decision

### Approve ✅

**Requirements**:
- [ ] All checklist items passed or have acceptable justification
- [ ] Tests pass locally
- [ ] Coverage >= 80%
- [ ] No security concerns
- [ ] Code quality is good
- [ ] Documentation is adequate

**Comment**:
```
✅ LGTM (Looks Good To Me)

[Positive feedback about the PR]

Approved with minor suggestions:
- [Optional: list minor non-blocking suggestions]
```

---

### Request Changes ⚠️

**Use when**:
- Critical issues found
- Tests not adequate
- Security vulnerabilities
- Breaking changes not documented

**Comment Template**:
```
⚠️ Requesting changes

**Blocking issues**:
1. [Issue 1 with location]
2. [Issue 2 with location]

**Must fix before approval**:
- [ ] Fix issue 1
- [ ] Add test for scenario X
- [ ] Update documentation

**Suggestions (non-blocking)**:
- Consider refactoring X for clarity
- Could extract method Y
```

---

### Comment (Non-blocking) 💬

**Use when**:
- Minor suggestions
- Questions for clarification
- Learning opportunity

**Comment Template**:
```
💬 Some thoughts and questions

**Questions**:
- Why did you choose approach X over Y?
- Is this pattern documented somewhere?

**Suggestions (optional)**:
- Consider using X pattern here for [benefit]
- Variable name could be more descriptive

Overall looks good! 👍
```

---

## Post-Approval

After approval, author should:

- [ ] Address non-blocking comments (optional but appreciated)
- [ ] Squash commits if needed
- [ ] Update branch with main one last time
- [ ] Merge when CI is green
- [ ] Delete feature branch
- [ ] Move tasks to "Done" in tasks.md
- [ ] Update relevant documentation

---

## Review Time Tracking

- Time to first response: ___ hours
- Total review time: ___ hours
- Rounds of review: ___

**Target**: First response < 24 hours, total < 48 hours

---

## Notes

[Any additional notes, concerns, or context for future reference]

---

**Template Version**: 1.0.0  
**Last Updated**: 2026-01-24


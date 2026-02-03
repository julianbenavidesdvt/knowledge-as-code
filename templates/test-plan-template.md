---
description: "Test plan template for feature testing documentation"
---

# Test Plan: [FEATURE NAME]

## Metadata

| Campo | Valor |
|-------|-------|
| **Feature** | [N-feature-name] |
| **Fecha de Creación** | [YYYY-MM-DD] |
| **Última Actualización** | [YYYY-MM-DD] |
| **Estado** | Draft / In Review / Approved |
| **Autor** | [Nombre o AI Agent] |

---

## 1. Alcance del Testing

### En Alcance

Lista de componentes, módulos y funcionalidades que serán probados:

- [ ] [Componente/Módulo 1]: Descripción breve
- [ ] [Componente/Módulo 2]: Descripción breve
- [ ] [Componente/Módulo 3]: Descripción breve

### Fuera de Alcance

Componentes excluidos y justificación:

| Componente | Razón de Exclusión |
|------------|-------------------|
| [Componente] | Ya cubierto en otro feature |
| [Componente] | Código legacy sin cambios |

---

## 2. Estrategia de Testing

### Pirámide de Tests

```
        ╱╲
       ╱E2E╲        ← Pocos, lentos, alto valor
      ╱──────╲
     ╱ Integr. ╲    ← Moderados, validan interacciones
    ╱────────────╲
   ╱  Unit Tests  ╲ ← Muchos, rápidos, aislados
  ╱────────────────╲
```

### Distribución Objetivo

| Tipo | % del Total | Tiempo Ejecución |
|------|-------------|------------------|
| Unit | 70% | < 30 seg |
| Integration | 25% | < 2 min |
| E2E | 5% | < 5 min |

---

## 3. Matriz de Cobertura (User Story → Tests)

### User Story 1: [Título] (Prioridad: P1)

**Criterios de Aceptación → Tests**:

| ID | Criterio de Aceptación | Tipo Test | ID Test | Estado |
|----|------------------------|-----------|---------|--------|
| AC-1.1 | [Criterio del spec.md] | Unit | UT-001 | ⬜ Pending |
| AC-1.2 | [Criterio del spec.md] | Integration | IT-001 | ⬜ Pending |
| AC-1.3 | [Criterio del spec.md] | E2E | E2E-001 | ⬜ Pending |

### User Story 2: [Título] (Prioridad: P2)

| ID | Criterio de Aceptación | Tipo Test | ID Test | Estado |
|----|------------------------|-----------|---------|--------|
| AC-2.1 | [Criterio del spec.md] | Unit | UT-010 | ⬜ Pending |
| AC-2.2 | [Criterio del spec.md] | Integration | IT-010 | ⬜ Pending |

### User Story 3: [Título] (Prioridad: P3)

| ID | Criterio de Aceptación | Tipo Test | ID Test | Estado |
|----|------------------------|-----------|---------|--------|
| AC-3.1 | [Criterio del spec.md] | Unit | UT-020 | ⬜ Pending |

---

## 4. Casos de Prueba Detallados

### 4.1 Unit Tests

#### UT-001: [Nombre Descriptivo]

- **Componente**: `src/services/[service].ts`
- **User Story**: US1
- **Prioridad**: Alta

| Escenario | Input | Expected Output | Estado |
|-----------|-------|-----------------|--------|
| Happy Path | Datos válidos | Resultado esperado | ⬜ |
| Edge Case: Empty | `null` / `[]` | Error controlado | ⬜ |
| Edge Case: Max | Valor máximo | Comportamiento límite | ⬜ |
| Error Case | Datos inválidos | Excepción específica | ⬜ |

**Código de Test** (ejemplo):
```typescript
describe('ServiceName', () => {
  describe('methodName', () => {
    it('should_return_result_when_valid_input', () => {
      // Arrange
      const input = { /* valid data */ };

      // Act
      const result = service.methodName(input);

      // Assert
      expect(result).toEqual(expectedOutput);
    });

    it('should_throw_error_when_invalid_input', () => {
      // Arrange
      const input = null;

      // Act & Assert
      expect(() => service.methodName(input))
        .toThrow(InvalidInputError);
    });
  });
});
```

#### UT-002: [Nombre Descriptivo]

[Misma estructura...]

---

### 4.2 Integration Tests

#### IT-001: [Nombre del Flujo]

- **Componentes Involucrados**: Service A → Repository → Database
- **User Story**: US1
- **Prioridad**: Alta

**Setup Requerido**:
```typescript
beforeEach(async () => {
  // Configurar base de datos de prueba
  await testDb.seed(fixtures.users);
});

afterEach(async () => {
  // Limpiar datos
  await testDb.cleanup();
});
```

| Escenario | Flujo | Verificación | Estado |
|-----------|-------|--------------|--------|
| Create and Read | POST → GET | Datos persistidos correctamente | ⬜ |
| Update | PUT → GET | Datos actualizados | ⬜ |
| Delete | DELETE → GET | Recurso no encontrado | ⬜ |

---

### 4.3 Contract Tests

> Solo si existe `contracts/` con especificaciones OpenAPI

#### CT-001: [Endpoint Name]

- **Endpoint**: `POST /api/v1/[resource]`
- **Contract**: `contracts/endpoints.yaml`

| Escenario | Request | Expected Response | Status |
|-----------|---------|-------------------|--------|
| Valid Request | Schema compliant | 200/201 + valid body | ⬜ |
| Invalid Request | Missing required field | 400 + error details | ⬜ |
| Unauthorized | No token | 401 | ⬜ |

---

### 4.4 E2E Tests

> Solo para flujos críticos de User Stories P1

#### E2E-001: [Nombre del Flujo de Usuario]

- **User Story**: US1
- **Precondiciones**: Usuario registrado, sistema disponible

**Pasos**:
1. Usuario navega a [página]
2. Usuario ingresa [datos]
3. Usuario hace clic en [botón]
4. Sistema muestra [resultado]

**Verificaciones**:
- [ ] Elemento X visible
- [ ] Datos Y correctos
- [ ] Navegación a Z exitosa

---

## 5. Datos de Prueba (Fixtures)

### 5.1 Fixtures de Entidades

Basado en `data-model.md`:

#### User Fixture
```typescript
// tests/fixtures/user.fixture.ts
export const validUser = {
  id: 'usr-001',
  email: 'test@example.com',
  name: 'Test User',
  role: 'user',
  createdAt: new Date('2024-01-01'),
};

export const adminUser = {
  ...validUser,
  id: 'usr-admin',
  email: 'admin@example.com',
  role: 'admin',
};

export const invalidUser = {
  id: '',
  email: 'not-an-email',
  name: '',
};
```

#### [Entity] Fixture
```typescript
// tests/fixtures/[entity].fixture.ts
export const valid[Entity] = {
  // campos según data-model.md
};
```

### 5.2 Factory Functions

```typescript
// tests/fixtures/factories.ts
export function createUser(overrides?: Partial<User>): User {
  return {
    id: `usr-${Date.now()}`,
    email: `user-${Date.now()}@test.com`,
    name: 'Generated User',
    role: 'user',
    createdAt: new Date(),
    ...overrides,
  };
}
```

---

## 6. Configuración de Entorno

### 6.1 Variables de Entorno para Tests

```env
# .env.test
NODE_ENV=test
DATABASE_URL=postgres://localhost:5432/test_db
JWT_SECRET=test-secret-key
LOG_LEVEL=error
```

### 6.2 Dependencias de Testing

```json
{
  "devDependencies": {
    "jest": "^29.x",
    "@types/jest": "^29.x",
    "ts-jest": "^29.x",
    "supertest": "^6.x",
    "@types/supertest": "^2.x"
  }
}
```

### 6.3 Configuración de Jest

```javascript
// jest.config.js
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  coverageDirectory: 'coverage',
  collectCoverageFrom: [
    'src/**/*.ts',
    '!src/**/*.d.ts',
    '!src/main.ts',
  ],
  coverageThreshold: {
    global: {
      branches: 75,
      functions: 80,
      lines: 80,
      statements: 80,
    },
  },
};
```

---

## 7. Criterios de Éxito

### 7.1 Criterios de Aceptación del Testing

| Criterio | Valor Mínimo | Objetivo |
|----------|--------------|----------|
| Tests P1 pasando | 100% | 100% |
| Tests P2 pasando | 95% | 100% |
| Cobertura de líneas | 80% | 90% |
| Cobertura de ramas | 75% | 85% |
| Tests flaky | 0 | 0 |
| Tiempo total ejecución | < 5 min | < 3 min |

### 7.2 Definition of Done para Testing

- [ ] Todos los tests del plan implementados
- [ ] Cobertura cumple mínimos establecidos
- [ ] No hay tests ignorados sin justificación
- [ ] test-report.md generado
- [ ] Checklist de testing completado

---

## 8. Riesgos y Mitigaciones

| Riesgo | Probabilidad | Impacto | Mitigación |
|--------|--------------|---------|------------|
| Base de datos de prueba no disponible | Media | Alto | Usar SQLite in-memory como fallback |
| Tests E2E lentos | Alta | Medio | Limitar a flujos críticos P1 |
| Dependencias externas inestables | Media | Alto | Mocks comprehensivos |

---

## 9. Cronograma de Ejecución

| Fase | Tests | Duración Estimada |
|------|-------|-------------------|
| Unit Tests | UT-001 a UT-XXX | [Tiempo] |
| Integration Tests | IT-001 a IT-XXX | [Tiempo] |
| Contract Tests | CT-001 a CT-XXX | [Tiempo] |
| E2E Tests | E2E-001 a E2E-XXX | [Tiempo] |
| **Total** | | [Tiempo Total] |

---

## 10. Aprobaciones

| Rol | Nombre | Fecha | Firma |
|-----|--------|-------|-------|
| QA Lead | | | ⬜ Pending |
| Tech Lead | | | ⬜ Pending |
| Product Owner | | | ⬜ Pending |

---

## Notas

- Este plan debe actualizarse si cambian los requisitos en spec.md
- Los tests E2E son opcionales para features con prioridad P3
- La cobertura objetivo puede ajustarse según constitution.md

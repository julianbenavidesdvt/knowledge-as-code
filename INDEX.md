# Índice de Archivos - Antigravity Development Flow

**Última actualización**: 24 de Enero, 2026

---

## 📖 Documentos Principales

### 🎯 Empezar Aquí

| Archivo | Propósito | Cuándo Leer |
|---------|-----------|-------------|
| **[README.md](README.md)** | Guía completa del flujo Antigravity | Primera vez, referencia constante |
| **[RESUMEN_IMPLEMENTACION.md](RESUMEN_IMPLEMENTACION.md)** | Resumen de mejoras implementadas | Para entender qué cambió |
| **[mejoras.md](mejoras.md)** | Análisis detallado de problemas y soluciones | Para contexto técnico |

---

## 📋 Constitution & Principles

| Archivo | Propósito | Cuándo Usar |
|---------|-----------|-------------|
| **[memory/constitution.md](memory/constitution.md)** | Principios fundamentales del proyecto | Setup inicial, resolución de conflictos |

**Contenido clave**:
- Test-First Development (OBLIGATORIO)
- Pirámide de Testing (ratios)
- Unit Testing Standards
- Coverage targets (80% mínimo)
- CI/CD Requirements
- Quality Gates

---

## 🛠️ Templates de Workflow

### Templates Principales

| Archivo | Comando | Cuándo Usar |
|---------|---------|-------------|
| **[templates/spec-template.md](templates/spec-template.md)** | `/speckit.spec` | Al definir una nueva feature |
| **[templates/plan-template.md](templates/plan-template.md)** | `/speckit.plan` | Para diseño técnico |
| **[templates/tasks-template.md](templates/tasks-template.md)** | `/speckit.tasks` | Para desglose de implementación |
| **[templates/checklist-template.md](templates/checklist-template.md)** | `/speckit.checklist` | Antes de merge/deploy |

### Templates de Soporte

| Archivo | Propósito | Cuándo Usar |
|---------|-----------|-------------|
| **[templates/code-review-checklist.md](templates/code-review-checklist.md)** | Guía para code reviewers | Durante code review |
| **[templates/testing-standards.md](templates/testing-standards.md)** | Estándares y ejemplos de testing | Al escribir tests |
| **[templates/development-guidelines.md](templates/development-guidelines.md)** | Decisiones técnicas del proyecto | Living document del proyecto |

---

## 🔄 Orden de Uso en el Flujo

```
1. README.md
   └─> Entender el flujo completo

2. memory/constitution.md
   └─> Revisar principios

3. templates/spec-template.md
   └─> Definir user stories (Comando: /speckit.spec)

4. templates/plan-template.md
   └─> Diseño técnico + CI/CD (Comando: /speckit.plan)

5. templates/tasks-template.md
   └─> Desglosar tareas con TDD (Comando: /speckit.tasks)

6. templates/testing-standards.md
   └─> Referencia al escribir tests

7. .agent/workflows/implement.md
   └─> Ejecutar implementación (Comando: /speckit.implement)

8. templates/code-review-checklist.md
   └─> Hacer code review

9. templates/checklist-template.md
   └─> Verificación final (Comando: /speckit.checklist)
```

---

## 📚 Guías de Referencia

### Para Developers

**Setup inicial**:
1. Leer [README.md](README.md) - Sección "Configuración Inicial"
2. Leer [memory/constitution.md](memory/constitution.md) - Entender principios
3. Revisar [templates/testing-standards.md](templates/testing-standards.md) - Aprender estándares

**Durante desarrollo**:
- [templates/testing-standards.md](templates/testing-standards.md) - Cómo escribir tests
- [templates/development-guidelines.md](templates/development-guidelines.md) - Patrones del proyecto
- [README.md](README.md) - Comandos y troubleshooting

**Antes de PR**:
- [templates/code-review-checklist.md](templates/code-review-checklist.md) - Auto-review

### Para Reviewers

1. [templates/code-review-checklist.md](templates/code-review-checklist.md) - Checklist completa
2. [memory/constitution.md](memory/constitution.md) - Verificar compliance
3. [templates/testing-standards.md](templates/testing-standards.md) - Validar tests

### Para Project Managers

1. [README.md](README.md) - Visión general del flujo
2. [RESUMEN_IMPLEMENTACION.md](RESUMEN_IMPLEMENTACION.md) - Qué cambió
3. [mejoras.md](mejoras.md) - Análisis de mejoras

---

## 🎓 Recursos por Tópico

### Testing

| Tópico | Archivo | Sección |
|--------|---------|---------|
| Pirámide de Testing | [README.md](README.md) | "Pirámide de Testing" |
| Unit Tests | [templates/testing-standards.md](templates/testing-standards.md) | "Unit Tests" |
| Contract Tests | [templates/testing-standards.md](templates/testing-standards.md) | "Contract Tests" |
| Integration Tests | [templates/testing-standards.md](templates/testing-standards.md) | "Integration Tests" |
| E2E Tests | [templates/testing-standards.md](templates/testing-standards.md) | "E2E Tests" |
| Mocks & Fixtures | [templates/testing-standards.md](templates/testing-standards.md) | "Mocks y Fixtures" |
| Coverage | [memory/constitution.md](memory/constitution.md) | "Coverage targets" |

### CI/CD

| Tópico | Archivo | Sección |
|--------|---------|---------|
| Pipeline Architecture | [templates/plan-template.md](templates/plan-template.md) | "CI/CD Configuration" |
| Quality Gates | [README.md](README.md) | "Quality Gates" |
| Pre-commit Hooks | [templates/plan-template.md](templates/plan-template.md) | "Pre-Commit Hooks" |

### Workflow

| Tópico | Archivo | Sección |
|--------|---------|---------|
| Orden de Ejecución | [README.md](README.md) | "Orden de Ejecución" |
| TDD Process | [templates/tasks-template.md](templates/tasks-template.md) | Estructura de fases |
| User Stories | [templates/spec-template.md](templates/spec-template.md) | Todo el documento |
| Code Review | [templates/code-review-checklist.md](templates/code-review-checklist.md) | Todo el documento |

---

## 🔍 Búsqueda Rápida

### "Necesito saber cómo..."

| Pregunta | Respuesta en... |
|----------|-----------------|
| ¿Cuál es el orden del flujo? | [README.md](README.md) - "Orden de Ejecución" |
| ¿Cómo escribir unit tests? | [templates/testing-standards.md](templates/testing-standards.md) - "Unit Tests" |
| ¿Cuál es la cobertura mínima? | [memory/constitution.md](memory/constitution.md) - 80% overall |
| ¿Cómo hacer code review? | [templates/code-review-checklist.md](templates/code-review-checklist.md) |
| ¿Qué comandos usar? | [README.md](README.md) - "Comandos Disponibles" |
| ¿Cómo configurar CI/CD? | [templates/plan-template.md](templates/plan-template.md) - "CI/CD Configuration" |
| ¿Cómo nombrar tests? | [templates/testing-standards.md](templates/testing-standards.md) - "Naming Conventions" |
| ¿Qué es TDD en este flujo? | [memory/constitution.md](memory/constitution.md) - "Test-First Development" |

### "Estoy en la fase de... ¿qué necesito?"

| Fase | Documento Necesario |
|------|---------------------|
| Definir feature | [templates/spec-template.md](templates/spec-template.md) |
| Diseño técnico | [templates/plan-template.md](templates/plan-template.md) |
| Crear tareas | [templates/tasks-template.md](templates/tasks-template.md) |
| Escribir tests | [templates/testing-standards.md](templates/testing-standards.md) |
| Implementar | [README.md](README.md) + [templates/tasks-template.md](templates/tasks-template.md) |
| Code review | [templates/code-review-checklist.md](templates/code-review-checklist.md) |
| Pre-merge | [templates/checklist-template.md](templates/checklist-template.md) |

---

## 📊 Mapa de Dependencias

```
README.md (punto de entrada)
    │
    ├─> memory/constitution.md (principios)
    │
    ├─> templates/spec-template.md
    │   └─> templates/plan-template.md
    │       └─> templates/tasks-template.md
    │           ├─> templates/testing-standards.md (referencia)
    │           └─> templates/development-guidelines.md (referencia)
    │               └─> templates/code-review-checklist.md
    │                   └─> templates/checklist-template.md
    │
    └─> RESUMEN_IMPLEMENTACION.md (contexto)
        └─> mejoras.md (análisis detallado)
```

---

## 🆕 Cambios Recientes

### 2026-01-24: Implementación Completa

**Archivos Nuevos**:
- ✅ README.md
- ✅ templates/code-review-checklist.md
- ✅ templates/testing-standards.md
- ✅ templates/development-guidelines.md (renombrado desde agent-file-template.md)
- ✅ RESUMEN_IMPLEMENTACION.md
- ✅ INDEX.md (este archivo)

**Archivos Modificados**:
- ✅ memory/constitution.md (poblada completamente)
- ✅ templates/tasks-template.md (unit tests integrados)
- ✅ templates/plan-template.md (CI/CD section agregada)
- ✅ mejoras.md (análisis completo)

**Archivos Eliminados**:
- ❌ templates/agent-file-template.md (reemplazado)

---

## 💡 Tips de Navegación

1. **Nuevo en el proyecto**: Empieza por [README.md](README.md)
2. **Necesitas ejemplo concreto**: Ve a [templates/testing-standards.md](templates/testing-standards.md)
3. **Tienes duda sobre proceso**: Busca en [README.md](README.md) o [memory/constitution.md](memory/constitution.md)
4. **Vas a hacer code review**: Abre [templates/code-review-checklist.md](templates/code-review-checklist.md)
5. **Necesitas reference rápida**: Este archivo (INDEX.md)

---

## 📞 Soporte

**Si no encuentras lo que buscas**:
1. Usa Ctrl+F en este índice
2. Revisa la sección "Búsqueda Rápida" arriba
3. Consulta el [README.md](README.md)
4. Lee [RESUMEN_IMPLEMENTACION.md](RESUMEN_IMPLEMENTACION.md) para contexto

---

**Versión**: 1.0.0  
**Última actualización**: 24 de Enero, 2026  
**Mantenido por**: Antigravity Team


---
name: task_manager
description: Skill para gestionar la preparación, validación y análisis de tareas de implementación.
---

# Gestor de Tareas de Implementación

Esta skill reemplaza scripts como `check-prerequisites` y ayuda en el proceso de generación de `tasks.md`, verificando el entorno y validando los artefactos generados.

## Instrucciones

### 1. Verificar Prerrequisitos (Check Prerequisites)

Utiliza esta función al inicio del flujo de trabajo de tareas (`tasks.md`) para identificar si existen los documentos de diseño necesarios antes de generar tareas.

1.  **Identificar Contexto de Rama**:
    - Obtén la rama actual: `git branch --show-current`.
    - Verifica que sea una rama de funcionalidad (formato `N-nombre`).
    - Define `FEATURE_DIR` como `specs/[RAMA]/`.

2.  **Buscar Documentos de Diseño**:
    Verifica la existencia de los siguientes archivos dentro de `FEATURE_DIR`:
    - `plan.md` o `plan_es.md` (Design Plan)
    - `spec.md` o `spec_es.md` (Feature Spec)
    - `tasks.md` o `tasks_es.md` (Implementation Tasks) - *Requerido para Implementación*
    - `data-model.md` (Opcional)
    - `research.md` (Opcional)
    - `quickstart.md` (Opcional)
    - `contracts/` (Directorio Opcional)

3.  **Reportar Estado**:
    Genera un resumen claro para el contexto del agente:
    - **FEATURE_DIR**: `[Ruta Absoluta]`
    - **Documentos Encontrados**: Lista de archivos existentes.
    - **Documentos Faltantes**: Lista de archivos críticos faltantes.
    
    *Nota: Para flujos de generación, Plan y Spec son obligatorios. Para flujos de implementación, tasks.md TAMBIÉN es obligatorio.*
    
    *Si faltan documentos obligatorios para el flujo actual, detén el proceso y notifica al usuario.*

### 2. Validar Formato de Tareas (Validate Tasks)

Utiliza esta función después de generar el archivo `tasks.md` para asegurar que cumple estrictamente con el formato requerido para su ejecución automatizada.

1.  **Leer Archivo de Tareas**:
    Lee el contenido completo del archivo `tasks.md` generado.

2.  **Verificar Reglas de Formato**:
    - **Checkboxes**: Cada línea de tarea debe comenzar con `- [ ]`.
    - **IDs de Tarea**: Debe incluir un ID secuencial `Txxx` (ej. `T001`).
    - **Etiquetas de Historia**: Las tareas pertenecientes a historias de usuario deben tener la etiqueta `[USx]` (ej. `[US1]`).
      - *Excepción*: Tareas de Fase 1 (Setup) y Fase 2 (Foundational) pueden no tenerla.
    - **Rutas de Archivo**: La descripción debe incluir una ruta de archivo explícita o indicar claramente la acción de configuración.

3.  **Reportar Errores de Validación**:
    Si encuentras errores, lista las líneas específicas y qué regla violan.
    *Ejemplo: "Línea 45: Falta ID de tarea (Txxx)"*

### 3. Analizar Consistencia (Opcional)

Si se solicita, cruza la información entre `tasks.md` y `spec.md` para asegurar que todas las historias de usuario tienen tareas asociadas.

---
name: context_manager
description: Skill para actualizar los archivos de contexto del agente (CLAUDE.md, etc.) basándose en el plan de implementación.
---

# Gestor de Contexto del Agente

Esta skill reemplaza la funcionalidad del script `update-agent-context.sh`. Su objetivo es mantener actualizados los archivos de instrucciones del agente (como `CLAUDE.md`, `GEMINI.md`, `.cursor/rules/*.mdc`) con la información tecnológica decidida en el `plan.md`.

## Instrucciones

### 1. Analizar el Plan de Implementación

1.  **Leer archivo de plan**:
    Lee el contenido de `plan.md` (o `plan_es.md`) en la carpeta de la característica actual.

2.  **Extraer Datos Clave**:
    Busca las siguientes secciones o líneas (acepta español e inglés):
    *   **Lenguaje/Versión** (`Language/Version` / `Lenguaje/Versión`)
    *   **Dependencias Principales** (`Primary Dependencies` / `Dependencias Principales`)
    *   **Almacenamiento/Base de Datos** (`Storage` / `Almacenamiento`)
    *   **Tipo de Proyecto** (`Project Type` / `Tipo de Proyecto`)

    *Ignora valores como "N/A" o "NEEDS CLARIFICATION".*

### 2. Determinar Archivos de Agente a Actualizar

Identifica qué archivos de contexto existen en la raíz del repositorio o sus configuraciones:

- `CLAUDE.md` (Claude)
- `GEMINI.md` (Gemini)
- `.github/agents/copilot-instructions.md` (Copilot)
- `.cursor/rules/specify-rules.mdc` (Cursor)
- `.windsurf/rules/specify-rules.md` (Windsurf)
- Otros definidos en el proyecto (ej. `AGENTS.md`)

*Si no existe ninguno, crea `CLAUDE.md` por defecto.*

### 3. Actualizar Contenido del Archivo de Agente

Para cada archivo de agente encontrado:

1.  **Tecnologías Activas (Active Technologies)**:
    - Busca la sección `## Active Technologies`. Si no existe, créala.
    - Agrega una línea con el formato:
      `- [Lenguaje] + [Framework] ([Nombre-Rama])`
      *Ejemplo: `- Python + FastAPI (5-auth-usuario)`*
    - Evita duplicados exactos.

2.  **Cambios Recientes (Recent Changes)**:
    - Busca la sección `## Recent Changes`. Si no existe, créala.
    - Inserta al inicio de la lista la nueva funcionalidad:
      `- [Nombre-Rama]: Added [Tecnologías]`
      *Ejemplo: `- 5-auth-usuario: Added Python + FastAPI`*
    - Mantén solo los últimos 3-5 cambios para no saturar el contexto.

3.  **Comandos de Construcción/Test**:
    - Si el archivo tiene una sección de comandos, asegúrate de que reflejen el lenguaje/framework seleccionado (ej. `npm test`, `pytest`, `cargo test`).

### 4. Guardar Cambios

Escribe los cambios en el archivo correspondiente.

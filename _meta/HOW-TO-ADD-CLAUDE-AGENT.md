# Cómo agregar un subagent nuevo (Claude Code)

Los subagents son agentes especializados que trabajan en contextos independientes. Son equivalentes a los agents de OpenCode.

---

## Cuándo crear un subagent nuevo

Crea un subagent cuando:
- Hay una nueva área funcional del negocio
- Necesitas un agente especializado con herramientas específicas
- Quieres aislar contexto de operaciones voluminosas
- El agente requiere permisos diferentes a los demás

Ejemplos:
- `marketing` — Especialista en contenido y campañas
- `curator` — Único con permisos para actualizar docs permanentes
- `db-reader` — Solo queries read-only a base de datos

---

## Paso 1: Crear el archivo .md

```
.claude/agents/[nombre].md
```

### Estructura base

```markdown
---
name: [nombre-subagent]
description: |
  [Descripción detallada de qué hace y cuándo usarlo.
  Claude usa esta descripción para decidir cuándo delegar tareas a este subagent.
  Sé específico sobre el dominio y responsabilidades.]
tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
model: sonnet
permissionMode: default
skills:
  - [skills a pre-cargar, si aplica]
---

Sos el agente responsable de [área/función].

## Antes de cualquier tarea

1. Lee `knowledge/empresa.md` para entender el negocio
2. Lee `areas/[area]/overview.md` para entender el área
3. [Otros docs relevantes]

## Tu área de conocimiento

[Dónde está la documentación relevante]

## Qué podés hacer

[Lista de capacidades específicas]

## Proceso de trabajo

1. Lee el contexto necesario antes de ejecutar
2. Si falta información crítica, pedila antes de inventar
3. [Otros pasos del proceso]

## Output

- Guarda outputs en `_inbox/YYYY-MM-DD-[area]-descripcion.md`
- [Otras convenciones de output]

## Límites

- No modifiques archivos fuera de `_inbox/` (salvo curator)
- No tomes decisiones fuera del alcance
- Si la tarea involucra otra área, deriva
```

---

## Paso 2: Configurar permisos y tools

### Tools disponibles

- `Read` — Leer archivos
- `Write` — Crear archivos nuevos
- `Edit` — Modificar archivos existentes
- `Bash` — Ejecutar comandos
- `Glob` — Buscar archivos por patrón
- `Grep` — Buscar contenido en archivos
- `Task` — Lanzar otros subagents (solo si necesario)

### Permission modes

- `default` — Pide confirmación para operaciones sensibles
- `acceptEdits` — Auto-acepta ediciones de archivos
- `dontAsk` — Auto-deniega prompts de permisos
- `bypassPermissions` — Salta todos los checks (usar con cuidado)

---

## Paso 3: Activación

Los subagents se cargan automáticamente al iniciar Claude Code desde el directorio del proyecto.

Si creaste el subagent con Claude Code corriendo:
```
/agents
```

Selecciona "Reload agents" o reinicia la sesión.

---

## Buenas prácticas

- **Description detallada:** Claude usa `description` para decidir cuándo delegar. Sé específico.
- **Tools mínimos:** Solo agrega los tools que realmente necesita el subagent.
- **Pre-cargar skills:** Usa `skills: [brand-voice]` para contexto automático.
- **Memoria persistente:** Usa `memory: project` si el subagent debe recordar entre sesiones.
- **Temperatura:** Usa `model: haiku` para tareas simples/rápidas, `sonnet` para análisis, `opus` para creatividad.

---

## Diferencias con OpenCode

| OpenCode | Claude Code |
|----------|-------------|
| `@agente` | Delegación automática |
| `temperature: 0.4` | `model: sonnet` |
| `permission.edit: ask` | `permissionMode: default` |
| No soporta memoria | `memory: user/project/local` |

Ambos usan YAML + Markdown y comparten la base de conocimiento.

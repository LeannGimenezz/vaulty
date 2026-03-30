# Claude Code Subagents — Spine Agents

Sistema de subagents de Claude Code para Spine Agents.

## Estructura

```
.claude/
├── agents/           # Subagents especializados (equivalente a .opencode/agents/)
│   ├── marketing.md
│   ├── operaciones.md
│   ├── finanzas.md
│   ├── producto.md
│   ├── rrhh.md
│   ├── legal.md
│   └── curator.md
├── skills/           # Skills de contexto (equivalente a .opencode/skills/)
│   ├── brand-voice.md
│   ├── product-knowledge.md
│   ├── audience-context.md
│   └── campaign-context.md
└── README.md         # Este archivo
```

## Subagents vs Skills

- **Subagents** (`.claude/agents/`): Agentes especializados con contextos independientes, equivalentes a los agents de OpenCode
- **Skills** (`.claude/skills/`): Módulos de contexto reutilizables que se cargan en subagents

## Formato de Subagent

```markdown
---
name: [nombre]
description: [descripción detallada - Claude usa esto para decidir cuándo delegar]
tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
model: sonnet|opus|haiku|inherit
permissionMode: default|acceptEdits|dontAsk|bypassPermissions
skills:
  - [skills a pre-cargar]
---

[System prompt del subagent en Markdown]
```

## Diferencias con OpenCode

| Aspecto | OpenCode | Claude Code |
|---------|----------|-------------|
| Ubicación | `.opencode/agents/` | `.claude/agents/` |
| Formato | YAML + Markdown | YAML + Markdown |
| Invocación | `@agente` | Delegación automática |
| Permisos | `permission:` + `tools:` | `permissionMode:` + `tools:` |
| Memoria | No soportado | `memory:` user/project/local |

Ambos sistemas comparten la misma base de conocimiento (`knowledge/`, `areas/`).

## Cómo Agregar un Subagent Nuevo

1. Crea `.claude/agents/[nombre].md` con YAML + Markdown
2. Define `description` clara (Claude usa esto para decidir cuándo usarlo)
3. Especifica `tools` necesarios
4. Escribe system prompt con instrucciones específicas
5. Actualiza `CLAUDE.md` con el nuevo subagent (opcional, solo para documentación)
6. Reinicia Claude Code o usa `/agents` para cargar

Ver subagents existentes como ejemplos.

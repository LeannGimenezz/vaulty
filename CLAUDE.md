# Spine Agents — Claude Code Router

Sistema de subagents de Claude Code para el vault de conocimiento de [Nombre Empresa].

## Contexto

Este vault contiene la base de conocimiento del equipo organizada por áreas. Los subagents de Claude Code son agentes especializados que trabajan en contextos independientes.

## Base de Conocimiento

### Antes de cualquier tarea
Lee `knowledge/empresa.md` para entender el negocio y `knowledge/equipo.md` para entender la estructura del equipo.

### Antes de crear contenido
Los subagents de área tienen preload el skill `brand-voice` cuando corresponde.

### Antes de ejecutar tareas por área
Cada área tiene su carpeta en `areas/[area]/` con un `MOC.md` que indica qué documentos leer.

---

## Subagents Disponibles

Claude delega automáticamente a estos subagents según la descripción de la tarea.

| Subagent | Cuándo usarlo |
|----------|---------------|
| `marketing` | Contenido, campañas, copy, estrategia de marketing |
| `operaciones` | Procesos, flujos operativos, coordinación |
| `finanzas` | Análisis financiero, reportes, métricas |
| `producto` | Features, roadmap, documentación de producto |
| `rrhh` | Personas, cultura, procesos de equipo |
| `legal` | Contratos, compliance, documentación legal |
| `curator` | Mantener la base de conocimiento actualizada |

---

## Skills Compartidos

Skills que los subagents pueden cargar para contexto adicional:

| Skill | Qué provee |
|-------|------------|
| `brand-voice` | Voz y tono de marca |
| `product-knowledge` | Contexto del producto/servicio |
| `audience-context` | Audiencias específicas |
| `campaign-context` | Campañas activas |

Los subagents de área tienen `brand-voice` pre-cargado cuando aplica.

---

## Diferencias con OpenCode

Si este vault también usa OpenCode:
- **OpenCode**: Agentes con `@agente` (CLI de OpenCode)
- **Claude Code**: Subagents (delegación automática o manual)
- **Compatibilidad**: Ambos usan la misma base de conocimiento (`knowledge/`, `areas/`)

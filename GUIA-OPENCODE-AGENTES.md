# Guía Completa: Sistema de Agentes con OpenCode + Obsidian

> **Versión:** 1.0  
> **Última actualización:** 30/03/2026  
> **Enfoque:** Para personas sin experiencia técnica que quieren implementar un sistema de agentes de IA en su negocio.

---

## Tabla de Contenidos

1. [¿Qué es esto y por qué lo necesitas?](#1-qué-es-esto-y-por-qué-lo-necesitas)
2. [Conceptos básicos que tenés que conocer](#2-conceptos-básicos-que-tenés-que-conocer)
3. [Paso 1: Instalar las herramientas](#3-paso-1--instalar-las-herramientas)
4. [Paso 2: Crear tu vault](#4-paso-2--crear-tu-vault)
5. [Paso 3: Estructura del sistema de agentes](#5-paso-3--estructura-del-sistema-de-agentes)
6. [Paso 4: Configurar la base de conocimiento](#6-paso-4--configurar-la-base-de-conocimiento)
7. [Paso 5: Crear tus primeros agentes](#7-paso-5--crear-tus-primeros-agentes)
8. [Paso 6: Crear skills (opcional pero recomendado)](#8-paso-6--crear-skills-opcional-pero-recomendado)
9. [Paso 7: Configurar el router principal (AGENTS.md)](#9-paso-7--configurar-el-router-principal-agentsmd)
10. [Paso 8: Primeros pasos con OpenCode](#10-paso-8--primeros-pasos-con-opencode)
11. [Ejemplos completos](#11-ejemplos-completos)
12. [Mejores prácticas](#12-mejores-prácticas)
13. [FAQ - Preguntas frecuentes](#13-faq--preguntas-frecuentes)

---

## 1. ¿Qué es esto y por qué lo necesitas?

### El problema

Cuando manejás un negocio (o trabajás en uno), seguro notaste que:
- Hay tareas que se repiten todo el tiempo y te roban energía
- La información está en la cabeza de una sola persona y si se va, se pierde
- Entre tu equipo y vos se generan muchos mensajes, mails y caos
- Contratar gente es caro y el onboarding es lento

### La solución

Un **sistema de agentes de IA** es como tener un equipo virtual que:
- Conoce TU negocio, porque tiene toda la info documentada
- Trabaja 24/7 sin cansarse
-Hace las tareas repetitivas por vos
- Entrega resultados consistentes

### ¿Qué es OpenCode + Obsidian?

| Herramienta | ¿Qué hace? | ¿Por qué las dos? |
|-------------|-------------|--------------------|
| **Obsidian** | Guardás toda la información de tu negocio en archivos de texto (tu "base de conocimiento") | Es donde viv la "memoria" del sistema |
| **OpenCode** | Conecta agentes de IA a esa base de conocimiento | Los agentes leen tus docs y ejecutan tareas |

**En resumen:** Obsidian es la biblioteca, OpenCode es el bibliotecario. Sin biblioteca, el bibliotecario no tiene de dónde sacar información.

---

## 2. Conceptos básicos que tenés que conocer

Antes de empezar, aclaremos algunos términos que vas a ver repetidamente:

### 2.1 ¿Qué es un VAULT?

Un **vault** (bóveda, en inglés) es simplemente una **carpeta en tu computadora** que contiene:
- Tu base de conocimiento (en Obsidian)
- Los archivos de configuración de OpenCode
- Los agentes y skills

**Analogía:** Es como el directorio raíz de un proyecto. Todo está adentro.

**Ubicación ejemplo:**
```
C:\Users\TuNombre\Desktop\mi-negocio-vault\
```

### 2.2 ¿Qué es un AGENTE?

Un **agente** es un asistente virtual especializado en algo. 

**Ejemplos:**
- `@comercial` → sabe de ventas, prospección, estrategias
- `@soporte` → sabe de atención al cliente, helpdesk
- `@marketing` → sabe de contenido, campañas, redes

**Cada agente tiene:**
- Un **nombre** (para invocarlo con `@nombre`)
- Una **descripción** (para que OpenCode sepa cuándo invocarlo)
- Un **rol** (qué sabe hacer, qué no)
- **Reglas** (límites, dónde guarda outputs)

### 2.3 ¿Qué es una SKILL?

Una **skill** es un conjunto de instrucciones que se carga cuando un agente necesita contexto específico.

**Ejemplo:**
- `empresa-context` → Carga info general de la empresa
- `comercial-context` → Carga info específica del área comercial

**Analogía:** Es como un "modo" que prendés antes de una tarea. Agente entra al modo, lee los archivos relevantes, y después ejecuta.

### 2.4 ¿Qué es el ROUTER (AGENTS.md)?

Es el archivo que define **quién hace qué**. Es como la recepcionista de una empresa:

> **Vos:** "Necesito algo de marketing"  
> **Recepcionista (AGENTS.md):** "Ah, eso lo maneja @marketing. Te paso."

---

## 3. Paso 1 — Instalar las herramientas

### 3.1 Instalar Obsidian

**Obsidian** es gratis para uso personal. Es una app para tomar notas con links entre archivos.

**Descarga:**
1. Ir a [https://obsidian.md](https://obsidian.md)
2. Click en **Download**
3. Elegí tu sistema operativo (Windows/Mac/Linux)
4. Installá la aplicación

**Primer uso:**
1. Abrí Obsidian
2. Te va a preguntar algo como "Create new vault" o "Open existing vault"
3. Elegí **Create new vault**
4. Nombre: `mi-negocio-vault` (o el nombre que quieras)
5. Elegí dónde guardarlo (recomiendo el Escritorio o una carpeta fácil)

**Plugins recomendados (opcional pero vale):**
1. Click en el engranaje (Settings) → Community plugins
2. Buscá e instalá:
   - **Templater** → Para crear archivos desde plantillas
   - **Obsidian Git** → Para hacer backup automático a GitHub
   - **Dataview** → Para hacer consultas sobre tus notas

### 3.2 Instalar OpenCode

**OpenCode** es la herramienta que conecta los agentes de IA con tu vault.

**Descarga:**
1. Ir a [https://opencode.ai](https://opencode.ai)
2. Click en **Download** o **Get Started**
3. Elegí tu sistema operativo

**En Windows:**
- Descargá el `.exe`
- Ejecutalo y seguí los pasos del instalador

**En Mac:**
- Descargá el `.dmg`
- Arrastrá el ícono a Applications

**En Linux:**
```bash
# Opción 1: npm (si tenés Node.js)
npm install -g opencode

# Opción 2: binary
# Descargá el binary de releases y hacelo ejecutable
chmod +x opencode
sudo mv opencode /usr/local/bin/
```

### 3.3 Configurar OpenCode por primera vez

1. Abrí una terminal (en Windows: `Win + R`, escribí `cmd`, enter)
2. Verificá que está instalado:
   ```bash
   opencode --version
   ```
   (Si tira error, reinstalá o buscá ayuda)

3. Necesitás configurar un **AI Provider** (el servicio de IA que va a usar):
   
   **Opción A: Anthropic (Claude)** — RECOMENDADA
   ```bash
   opencode config set provider anthropic
   opencode config set anthropic_api_key TU_API_KEY
   ```
   
   **Opción B: OpenAI (GPT)**
   ```bash
   opencode config set provider openai
   opencode config set openai_api_key TU_API_KEY
   ```

   > **¿Dónde consigo una API Key?**
   > - Anthropic: [console.anthropic.com](https://console.anthropic.com)
   > - OpenAI: [platform.openai.com](https://platform.openai.com)
   > (Necesitás crear una cuenta y puede requerir tarjeta de crédito, pero viene con crédito gratis)

---

## 4. Paso 2 — Crear tu vault

Ahora vamos a armar la estructura de carpetas que necesita el sistema.

### 4.1 Estructura de carpetas

Creá las siguientes carpetas dentro de tu vault (podés usar el Explorador de Windows o desde Obsidian):

```
mi-negocio-vault/
├── .opencode/              ← Acá van los agentes y skills
│   ├── agents/            ← Los agentes
│   └── skills/             ← Las skills
├── areas/                 ← Documentación por área de tu negocio
│   ├── comercial/
│   ├── marketing/
│   ├── operaciones/
│   ├── finanzas/
│   └── [otras áreas]
├── knowledge/              ← Conocimiento general
├── templates/              ← Plantillas reutilizables
├── _inbox/                 ← Outputs de los agentes
├── _meta/                  ← Documentación del sistema
└── AGENTS.md               ← Router de agentes (CLAVE)
```

### 4.2 Cómo crear las carpetas (paso a paso)

**Desde Obsidian:**
1. En el panel izquierdo, click en el ícono de carpeta
2. Click derecho → New folder
3. Creá cada una

**Desde Windows Explorer:**
1. Abrí la carpeta del vault
2. Click derecho → New → Folder
3. Nombrá cada una

**Desde terminal:**
```bash
cd ruta/a/tu/vault
mkdir .opencode areas knowledge templates _inbox _meta
mkdir .opencode/agents .opencode/skills
mkdir areas/comercial areas/marketing areas/operaciones areas/finanzas
```

### 4.3 Archivo README.md (opcional pero recomendado)

Creá un archivo `README.md` en la raíz del vault (desde Obsidian: Click derecho → New note):

```markdown
# Mi Negocio — Sistema de Agentes

Sistema operativo de equipos basado en agentes de IA y documentación viva.

## Estructura

- `.opencode/` — Agentes y skills
- `areas/` — Documentación por área
- `knowledge/` — Base de conocimiento compartida
- `templates/` — Plantillas
- `_inbox/` — Outputs de agentes

## Cómo usar

1. Abrí terminal en esta carpeta
2. Ejecutá `opencode`
3. Escribí tu pedido

## Ver también

- [[AGENTS]] — Router de agentes
- [[_meta/GUIA]] — Guía completa del sistema
```

---

## 5. Paso 3 — Estructura del sistema de agentes

### 5.1 La carpeta `.opencode/`

Esta carpeta es el "cerebro" del sistema. Contiene:

| Carpeta | Contenido | Ejemplo |
|---------|-----------|---------|
| `.opencode/agents/` | Los archivos de cada agente | `comercial.md`, `soporte.md` |
| `.opencode/skills/` | Las skills (subcarpetas) | `empresa-context/SKILL.md` |

**Importante:** La carpeta `.opencode/` tiene un punto adelante, lo que significa que está "oculta" en algunos sistemas. Si no la ves en Obsidian, asegurate de que "Show hidden files" esté activado.

### 5.2 Archivo AGENTS.md (el router)

Este archivo es **LA CLAVE** de todo el sistema. Tiene que estar en la **raíz del vault**.

¿Qué hace? Define:
- Cuáles agentes existen
- Qué hace cada uno
- Cuándo usar cada uno
- Reglas globales

**Sin este archivo, OpenCode no sabe qué agente invocar.**

---

## 6. Paso 4 — Configurar la base de conocimiento

Antes de crear agentes, necesitás documentar tu negocio. Los agentes leen estos archivos para entender el contexto.

### 6.1 Archivos obligatorios en `knowledge/`

#### `knowledge/empresa.md` — Quién sos

```markdown
---
title: "Overview de la empresa"
description: "Documento fundacional: qué hace la empresa, modelo de negocio, mercados y métricas clave."
tags:
  - empresa
  - fundacional
---

# Nombre de Tu Empresa

## Qué es

[Breve descripción de qué hace la empresa y el problema que resuelve.]

## Modelo de negocio

[Explicá cómo generan valor y cómo ganan plata. Ej: "Vendemos software por suscripción mensual" o "Ofrecemos servicios de consulting por hora/proyecto".]

## Mercados

[En qué mercados o geografías operan. Ej: "Argentina, focalizados en Mendoza y Córdoba" o "Latinoamérica, con foco en PMEs".]

## Métricas de tracción

| Métrica | Valor |
|---------|-------|
| Tiempo en operación | [X años/meses] |
| Clientes activos | [X] |
| Ingreso mensual | [USD X] |
| Retención | [X%] |

## Diferenciadores

[Qué hace a tu empresa distinta de sus competidores. Ej: "Atención personalizada 24/7" o "Especialización en el rubro logístico".]

## El problema que resuelven

[Descripción clara del problema del cliente que la empresa resuelve. Ej: "Ayudamos a empresas logistic as a automatizar sus procesos de facturación".]

## Documentos relacionados

- [[equipo]] — Estructura del equipo
- [[voz-tono]] — Personalidad y tono de comunicación
- [[glosario]] — Terminología oficial
```

#### `knowledge/equipo.md` — Quién hace qué

```markdown
---
title: "Equipo"
description: "Estructura del equipo, roles y responsabilidades."
tags:
  - empresa
  - equipo
---

# Nuestro Equipo

## Roles y responsabilidades

| Nombre | Rol | Responsabilidades | Contacto |
|--------|-----|-------------------|----------|
| [Nombre] | [Rol] | [Qué hace] | [email] |
| [Nombre] | [Rol] | [Qué hace] | [email] |

## Organigrama

[Quién reporta a quién.]

## Decision makers

[Quién puede tomar qué tipo de decisiones.]

## Documentos relacionados

- [[empresa]] — Contexto general
- [área/equipo] — Equipo por área específica
```

#### `knowledge/voz-tono.md` — Cómo hablás

```markdown
---
title: "Voz y Tono"
description: "Personalidad de marca, tono y estilo de comunicación."
tags:
  - empresa
  - marca
  - comunicación
---

# Voz y Tono de la Empresa

## Personalidad de marca

[Cómo sería la empresa si fuera una persona. Ej: "Somos el amigo que te explica las cosas sin hacerte boludo".]

## Tono general

| Característica | Cómo se manifiesta |
|----------------|-------------------|
| [Profesional] | [Ej: "Comunicados claros y concisos"] |
| [Cercano] | [Ej: "Usamos vos, no usted"] |
| [Transparente] | [Ej: "No ocultamos información"] |

## Frases características

- "[Frase 1 que usan]"
- "[Frase 2 que usan]"

## Palabras que usamos

- [Término 1]
- [Término 2]

## Palabras que evitamos

| Evitamos | Usamos en cambio |
|----------|------------------|
| [Palabra] | [Alternativa] |

## Tono por canal

| Canal | Tono |
|-------|------|
| Email | [Cómo escribimos] |
| WhatsApp | [Cómo escribimos] |
| LinkedIn | [Cómo escribimos] |
| Reuniones | [Cómo hablamos] |
```

#### `knowledge/glosario.md` — Términos que usan

```markdown
---
title: "Glosario"
description: "Términos internos y específicos del negocio."
tags:
  - empresa
  - comunicación
---

# Glosario

## Términos del negocio

| Término | Definición | Ejemplo de uso |
|---------|------------|----------------|
| [Término] | [Qué significa] | [Cómo se usa] |

## Términos técnicos

| Término | Explicación simple | Cuándo usarlo |
|---------|-------------------|---------------|
| [Término tech] | [En lenguaje simple] | [Contexto] |

## Acrónimos

| Acrónimo | Significado |
|----------|-------------|
| [ABC] | [Always Be Closing] |

## Frases internas

- "[Frase 1]" — [Qué significa]
- "[Frase 2]" — [Qué significa]
```

### 6.2 Carpetas por área (`areas/[nombre]/`)

Cada área de tu negocio tiene su propia carpeta con documentos específicos.

#### Estructura mínima por área

```
areas/
└── [nombre-del-area]/
    ├── MOC.md              ← Índice (qué hay en esta carpeta)
    ├── overview.md         ← Qué hace esta área
    ├── objetivos.md        ← Metas y KPIs
    ├── estrategia.md       ← Cómo opera
    ├── procesos.md         ← Flujos clave
    ├── herramientas.md     ← Stack tecnológico
    ├── equipo.md           ← Gente del área
    └── kpis.md             ← Métricas del área
```

#### `MOC.md` — Mapa de contenido del área

```markdown
---
title: "Mapa de contenido — [Área]"
description: "Índice de documentos del área [Nombre]."
tags:
  - MOC
  - [area]
---

# [Área] — Mapa de Contenido

[Breve descripción del área.]

---

## Documentos del área

- [[overview]] — Qué hace esta área, su misión
- [[objetivos]] — Metas y KPIs
- [[estrategia]] — Cómo opera
- [[procesos]] — Flujos clave
- [[herramientas]] — Stack tecnológico
- [[equipo]] — Gente del área
- [[kpis]] — Métricas

---

## Documentos relacionados

- [[../../knowledge/empresa]] — Contexto general
- [[../../AGENTS]] — Agentes disponibles
```

#### `overview.md` — Qué hace el área

```markdown
---
title: "Overview — [Área]"
description: "Qué hace el área, su misión y cómo se relaciona con el resto."
tags:
  - [area]
  - overview
---

# [Área] — Overview

## Misión del área

[Para qué existe esta área en una o dos oraciones.]

## Qué hace

[Descripción de las responsabilidades principales.]

## Cómo se relaciona con otras áreas

| Área | Relación |
|------|----------|
| [Área A] | [Cómo interactúan] |
| [Área B] | [Cómo interactúan] |

## Métricas principales

| Métrica | Descripción |
|---------|-------------|
| [Métrica] | [Qué mide] |

## Documentos relacionados

- [[objetivos]] — Metas y KPIs
- [[estrategia]] — Cómo opera
- [[../../knowledge/empresa]] — Contexto general
```

---

## 7. Paso 5 — Crear tus primeros agentes

### 7.1 Anatomía de un agente

Un agente es un archivo `.md` con tres partes:

```
---
description: |          ← QUÉ hace y CUÁNDO usarlo (la más importante)
  Descripción...
mode: subagent         ← Tipo de agente
temperature: 0.4        ← Qué tan "creativo" es (0 = preciso, 1 = random)
tools:                 ← Qué puede hacer
  write: true
  edit: true
  bash: false
permission:            ← Permisos de edición/ejecución
  edit: ask
---

# Rol del agente         ← IDENTIDAD

## Tu rol              ← QUÉ sos

[Descripción del rol]

## Antes de cualquier tarea  ← CONTEXTO NECESARIO

1. Leé `knowledge/empresa.md`
2. Leé `areas/[area]/overview.md`

## Qué podés hacer     ← CAPACIDADES

- [Capacidad 1]
- [Capacidad 2]

## Proceso de trabajo  ← CÓMO LO HACÉS

1. [Paso 1]
2. [Paso 2]

## Output              ← DÓNDE GUARDÁS

- Guardá outputs en `_inbox/YYYY-MM-DD-[agente]-descripcion.md`

## Límites             ← QUÉ NO HACÉS

- No modifiques archivos fuera de `_inbox/`
- [Otros límites]
```

### 7.2 Crear el archivo del agente

1. Andá a la carpeta `.opencode/agents/`
2. Click derecho → New note
3. Nombralo con el nombre del agente (ej: `comercial.md`)
4. Pegá el contenido del template

### 7.3 Template de agente básico

```markdown
---
description: |
  Agente especializado en [área]. Usar cuando el pedido pertenezca a 
  [descripción de qué tipo de tareas maneja].
mode: subagent
temperature: 0.4
tools:
  write: true
  edit: true
  bash: false
permission:
  edit: ask
---

# Agente [Nombre]

## Tu rol

Soy el agente responsable del área de [Nombre del área]. Mi trabajo es [qué hacés].

## Antes de cualquier tarea

1. Leé `knowledge/empresa.md` para entender el negocio
2. Leé `areas/[area]/overview.md` para entender el área
3. Leé `areas/[area]/objetivos.md` para conocer las prioridades
4. Si la tarea involucra comunicación externa, leé `knowledge/voz-tono.md`
5. Si la tarea involucra terminología específica, leé `knowledge/glosario.md`
6. Revisá `areas/[area]/MOC.md` para ver todos los documentos disponibles
7. Si existe un template relevante en `templates/`, usalo

## Tu área de conocimiento

Tu documentación base está en `areas/[area]/`. Revisá el `MOC.md` de esa carpeta.

## Qué podés hacer

- [Capacidad 1]
- [Capacidad 2]
- [Capacidad 3]

## Proceso de trabajo

1. Leé el contexto necesario antes de ejecutar
2. Si falta información crítica, pedila antes de inventar
3. Proponé 2-3 variantes cuando tenga sentido
4. Guardá outputs en `_inbox/` si te lo piden

## Output

- Guardá outputs en `_inbox/YYYY-MM-DD-[agente]-descripcion.md`
- Sé específico: preferí entregables concretos sobre respuestas genéricas

## Límites

- No modifiques archivos fuera de `_inbox/`
- No tomes decisiones estratégicas sin aprobación humana
- No inventés datos que no estén documentados
- Si la tarea involucra otra área, derivá o coordiná
- Si necesitás actualizar documentación, derivá a otro agente
```

### 7.4 Ejemplo: Agente comercial completo

```markdown
---
description: |
  Agente Comercial. Especialista en ventas, canales de distribución, prospección
  de clientes y estrategias comerciales. Usar cuando el pedido pertenezca al área
  de Comercial: ventas, canales, prospección, cierres, estrategias de revenue.
mode: subagent
temperature: 0.4
tools:
  write: true
  edit: true
  bash: false
permission:
  edit: ask
---

# Agente Comercial

## Tu rol

Soy el agente responsable del área de Comercial. Mi trabajo es ayudar con:
- Prospección y calificación de leads
- Estrategia de canales de distribución
- Preparación de propuestas comerciales
- Análisis de oportunidades de venta
- Estrategia de pricing

## Antes de cualquier tarea

1. Leé `knowledge/empresa.md` para entender el negocio
2. Leé `areas/comercial/overview.md` para entender el área
3. Leé `areas/comercial/objetivos.md` para conocer las prioridades actuales
4. Si la tarea involucra comunicación externa, leé `knowledge/voz-tono.md`
5. Si la tarea involucra terminología específica, leé `knowledge/glosario.md`
6. Revisá `areas/comercial/MOC.md` para ver todos los documentos disponibles

## Qué podés hacer

- Análisis de oportunidades de venta
- Prospección y qualify de leads
- Estrategia de canales de distribución
- Preparación de propuestas comerciales
- Reportes de performance comercial
- Briefing para equipos de venta
- Análisis de competencia y mercado
- Estrategia de pricing

## Proceso de trabajo

1. Leé el contexto necesario antes de ejecutar
2. Si falta información crítica para la tarea, pedila antes de inventar
3. Proponé 2-3 variantes cuando tenga sentido
4. Guardá outputs en `_inbox/` si me lo piden

## Output

- Guardá outputs en `_inbox/YYYY-MM-DD-comercial-descripcion.md`
- Sé específico: preferí entregables concretos sobre respuestas genéricas

## Límites

- No modifiques archivos fuera de `_inbox/`
- No tomes decisiones comerciales estratégicas sin aprobación humana
- No inventés datos de mercado que no estén documentados
- Si la tarea involucra otra área, derivá o coordiná con el agente correspondiente
```

---

## 8. Paso 6 — Crear skills (opcional pero recomendado)

Las skills son conjuntos de instrucciones que cargan contexto específico. Se usan para no repetir información en cada agente.

### 8.1 Cuándo crear una skill

| Situación | ¿Crear skill? |
|-----------|----------------|
| Varios agentes necesitan leer los mismos archivos | ✅ Sí |
| Es contexto que cambia seguido | ✅ Sí |
| Un solo agente usa esa info | ❌ No (metélo directo en el agente) |

### 8.2 Estructura de una skill

```
.opencode/
└── skills/
    └── [nombre-skill]/
        └── SKILL.md
```

### 8.3 Template de skill

```markdown
---
name: [nombre-skill]
description: >
  [Descripción de cuándo usar esta skill. Ej: "Carga contexto específico del área
  comercial. Usar cuando trabajes en tareas de ventas o prospección."]
---

# [Nombre de la Skill]

## Instrucciones

Leé los siguientes archivos para cargar el contexto:

1. `knowledge/empresa.md` — Qué es, modelo de negocio, mercados
2. `knowledge/equipo.md` — Estructura del equipo
3. `knowledge/glosario.md` — Terminología oficial
[Añadir más archivos según necesidad]

## Qué encontrás en este contexto

- [Descripción del contexto que se carga]
- [Información específica]

## Cómo aplicar

- Usá esta skill como punto de partida obligatorio para cualquier tarea
- No inventes información que no esté documentada
- Si hay info desactualizada, notificá al agente principal

## Completar con información real

Este archivo carga información de los archivos de `knowledge/`. Completá esos
archivos con la información real de tu empresa antes de usar esta skill en producción.
```

### 8.4 Ejemplo: Skill de contexto de empresa

```markdown
---
name: empresa-context
description: >
  Carga el contexto general de la empresa: modelo de negocio, mercados,
  métricas y diferenciadores. Usar cuando necesites entender qué hace la empresa
  y cómo genera valor antes de ejecutar cualquier tarea.
---

# Empresa Context

## Instrucciones

Leé los siguientes archivos para conocer la empresa:

1. `knowledge/empresa.md` — Qué es, modelo de negocio, mercados
2. `knowledge/equipo.md` — Estructura del equipo
3. `knowledge/glosario.md` — Terminología oficial

## Qué encontrás en este contexto

- Descripción del negocio y propuesta de valor
- Modelo de monetización
- Mercados y geografías de operación
- Métricas de tracción y crecimiento
- Diferenciadores competitivos
- Estructura del equipo

## Cómo aplicar

- Usá esta skill como punto de partida obligatorio para cualquier tarea
- No inventes información sobre la empresa que no esté documentada
- Si hay info desactualizada, notificá al `@ORQUESTADOR`

## Completar con información de tu empresa

Este archivo carga información de `knowledge/empresa.md`. Completá ese archivo
con la información real de tu empresa antes de usar esta skill en producción.
```

---

## 9. Paso 7 — Configurar el router principal (AGENTS.md)

El archivo `AGENTS.md` es el "cerebro" de enrutamiento. Tiene que estar en la **raíz del vault**.

### 9.1 Estructura del AGENTS.md

```markdown
# [Nombre del Vault] — Router Principal

## Contexto

[Breve descripción del sistema de agentes y cómo funciona.]

## Base de conocimiento

### Antes de cualquier tarea
El punto de entrada obligatorio es [[knowledge/empresa]], que explica qué hace 
la empresa, su modelo de negocio y mercados activos. Complementalo con 
[[knowledge/equipo]] para entender quién hace qué.

### Antes de crear contenido
Leé [[knowledge/voz-tono]] para respetar la personalidad de marca y 
[[knowledge/glosario]] para usar la terminología correcta.

### Antes de ejecutar tareas por área
Cada área tiene su propia carpeta en `areas/` con documentación específica. 
Revisá el `MOC.md` de la carpeta del área correspondiente.

---

## Agentes disponibles

| Intención | Agente | Cuándo usarlo |
|-----------|--------|---------------|
| [Tipo de tarea] | `@nombre` | [Cuándo invocar este agente] |

---

## Reglas de enrutamiento

1. [Regla 1]
2. [Regla 2]
3. [Regla 3]

---

## Reglas globales

- [Regla global 1]
- [Regla global 2]
- [Regla global 3]
```

### 9.2 Ejemplo completo de AGENTS.md

```markdown
# Mi Negocio — Router Principal

## Contexto

Este es el sistema de agentes de Mi Negocio. La base de conocimiento del 
equipo está en `knowledge/` y en las carpetas de cada área en `areas/`.

## Base de conocimiento

### Antes de cualquier tarea
El punto de entrada obligatorio es [[knowledge/empresa]], que explica qué hace 
la empresa, su modelo de negocio y mercados activos. Complementalo con 
[[knowledge/equipo]] para entender quién hace qué.

### Antes de crear contenido
Leé [[knowledge/voz-tono]] para respetar la personalidad de marca y 
[[knowledge/glosario]] para usar la terminología correcta.

### Antes de ejecutar tareas por área
Cada área tiene su propia carpeta en `areas/` con documentación específica. 
Revisá el `MOC.md` de la carpeta del área correspondiente para entender qué 
documentos leer.

---

## Agentes disponibles

| Intención | Agente | Cuándo usarlo |
|-----------|--------|---------------|
| Coordinar tareas | `@ORQUESTADOR` | Punto de entrada para cualquier request |
| Ventas y canales | `@comercial` | Prospección, cierre, estrategias comerciales |
| Marketing y contenido | `@marketing` | Campañas, redes sociales, contenido |
| Producto y desarrollo | `@desarrollo` | Features, roadmap, documentación técnica |
| Atención al cliente | `@soporte` | Helpdesk, incidencias, resolución |
| Operaciones | `@operaciones` | Procesos internos, coordinación |
| Datos y métricas | `@analytics` | Reportes, dashboards, insights |

---

## Reglas de enrutamiento

1. Identificá el área de la empresa a la que pertenece la tarea
2. Delegá al agente correspondiente
3. Si la tarea **combina varias áreas**, descomponela y delegá cada parte
4. Si **no encaja en ningún área**, respondé directamente o pedí clarificación
5. Si **falta contexto crítico**, pedí lo mínimo necesario antes de ejecutar

---

## Reglas globales

- Siempre respetar `knowledge/voz-tono.md`
- Usar los templates de `templates/` cuando exista uno para el formato pedido
- Formato de fechas: YYYY-MM-DD en archivos, DD/MM/YYYY en contenido
- Los agentes escriben outputs en `_inbox/` salvo que se indique otra cosa
- Idioma por defecto: español
```

---

## 10. Paso 8 — Primeros pasos con OpenCode

### 10.1 Abrir el vault con OpenCode

```bash
# Navegá a la carpeta del vault
cd ruta/a/tu-vault

# Ejecutá opencode
opencode
```

**Ejemplo Windows:**
```bash
cd C:\Users\TuNombre\Desktop\mi-negocio-vault
opencode
```

**Ejemplo Mac/Linux:**
```bash
cd ~/Desktop/mi-negocio-vault
opencode
```

### 10.2 Comandos básicos

Una vez dentro de OpenCode:

| Comando | Qué hace |
|---------|----------|
| `@agente [pedido]` | Invoca a un agente específico con un pedido |
| `[pregunta directa]` | Le preguntás algo sin invocar agente |
| `exit` o `quit` | Salir de OpenCode |
| `help` | Ver ayuda |

### 10.3 Ejemplos de uso

**Invocar un agente específico:**
```
@comercial Necesito una lista de 10 estrategias para captar leads en el rubro logístico
```

**Pregunta directa:**
```
¿Qué métricas debería seguir para el área de ventas?
```

**Pedido complejo:**
```
@marketing Armame un calendario de contenido para Instagram para todo marzo. Somos una empresa de servicios de limpieza para oficinas.
```

### 10.4 Verificar que funciona

Probá esto:

```
@ORQUESTADOR ¿Qué áreas tenemos documentadas?
```

Si el agente responde con info de tu vault, ¡todo funciona! 🎉

### 10.5 Mensajes de error comunes

| Error | Solución |
|-------|----------|
| `Command not found: opencode` | Reinstalá OpenCode o verificá que esté en el PATH |
| `No agents found` | Checkeá que AGENTS.md esté en la raíz del vault |
| `Permission denied` | Verificá los permisos del archivo `AGENTS.md` |
| `API key not found` | Configurá tu API key con `opencode config set` |

---

## 11. Ejemplos completos

### 11.1 Ejemplo: Agente de soporte técnico

```markdown
---
description: |
  Agente de Soporte Técnico. Especialista en helpdesk, resolución de problemas
  técnicos y gestión de incidencias. Usar cuando el pedido sea sobre soporte a
  usuarios, troubleshooting, FAQs técnicos o gestión de tickets.
mode: subagent
temperature: 0.3
tools:
  write: true
  edit: false
  bash: false
permission:
  edit: ask
---

# Agente Soporte

## Tu rol

Soy el agente responsable del área de Soporte Técnico. Mi trabajo es:
- Resolver problemas técnicos de los usuarios
- Gestionar tickets de soporte
- Crear FAQs y documentación de troubleshooting
- Escalar temas cuando sea necesario

## Antes de cualquier tarea

1. Leé `knowledge/empresa.md` para entender el negocio
2. Leé `areas/soporte/overview.md` para entender el área
3. Leé `areas/soporte/procesos.md` para conocer el flujo de soporte
4. Si la tarea involucra términos técnicos, leé `knowledge/glosario.md`
5. Revisá `areas/soporte/MOC.md` para ver todos los documentos disponibles

## Qué podés hacer

- Troubleshooting de problemas comunes
- Redacción de respuestas a tickets
- Creación de guías de ayuda
- Clasificación y priorización de incidencias
- Escalamiento a desarrollo cuando corresponda

## Proceso de trabajo

1. Identificá el tipo de problema
2. Buscá si hay documentación previa (FAQ, guías)
3. Si no hay, resolvé con lógica y buena práctica
4. Si no puedo resolver, escalá al agente de desarrollo
5. Documentá la solución para futura referencia

## Output

- Guardá outputs en `_inbox/YYYY-MM-DD-soporte-descripcion.md`
- Incluí pasos de resolución para referencia futura

## Límites

- No modifiques archivos fuera de `_inbox/`
- No proporciones credenciales o información sensible
- No realices cambios en sistemas de producción
- Escalá a `@desarrollo` si el problema requiere cambios de código
```

### 11.2 Ejemplo: Skill de contexto comercial

```markdown
---
name: comercial-context
description: >
  Carga contexto específico del área comercial: estrategias de venta, canales,
  prospección y métricas comerciales. Usar cuando trabajes en tareas de ventas,
  prospección de clientes, estrategias de revenue o preparación de propuestas.
---

# Comercial Context

## Instrucciones

Leé los siguientes archivos:

1. `knowledge/empresa.md` — Modelo de negocio y propuesta de valor
2. `areas/comercial/overview.md` — Misión y responsabilidades del área
3. `areas/comercial/objetivos.md` — Metas y KPIs comerciales actuales
4. `areas/comercial/estrategia.md` — Canales y estrategias activas
5. `areas/comercial/procesos.md` — Flujo de ventas y operativos
6. `knowledge/glosario.md` — Terminología de ventas

## Qué encontrás en este contexto

- Estrategias de canal y distribución
- Proceso de prospección y qualify
- Métricas de seguimiento comercial
- Pricing y modelos de revenue
- Propuestas y materiales de venta

## Cómo aplicar

- Usá esta skill como punto de partida para cualquier tarea de `@comercial`
- No inventés estrategias o tácticas que no estén documentadas
- Si hay info desactualizada, notificá al `@comercial`

## Completar con información de tu empresa

Esta skill carga información del área comercial de tu vault. Completá los
archivos en `areas/comercial/` con la información real antes de usar.
```

---

## 12. Mejores prácticas

### 12.1 Para que los agentes funcionen bien

**✅ HACER:**

1. **Documentá todo:** Cuanta más info en tu vault, mejor van a responder los agentes
2. **Mantené la info actualizada:** Un agente con info vieja es peor que ninguno
3. **Usá descripciones claras:** La `description` del frontmatter es lo que usa OpenCode para decidir a quién invocar
4. **Separó áreas por responsabilidad:** Cada agente = un área clara
5. **Empezá simple:** No intentes documentar todo de entrada. Arrancá con lo crítico.

**❌ NO HACER:**

1. **No dejar archivos vacíos:** Si el archivo no tiene contenido, el agente no puede leerlo
2. **No inventar info:** Si no está documentado, decí "no tengo esa info"
3. **No dar permisos de más:** Los agentes solo necesitan `write` y a veces `bash`
4. **No hacer agentes multi-propósito:** Si un agente hace de todo, no hace nada bien
5. **No skippear el AGENTS.md:** Sin router, el sistema no sabe a quién invocar

### 12.2 Estructura de archivos recomendada

```
vault/
├── .opencode/
│   ├── agents/           ← Agentes (uno por archivo)
│   └── skills/           ← Skills (una carpeta por skill)
├── areas/                ← Un archivo por rol/área
│   ├── comercial/
│   │   ├── MOC.md        ← Índice DEL ÁREA (clave)
│   │   └── *.md          ← Documentos del área
│   └── [otras áreas]
├── knowledge/            ← Contexto general
│   ├── empresa.md        ← OBLIGATORIO
│   ├── equipo.md         ← OBLIGATORIO
│   ├── voz-tono.md       ← OBLIGATORIO
│   └── glosario.md       ← MUY RECOMENDADO
├── templates/            ← Plantillas de documentos
├── _inbox/               ← Output de agentes
├── _meta/                ← Docs del sistema
└── AGENTS.md             ← ROUTER (clave)
```

### 12.3 Tips de organización

| Problema | Solución |
|----------|----------|
| Agente que no sabe qué hacer | Revisá que tenga los archivos correctos en "Antes de cualquier tarea" |
| Agente que inventa info | Agregá "No inventés información, decí 'no tengo esa info'" |
| Agente que no para de hablar | Agregá límite de output o pedí formato específico |
| No sé qué agente invocar | Creá `@ORQUESTADOR` que sea el punto de entrada único |
| Documentación desactualizada | Poné fecha de última actualización y revisá periódicamente |

---

## 13. FAQ — Preguntas frecuentes

### 13.1 ¿Necesito saber programar?

**No.** El sistema está diseñado para que cualquier persona pueda usarlo. Si sabés usar Obsidian (tomar notas, crear archivos), podés usar esto.

### 13.2 ¿Cuánto cuesta?

| Componente | Costo |
|------------|-------|
| Obsidian | Gratis (personal) / USD 8/mes (Catalyst) |
| OpenCode | Gratis (open source) |
| API de IA (Anthropic/OpenAI) | Crédito gratis inicial + pay-as-you-go |

**Costo mínimo:** $0 (usando el tier gratuito de las APIs).

### 13.3 ¿Mis datos están seguros?

**Depende de vos:**
- Los archivos de tu vault están en tu computadora
- La info se manda a la API de IA (Anthropic o OpenAI) para procesarla
- Si usás Obsidian Git, se sincroniza a GitHub (también asegurable)

### 13.4 ¿Puedo tener varios vaults?

**Sí.** Cada vault es independiente. Podés tener:
- `mi-negocio-vault`
- `cliente-a-vault`
- `proyecto-x-vault`

Cada uno con sus propios agentes y skills.

### 13.5 ¿Cómo actualizo los agentes?

Simplemente editá el archivo `.md` del agente. Los cambios se aplican la próxima vez que invoques al agente.

### 13.6 ¿Qué pasa si un agente no responde bien?

1. Verificá que tenga toda la documentación necesaria
2. Agregá o actualizá los archivos en `knowledge/` o `areas/`
3. Ajustá las instrucciones en el campo `## Qué podés hacer`
4. Agregá ejemplos en la descripción del frontmatter

### 13.7 ¿Puedo crear agentes para clientes?

**Sí.** Cada vault puede ser para un cliente diferente. Los agentes saben de ese cliente específico porque leés los archivos de `knowledge/` y `areas/`.

### 13.8 ¿OpenCode funciona en celular?

Hay una app de OpenCode para iOS/Android, pero para **configurar** el sistema necesitás una computadora.

---

## Próximos pasos

Una vez que tengas todo funcionando:

1. **Completá la base de conocimiento** — Empezá con `knowledge/empresa.md`
2. **Creá tus primeros 2-3 agentes** — Sugerencia: `@comercial` + `@soporte`
3. **Probá con pedidos reales** — Invocalos desde OpenCode
4. **Iterá y mejorá** — Ajustá las instrucciones según cómo respondan
5. **Expandí** — Agregá más áreas y agentes según necesidad

---

## Recursos adicionales

- [Documentación oficial de OpenCode](https://opencode.ai/docs)
- [Comunidad de OpenCode](https://discord.gg/opencode)
- [Documentación de Obsidian](https://help.obsidian.md)

---

*Guía creada el 30/03/2026 — Versión 1.0*

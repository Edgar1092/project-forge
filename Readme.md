# ProjectForge 🔨

> Meta-agente que convierte cualquier IA en un arquitecto y orquestador de proyectos.
> Entrevista, detecta inconsistencias, genera el kit completo de agentes IA — y en runtime procesa cada tarea con un pipeline Analista → Desarrollador → QA con memoria persistente compartida.

---

## ¿Qué es ProjectForge?

ProjectForge es un **system prompt portable** con dos capas:

**Capa Arquitecto (entrevista):** al activarse en una IA (Claude Code, Claude, Copilot, Cursor...) se convierte en un consultor técnico senior que:

1. **Entrevista** al desarrollador con preguntas agrupadas y progresivas
2. **Investiga** activamente proyectos similares, stacks recomendados y riesgos
3. **Detecta inconsistencias** técnicas antes de continuar
4. **Opina con criterio senior** — no solo levanta información, recomienda
5. **Genera el kit multi-host** adaptado a las herramientas de cada dev del equipo

**Capa Orquestador (runtime):** el kit generado convierte a la IA del proyecto en un orquestador ("Jarvis") que procesa cada tarea, historia de usuario o bug mediante un **pipeline de roles especializados** con memoria persistente vía [engram](https://github.com/Gentleman-Programming/engram).

---

## ¿Qué problema resuelve?

**Sin ProjectForge:** le explicas a la IA tu proyecto cada vez que abres una conversación, el contexto se pierde entre sesiones y entre devs, y el código sale sin un proceso de revisión consistente.

**Con ProjectForge:** una sesión de entrevista genera el kit. De ahí en adelante, cualquier IA del equipo entiende el proyecto desde el primer mensaje, cada tarea pasa por análisis → desarrollo → QA, y las decisiones quedan en una memoria compartida que sobrevive sesiones, agentes y máquinas.

---

## Novedades de la v3

| | v2 | v3 |
|---|---|---|
| Roles | Rol QA interno en un solo prompt | **Subagentes reales** (Analista, Dev, QA, Seguridad, Infra) en Claude Code; modo degradado en otros hosts |
| Pipeline | Revisión QA al final | **Triaje + pipeline por etapas** con handoffs explícitos y loop QA↔Dev acotado |
| Memoria | MEMORY_LOG.md manual | **engram** — SQLite local + MCP, compartida entre agentes, hosts y devs |
| Hosts | Un prompt para todos | **Adaptadores por host** generados desde una fuente canónica |
| Skills | Roadmap de aprendizaje del equipo | + **skills de agente sugeridas** según stack y gaps del equipo |
| Confidencialidad | — | **Modo confidencial** para repos que no pueden mostrar rastros de IA |

---

## Modos de operación

```
[A] 🚀 Proyecto Nuevo      → Entrevista guiada desde cero
[B] 🔍 Proyecto Existente  → Analiza código existente · migra kits v2 a v3
[C] 🔄 Actualizar Kit      → Regenera fuente canónica y adaptadores
```

---

## Kit generado (multi-host)

**Principio: una fuente canónica, adaptadores generados.** Los adaptadores nunca se editan a mano — se regeneran con el Modo C.

```
proyecto/
├── documents/                         ← FUENTE CANÓNICA
│   ├── AI_GUIDELINES.md               ← reglas, convenciones, checklist QA
│   ├── PROJECT_SUMMARY.md             ← descripción, stack, estado, fases MVP
│   ├── PIPELINE.md                    ← triaje, etapas, handoffs, protocolo de memoria
│   └── TEAM_SKILLS.md                 ← gaps del equipo + roadmap (opcional)
│
├── CLAUDE.md                          ← adaptador Claude Code (orquestador)
├── .claude/
│   ├── agents/                        ← analista · desarrollador · qa · seguridad · infra
│   ├── commands/                      ← /tarea · /bug · /memoria
│   └── skills/                        ← skills sugeridas según stack
│
├── .github/copilot-instructions.md    ← adaptador Copilot (si algún dev lo usa)
├── .cursor/rules/projectforge.mdc     ← adaptador Cursor (si algún dev lo usa)
│
├── GETTING_STARTED.md                 ← arranque + setup de engram paso a paso
└── system-prompt.txt                  ← versión comprimida para cualquier IA
```

### Matriz de capacidades por host

| Capacidad | Claude Code | Cursor | Copilot (VS Code) |
|---|---|---|---|
| Pipeline multi-agente real (subagentes aislados) | ✅ | ❌ degradado | ❌ degradado |
| Comandos /tarea /bug /memoria | ✅ | parcial | ❌ |
| Skills instalables | ✅ | ❌ | ❌ |
| Memoria engram vía MCP | ✅ | ✅ | ✅ |
| Fuente canónica documents/ | ✅ | ✅ | ✅ |

> Claude Code dentro de VS Code cuenta como Claude Code — es el mismo CLI, misma experiencia completa.
> El **modo degradado** ejecuta el mismo pipeline secuencialmente en un solo agente: mismas etapas, mismos artefactos, misma memoria. Se pierde el aislamiento de contexto, no el método.

---

## El pipeline (runtime)

### Triaje

Toda solicitud de trabajo se clasifica antes de delegar:

```
🧭 Triaje: HU-042
   Clasificación: TRIVIAL | ESTÁNDAR | SENSIBLE
   Pipeline: [etapas que aplicará]
```

- **TRIVIAL** → Dev directo con auto-QA inline (sin quemar 5 agentes en un typo)
- **ESTÁNDAR** → 📐 Analista → 💻 Desarrollador → 🔍 QA
- **SENSIBLE** (auth, pagos, datos personales, deploy) → pipeline + 🛡️ Seguridad y/o ⚙️ Infraestructura

### Etapas y handoffs

Los subagentes **no comparten contexto** — cada etapa produce un artefacto que el orquestador pasa literal a la siguiente:

```
📐 ANALISTA      → SPEC (objetivo, criterios Given/When/Then, archivos, riesgos)
💻 DESARROLLADOR → código + RESUMEN DE IMPLEMENTACIÓN (qué, por qué, dónde)
🔍 QA            → VEREDICTO  ✅ listo · ⚠️ observaciones · 🔴 bloqueante → vuelve a Dev
```

El loop QA↔Dev tiene tope de **2 iteraciones** — a la tercera 🔴 escala al humano con el historial. Nunca loop infinito.

---

## Memoria persistente con engram

[engram](https://github.com/Gentleman-Programming/engram) es un binario local (Go + SQLite + FTS5) que expone memoria vía MCP a cualquier agente. ProjectForge lo usa como **única fuente de verdad de memoria episódica** — MEMORY_LOG.md desaparece.

```
AL INICIAR SESIÓN   → git pull + engram sync (importa memorias del equipo)
AL INICIAR TAREA    → mem_search con el ID de la tarea + módulo
AL CERRAR ETAPA     → mem_save (What/Why/Where/Learned, topic key = ID tarea)
AL CERRAR SESIÓN    → engram sync + commit de .engram/
```

- **Por proyecto:** una sola instalación, memorias aisladas automáticamente por proyecto — tus proyectos personales no se mezclan con los de la empresa.
- **Por equipo:** `engram sync` viaja por Git — el dev de Cursor ve las decisiones que guardaste desde Claude Code.
- **Conflictos:** si una decisión nueva contradice una vieja, engram lo detecta y el agente marca `supersedes`.
- **Separación estricta:** reglas estables → `documents/` · historia episódica → engram. Nunca al revés.

ProjectForge incluye **instalación asistida**: instala el binario, configura el host, te pide el reinicio y **verifica con `mem_stats` antes de migrar o usar memoria**.

---

## 🔒 Modo confidencial

Para proyectos donde el cliente prohíbe rastros de IA en el repo:

- Todo el kit queda **local**, excluido vía `.git/info/exclude` (no `.gitignore` — el gitignore se commitea y delata)
- engram en modo solo local — prohibido `engram sync` contra el repo del cliente; backup vía `engram export` o sync a un repo privado propio
- Higiene de commits: sin footers de co-autoría de IA, sin menciones a herramientas
- Checklist de verificación automática al activarlo

Se declara en la entrevista (Fase 2) y el kit se genera ya configurado.

---

## Características clave

### 🔍 Investigación activa
Busca proyectos similares, valida el stack propuesto, identifica riesgos conocidos y recomienda con justificación.

### ⚠️ Detector de inconsistencias
- *"Mencionaste Nx pero también repos separados — se contradicen"*
- *"Elegiste pipeline multi-agente pero ningún dev usa Claude Code — correrá siempre degradado, ¿lo confirmas?"*
- *"Esta decisión contradice la memoria HU-031 — ¿la nueva la reemplaza o conviven?"*

### 🧩 Skills sugeridas por stack y gaps
Cruza el stack contra los skills del equipo. Donde hay gap 🔴, la skill de agente es **prioritaria** — la IA compensa lo que el equipo aún no puede revisar.

### 🧪 Tests automáticos configurables
Flag `TESTS_AUTOMATICOS` por scope (todos / solo backend / solo frontend / desactivados), cambiable con lenguaje natural.

### 🌐 Bilingüe
Español o inglés — se elige al inicio y se mantiene toda la sesión.

---

## Cómo usar ProjectForge

### Opción 1 — Claude Code (recomendado: experiencia completa)
```bash
# Pegar el contenido de projectforge-v3-prompt.md en el CLAUDE.md
# de una carpeta vacía, abrir Claude Code y escribir cualquier mensaje.
# Funciona igual desde la terminal o desde la extensión de VS Code.
```

### Opción 2 — Claude Projects
Crear un proyecto en [claude.ai](https://claude.ai) y pegar el prompt en las instrucciones.

### Opción 3 — GitHub Copilot
```bash
.github/copilot-instructions.md
```

### Opción 4 — Cursor
```
Settings → Rules for AI → pegar el contenido
```

### Opción 5 — Windsurf / Cline / ChatGPT
`.windsurfrules` · `.clinerules` · Custom GPT con el prompt como instrucciones.

> En hosts sin subagentes, ProjectForge entrevista y genera el kit igual; el pipeline correrá en modo degradado.

---

## Migrar un proyecto v2 a v3

1. Instala engram (ProjectForge te guía, o ejecuta los pasos él mismo en Claude Code)
2. Reinicia el host y verifica `mem_stats`
3. Modo B → pega tu kit v2 (AGENT_PROMPT, PROJECT_SUMMARY, MEMORY_LOG)
4. ProjectForge regenera la estructura v3 y migra **solo las entradas significativas** del MEMORY_LOG a engram (decisiones, aprendizajes, bugs con causa raíz — el ruido no se migra)
5. `engram sync` + commit de `.engram/` (salvo modo confidencial)

---

## Ejemplo de sesión (runtime)

```
Usuario: /tarea HU-042 validación de cupones en el checkout

Orquestador: 🧭 Triaje: HU-042
             Clasificación: SENSIBLE (toca flujo de pagos)
             Pipeline: Analista → Dev → QA → Seguridad

             🧠 mem_search "HU-042 checkout"...
             Encontré: "HU-031: elegimos validación server-side de
             promos" — lo paso al Analista.

             📐 Analista → SPEC generado (4 criterios de aceptación)
             💻 Desarrollador → 3 archivos + tests (RESUMEN adjunto)
             🔍 QA → ⚠️ Observaciones menores: [falta caso cupón expirado]
             💻 Desarrollador → corregido
             🔍 QA → ✅ Sin observaciones
             🛡️ Seguridad → ✅ Sin observaciones

             ━━━━━━━━━━━━━━━━━━━━━━━━━━━
             📦 HU-042 — Completada
                📐 ✅ · 💻 ✅ · 🔍 ✅ · 🛡️ ✅
                🧠 Memoria guardada: HU-042
             ━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Stack agnóstico

- **Backend:** NestJS · Express · FastAPI · Laravel · Spring Boot
- **Frontend:** Angular · React · Vue · Next.js
- **Mobile:** Ionic · React Native · Flutter
- **Base de datos:** PostgreSQL · MySQL · MongoDB · SQLite
- **Monorepo:** Nx · Turborepo · Lerna
- **Testing:** Jest · Vitest · Jasmine · Pytest

---

## Archivos del repositorio

```
projectforge/
├── projectforge-v3-prompt.md   ← El system prompt de ProjectForge v3
├── README.md                   ← Esta guía
└── examples/
    └── urvi/                   ← Ejemplo de kit generado
```

---

## Contribuir

1. Fork del repo
2. Rama: `git checkout -b feature/mejora-nombre`
3. Documenta el caso de uso en el PR
4. PR contra `main`

---

## Licencia

MIT — libre para uso personal y comercial.

---

> Creado con [Claude](https://claude.ai) · Versión 3.0 · Junio 2026
> Memoria persistente por [engram](https://github.com/Gentleman-Programming/engram) (Gentleman Programming)

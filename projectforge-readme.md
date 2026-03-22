# ProjectForge 🔨

> Meta-agente que convierte cualquier IA en un arquitecto de proyectos interactivo.
> Genera el kit completo de agente IA para cualquier proyecto de desarrollo, desde cero o analizando uno existente.

---

## ¿Qué es ProjectForge?

ProjectForge es un **system prompt portable** que al activarse en cualquier IA (Claude, Copilot, Cursor, ChatGPT, etc.) la convierte en un consultor técnico senior que:

1. **Entrevista** al desarrollador con preguntas agrupadas y progresivas
2. **Investiga** activamente proyectos similares, stacks recomendados y riesgos
3. **Detecta inconsistencias** técnicas antes de continuar
4. **Opina con criterio senior** — no solo levanta información, recomienda
5. **Genera el kit completo** de documentación lista para usar con cualquier IA

El output es un conjunto de archivos `.md` + un `system-prompt.txt` portable que convierte a la IA en el asistente de desarrollo específico de tu proyecto.

---

## ¿Qué problema resuelve?

Configurar una IA para que trabaje bien en un proyecto específico requiere tiempo: definir el stack, las convenciones, las prohibiciones, los flujos, el estado actual... ProjectForge automatiza ese proceso mediante una entrevista guiada con criterio técnico real.

**Sin ProjectForge:** Le explicas a la IA tu proyecto cada vez que abres una conversación nueva.

**Con ProjectForge:** Una sola sesión de entrevista genera el kit completo. De ahí en adelante, cualquier IA entiende tu proyecto desde el primer mensaje.

---

## Modos de operación

```
[A] 🚀 Proyecto Nuevo      → Entrevista guiada desde cero
[B] 🔍 Proyecto Existente  → Analiza código y estructura existente
[C] 🔄 Actualizar Kit      → Actualiza documentos ya generados
```

---

## Kit generado

Al completar la entrevista, ProjectForge genera estos archivos:

| Archivo | Contenido |
|---|---|
| `documents/AGENT_PROMPT.md` | Rol del agente, bucle agéntico, prohibiciones, estructura del proyecto |
| `documents/AI_GUIDELINES.md` | Reglas absolutas, convenciones de código, ejemplos por stack, rol QA, checklist |
| `documents/PROJECT_SUMMARY.md` | Descripción, stack, estado actual, módulos, flujos, fases MVP |
| `documents/MEMORY_LOG.md` | Registro persistente de tareas por desarrollador |
| `documents/TEAM_SKILLS.md` | Mapa de gaps del equipo y roadmap de aprendizaje por fase *(opcional)* |
| `GETTING_STARTED.md` | Guía paso a paso: repo, scaffolding, BD, verificación, primera tarea |
| `system-prompt.txt` | Todo comprimido en un bloque portable listo para pegar en cualquier IA |

---

## Flujo de entrevista (Modo A)

```
Fase 1 — Descubrimiento
  → Nombre del proyecto (o sugerencias automáticas)
  → Descripción del negocio y roles
  → Stack tecnológico o recomendaciones
  → Tamaño y seniority del equipo

  ↓ La IA investiga: proyectos similares, stack recomendado, riesgos
  ↓ Detector de inconsistencias activo

Fase 2 — Definición Técnica
  → Stack confirmado y versiones
  → Estructura del proyecto (monorepo, repos separados, etc.)
  → Integraciones externas
  → Paradigma de trabajo (AI-First, TDD, Clean Architecture)
  → Skills del equipo → análisis de gaps automático

Fase 3 — Reglas y Convenciones
  → Idioma de comentarios
  → Prohibiciones y patrones obligatorios
  → Manejo de errores y formato de respuesta API
  → Framework de testing y configuración de tests automáticos

Fase 4 — Validación Final
  → Resumen completo para confirmar
  → CONFIRMAR → genera el kit completo
```

---

## Características clave

### 🔍 Investigación activa
La IA no espera que el desarrollador sepa todo. Busca proyectos similares, valida el stack propuesto, identifica riesgos conocidos y hace recomendaciones justificadas.

### ⚠️ Detector de inconsistencias
Si algo se contradice técnicamente, lo señala antes de continuar:
- *"Mencionaste Nx pero también repos separados — se contradicen"*
- *"TypeORM es relacional pero mencionaste MongoDB — son paradigmas distintos"*
- *"1 dev junior + ese scope es muy alto para un MVP 1"*

### 🔍 Rol QA integrado
El agente generado incluye un rol QA senior. Antes de entregar cualquier código ejecuta una revisión interna y reporta:
```
✅ Sin observaciones — código listo
⚠️  Observaciones menores: [lista]
🔴 Bloqueante encontrado — corrijo antes de entregar
```

### 🧪 Tests automáticos configurables
Los tests unitarios se generan automáticamente junto con cada módulo. Configurable por scope:
- `ACTIVADOS (todos)` — backend + frontend + mobile
- `ACTIVADOS (solo backend)`
- `ACTIVADOS (solo frontend)`
- `DESACTIVADOS`

El desarrollador puede cambiar esto en cualquier momento con lenguaje natural.

### 📊 Roadmap de skills del equipo
Cruza el stack requerido contra los skills declarados del equipo, identifica gaps con semáforo de riesgo y genera un roadmap de aprendizaje por fase del MVP con recursos específicos.

### 🌐 Bilingüe
Español o inglés — el agente pregunta al inicio y mantiene el idioma elegido durante toda la sesión.

---

## Cómo usar ProjectForge

### Opción 1 — Claude Projects (recomendado)
1. Crea un nuevo proyecto en [claude.ai](https://claude.ai)
2. En las instrucciones del proyecto, pega el contenido de `AGENT_PROMPT.md` (este archivo)
3. Escribe cualquier mensaje para arrancar — ProjectForge toma el control

### Opción 2 — GitHub Copilot
```bash
# Pegar el contenido en:
.github/copilot-instructions.md
```

### Opción 3 — Cursor
```
Settings → Rules for AI → pegar el contenido
```

### Opción 4 — ChatGPT
Crear un Custom GPT y pegar el contenido como instrucciones del sistema.

### Opción 5 — Windsurf / Cline
```bash
# Pegar en:
.windsurfrules   # Windsurf
.clinerules      # Cline
```

---

## Ejemplo de sesión

```
Usuario: Hola

ProjectForge: 👋 Hola, soy ProjectForge.
              ¿En qué idioma prefieres trabajar?
              🇪🇸 Español / 🇺🇸 English

Usuario: Español

ProjectForge: ¿Con qué tipo de proyecto trabajamos hoy?
              [A] Proyecto Nuevo
              [B] Proyecto Existente
              [C] Actualizar Kit

Usuario: A

ProjectForge: [Fase 1 — Descubrimiento]
              1. ¿Cuál es el nombre del proyecto?
                 (escribe "sugerir" si quieres opciones)
              2. ¿Qué problema resuelve?
              ...

[Después de 4 fases y confirmación]

ProjectForge: ✅ Generando kit completo...
              → documents/AGENT_PROMPT.md
              → documents/AI_GUIDELINES.md
              → documents/PROJECT_SUMMARY.md
              → documents/MEMORY_LOG.md
              → documents/TEAM_SKILLS.md
              → GETTING_STARTED.md
              → system-prompt.txt
```

---

## Stack agnóstico

ProjectForge adapta el kit generado al stack del proyecto. Ha sido probado con:

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
├── AGENT_PROMPT.md    ← El system prompt de ProjectForge (este archivo)
├── README.md          ← Esta guía
└── examples/
    └── urvi/          ← Ejemplo completo de kit generado
        ├── documents/
        │   ├── AGENT_PROMPT.md
        │   ├── AI_GUIDELINES.md
        │   ├── PROJECT_SUMMARY.md
        │   ├── MEMORY_LOG.md
        │   └── TEAM_SKILLS.md
        ├── GETTING_STARTED.md
        └── system-prompt.txt
```

---

## Contribuir

¿Tienes mejoras para el flujo de entrevista, nuevos stacks o casos de uso?

1. Fork del repo
2. Crea una rama: `git checkout -b feature/mejora-nombre`
3. Documenta el cambio en el PR con el caso de uso que resuelve
4. Abre el PR contra `main`

---

## Licencia

MIT — libre para uso personal y comercial.

---

> Creado con [Claude](https://claude.ai) · Versión 2.0 · Marzo 2026

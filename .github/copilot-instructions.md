# AppEnfermeria — Reglas Globales (GitHub Copilot)

## Estado del proyecto

Este proyecto está en **fase de descubrimiento**. Todavía no existe código ni arquitectura definida. Los artefactos actuales son:

- [`Requirements-Context.md`](../Requirements-Context.md) — contexto y reglas de negocio de alto nivel (fuente de verdad inicial del producto).
- `HU-*.md` (raíz del repo) — historias de usuario individuales ya redactadas.
- `specs/context/` — contexto consolidado que se construye y mantiene con los agentes de este kit (visión, personas, glosario, modelo de dominio, reglas de negocio indexadas, arquitectura).
- `specs/active/` y `specs/completed/` — especificación, plan y tareas de implementación por historia de usuario (solo aplican una vez exista arquitectura definida).

**No muevas ni renombres** `Requirements-Context.md` ni los `HU-*.md` existentes en la raíz — son la fuente de verdad del negocio y deben permanecer ahí.

## Flujo recomendado (de idea a implementación)

1. **Idea Shaper** — ayuda a aterrizar visión, problema, usuarios objetivo, propuesta de valor y alcance de MVP → `specs/context/vision.md`.
2. **Context Builder** — consolida `Requirements-Context.md` + todas las `HU-*.md` en `specs/context/` (glosario, personas, modelo de dominio, índice de reglas de negocio).
3. **User Story Completer** — compara el contexto consolidado contra las `HU-*.md` existentes, detecta huecos del backlog y redacta las historias faltantes con el mismo formato (`HU-XX-slug.md` en la raíz).
4. **Architecture Definer** — con el backlog razonablemente completo, propone y documenta la arquitectura (stack, capas, patrones de datos, decisiones clave) en `specs/context/architecture/` mediante ADRs, y deja lista la base de `.github/instructions/` para el stack elegido.
5. A partir de ahí, por cada historia de usuario: **Spec Builder → Plan Builder → Task Definer → Orchestrator** (que delega en los implementadores de capa y en TDD Implementer), cerrando con **Coverage Analyzer**, **Doc Updater** y **PR Builder**.

## Reglas Generales

- **Idioma:** todo el contenido (historias, specs, planes, tareas, ADRs, comentarios de negocio) en **español**.
- **Nunca inventes alcance.** Si falta información para tomar una decisión, pregunta explícitamente al developer (usa `vscode/askQuestions` cuando esté disponible) en vez de asumir.
- **File-first.** Todo agente que produzca un documento debe **guardarlo en disco primero** y luego presentar un resumen en el chat — nunca al revés.
- **Aprobación explícita.** Ningún agente avanza a la siguiente fase sin aprobación explícita del developer sobre el artefacto producido. Cada documento lleva un campo `status` en su frontmatter (`borrador` → `aprobado`/`aprobadas`) que refleja este estado.
- **Sin stack asumido.** Ningún agente de implementación (Domain/Application/Infrastructure/Api/Shared/TDD Implementer) debe asumir lenguaje, framework o base de datos hasta que **Architecture Definer** los documente en `specs/context/architecture/`. Si se les invoca antes de eso, deben detenerse e indicar que se debe correr Architecture Definer primero.
- **Contexto acumulado, no duplicado.** Cada documento nuevo (spec → plan → tareas) copia íntegramente la sección de contexto técnico del documento anterior en su propia sección "Contexto Técnico Acumulado", para que el siguiente agente sea autocontenido y no tenga que releer documentos previos.

## Estructura de carpetas

```
/
├── Requirements-Context.md        # Contexto y reglas de negocio (raíz, no mover)
├── HU-*.md                        # Historias de usuario (raíz, no mover)
├── specs/
│   ├── context/                   # Contexto consolidado (Context Builder)
│   │   ├── vision.md
│   │   ├── personas.md
│   │   ├── glossary.md
│   │   ├── domain-model.md
│   │   ├── business-rules-index.md
│   │   └── architecture/          # ADRs (Architecture Definer)
│   ├── active/{ID}-{slug}/        # specification.md, plan.md, tasks.md en curso
│   ├── completed/{ID}-{slug}/     # Historias ya implementadas
│   ├── qa-reports/                # Informes QA generados
│   └── templates/                 # Plantillas de todos los documentos
└── .github/
    ├── agents/                    # Este kit de agentes
    ├── prompts/                   # Slash commands
    ├── instructions/              # Convenciones (se completan tras Architecture Definer)
    └── skills/
```

## Agentes disponibles

Ver `.github/agents/`. Resumen por etapa:

| Etapa | Agentes |
|---|---|
| Descubrimiento | Idea Shaper, Context Builder, User Story Completer |
| Arquitectura | Architecture Definer |
| Especificación | Spec Builder, Plan Builder, Task Definer |
| Implementación | Orchestrator, Domain/Application/Infrastructure/Api/Shared Implementer, TDD Implementer, Codebase Explorer |
| Calidad y cierre | Coverage Analyzer, Doc Updater, PBI Doc Builder, QA Report Builder, API Integration Doc Builder, Tech Annex Writer, PR Builder |

---
name: adr
description: Cómo redactar Architecture Decision Records (ADRs) consistentes para las decisiones técnicas del proyecto.
---

# Skill — Architecture Decision Records (ADR)

## Cuándo crear un ADR

Cualquier decisión técnica que sea difícil o costosa de revertir: elección de lenguaje/framework, motor de base de datos, estrategia de autenticación, patrón de arquitectura, proveedor de infraestructura, estrategia de mensajería/integración externa.

No crear un ADR para decisiones triviales o fácilmente reversibles (nombre de una variable, formato de un archivo interno, etc.).

## Ubicación y numeración

`specs/context/architecture/ADR-{NN}-{slug}.md`, numerados secuencialmente empezando en `01`. Usa la [plantilla de ADR](../../../specs/templates/adr-template.md).

## Ciclo de vida

- `propuesta` — en discusión, todavía no aplica al código.
- `aceptada` — vigente, el resto de agentes debe respetarla.
- `reemplazada por ADR-{NN}` — ya no aplica; el ADR nuevo la sustituye. Nunca se borra un ADR antiguo, solo se marca como reemplazado.

## Buenas prácticas

- Una decisión por ADR. Si una decisión tiene múltiples facetas independientes, divide en varios ADRs enlazados.
- Sección "Alternativas consideradas" es obligatoria — un ADR sin alternativas evaluadas generalmente esconde una decisión no analizada.
- Referencia siempre la necesidad de negocio/HU que motivó la decisión cuando aplique.

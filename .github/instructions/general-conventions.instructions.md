---
applyTo: "**"
description: Convenciones generales mínimas mientras no exista arquitectura definida. Architecture Definer debe reemplazar/complementar este archivo con instructions específicas del stack elegido.
---

# Convenciones Generales (provisional)

Este archivo aplica mientras el proyecto no tiene arquitectura definida. Cuando **Architecture Definer** corra, debe agregar archivos de instructions específicos del stack (equivalentes a convenciones de lenguaje, capas, API, testing, etc.) y actualizar `.github/copilot-instructions.md` con la sección "Stack Técnico".

## Reglas mientras tanto

- Todo contenido de negocio (historias, specs, reglas, comentarios) en **español**.
- No se debe generar código de aplicación antes de que exista al menos un ADR de arquitectura aceptado en `specs/context/architecture/`.
- Cualquier agente de implementación que sea invocado sin arquitectura definida debe detenerse y solicitar correr `Architecture Definer` primero.
- Nunca hardcodear secretos, credenciales o tokens en ningún archivo, sin importar el stack que se elija.
- Nunca commitear archivos de configuración con datos sensibles reales (usar ejemplos/placeholders).

---
id: "{ID}"
historia: "HU-{ID}"
status: "borrador"
---

# Especificación — {Título de la historia}

## 1. Historia de usuario relacionada

Referencia a [`HU-{ID}-{slug}.md`](../../../HU-{ID}-{slug}.md) (ruta relativa a la raíz del repo).

## 2. Resumen ejecutivo

{2-4 líneas: qué se va a construir y por qué, en lenguaje de negocio.}

## 3. Contexto técnico descubierto

> Se completa SOLO si ya existe arquitectura definida (`specs/context/architecture/`). Organizar por capa/módulo según la arquitectura vigente. Si no hay arquitectura aún, dejar esta sección vacía y detener el flujo — se necesita correr Architecture Definer primero.

## 4. Requisitos funcionales

| ID | Requisito |
|---|---|
| RF-01 | |

## 5. Reglas de negocio

| ID | Regla |
|---|---|
| RN-01 | |

## 6. Modelo de datos

| Entidad | Campo | Tipo | Obligatorio | Notas |
|---|---|---|---|---|

## 7. Casos de uso / Endpoints

| Acción | Disparador (endpoint, evento, comando) | Entrada | Salida | Códigos/Estados posibles |
|---|---|---|---|---|

## 8. Validaciones

| Campo | Regla | Mensaje |
|---|---|---|

## 9. Autorización

Roles/permisos requeridos por caso de uso.

## 10. Dependencias

- Internas: {otras historias/módulos}
- Externas: {servicios de terceros, si aplica}

## 11. Criterios de aceptación (Gherkin)

```gherkin
Escenario: 
  Dado 
  Cuando 
  Entonces 
```

## 12. Requisitos no funcionales

- Rendimiento:
- Seguridad:
- Auditoría:
- Disponibilidad:

# HU-04 — Gestión de Tipos de Servicio

## Identificación

- **ID:** HU-04

- **Nombre:** Gestión de tipos de servicio

- **Actor principal:** Superadministrador

- **Prioridad:** Media

- **Estado:** Definida para MVP

## Historia de Usuario

> Como superadministrador, quiero gestionar los tipos de servicio disponibles en la plataforma, para definir las opciones de atención que podrán seleccionar los usuarios al momento de solicitar un servicio de enfermería.

## Descripción

El sistema deberá manejar un catálogo de tipos de servicio que permita identificar la modalidad de atención solicitada por el usuario.

Para el MVP se definirán inicialmente los siguientes tipos de servicio:

- Atención domiciliaria
- Acompañamiento
- Acompañamiento y transporte
- Atención hospitalaria

Los tipos de servicio deberán almacenarse como entidades independientes y podrán ser asociados a las solicitudes de servicio.

El superadministrador podrá crear nuevos tipos de servicio y editar el nombre y la descripción de los existentes desde su módulo administrativo, además de gestionar su estado activo/inactivo.

## Tipos de servicio iniciales

### Atención domiciliaria

Servicio de atención de enfermería realizado en el domicilio del usuario o paciente.

### Acompañamiento

Servicio de acompañamiento y asistencia al usuario o paciente.

### Acompañamiento y transporte

Servicio que incluye acompañamiento y transporte de acuerdo con las condiciones definidas en la solicitud.

### Atención hospitalaria

Servicio de atención y acompañamiento de enfermería dentro de una institución hospitalaria, contratado directamente por el usuario (sin convenio institucional).

> **Aclaración de alcance (INC-07, resuelta 2026-09-29):** este tipo de servicio sí está en el MVP. Es distinto de la "atención hospitalaria mediante convenio formal con una IPS" (integración administrativa/facturación con la institución), que `specs/context/vision.md` §5 excluye explícitamente del MVP por requerir due diligence legal aparte.

## Datos del tipo de servicio

Cada tipo de servicio deberá manejar como mínimo:

- Identificador único.
- Nombre.
- Descripción.
- Estado.
- Fecha y hora de creación.
- Fecha y hora de actualización.

## Estado del tipo de servicio

El tipo de servicio deberá contar con un estado que permita determinar si se encuentra disponible para nuevas solicitudes.

Los estados mínimos serán:

- Activo.
- Inactivo.

Un tipo de servicio inactivo no deberá aparecer como opción disponible para nuevas solicitudes.

## Reglas de negocio

### RN-01 — Catálogo inicial

El sistema deberá contar inicialmente con los cuatro tipos de servicio definidos para el MVP:

- Atención domiciliaria.
- Acompañamiento.
- Acompañamiento y transporte.
- Atención hospitalaria.

### RN-02 — Identificador único

Cada tipo de servicio deberá contar con un identificador único dentro del sistema.

### RN-03 — Nombre único

No deberán existir dos tipos de servicio activos con el mismo nombre.

### RN-04 — Servicios activos

Solamente los tipos de servicio en estado activo podrán ser seleccionados para nuevas solicitudes.

### RN-05 — Servicios inactivos

La desactivación de un tipo de servicio no deberá eliminar los registros históricos asociados a dicho servicio.

### RN-06 — Integridad histórica

Las solicitudes creadas anteriormente deberán conservar la referencia al tipo de servicio utilizado, incluso si posteriormente dicho tipo de servicio es marcado como inactivo.

### RN-07 — Extensibilidad

La estructura deberá permitir agregar nuevos tipos de servicio posteriormente desde el módulo del superadministrador.

### RN-08 — Roles

La gestión de tipos de servicio estará disponible únicamente para el superadministrador.

### RN-09 — Creación y edición

El superadministrador podrá crear nuevos tipos de servicio y editar el nombre y la descripción de los existentes, además de gestionar su estado.

## Flujo principal

1. El superadministrador ingresa al módulo de gestión de tipos de servicio.
2. El sistema muestra los tipos de servicio configurados.
3. El superadministrador puede consultar la información de cada tipo de servicio.
4. El sistema muestra como mínimo el nombre, descripción y estado.
5. El superadministrador puede crear un nuevo tipo de servicio proporcionando nombre y descripción.
6. El superadministrador puede editar el nombre y la descripción de un tipo de servicio existente.
7. El superadministrador puede gestionar el estado del tipo de servicio.
8. El sistema valida las reglas de negocio correspondientes.
9. Si el tipo de servicio es activado, queda disponible para nuevas solicitudes.
10. Si el tipo de servicio es desactivado, deja de estar disponible para nuevas solicitudes.
11. Los registros históricos asociados al tipo de servicio permanecen intactos.

## Flujos alternativos

### Intento de activar un tipo de servicio inválido

Si el sistema detecta que la información del tipo de servicio no cumple las reglas definidas:

- El sistema deberá rechazar la operación.
- El sistema deberá informar al superadministrador el motivo del rechazo.

### Tipo de servicio inactivo

Si un usuario intenta crear una solicitud y el tipo de servicio se encuentra inactivo:

- El tipo de servicio no deberá aparecer entre las opciones disponibles.
- El sistema deberá impedir que se cree una nueva solicitud utilizando dicho tipo de servicio.

### Tipo de servicio utilizado históricamente

Si un tipo de servicio es desactivado:

- Las solicitudes existentes no deberán modificarse.
- El tipo de servicio deberá continuar siendo identificable en los registros históricos.

## Criterios de aceptación

- [ ] El sistema cuenta con los cuatro tipos de servicio definidos para el MVP.
- [ ] Cada tipo de servicio posee un identificador único.
- [ ] Cada tipo de servicio posee nombre y descripción.
- [ ] Cada tipo de servicio posee un estado activo/inactivo.
- [ ] Los tipos de servicio activos pueden ser utilizados en nuevas solicitudes.
- [ ] Los tipos de servicio inactivos no pueden ser utilizados en nuevas solicitudes.
- [ ] Un tipo de servicio inactivo no aparece entre las opciones disponibles para el usuario.
- [ ] La desactivación de un tipo de servicio no elimina su información.
- [ ] Las solicitudes históricas conservan la referencia al tipo de servicio utilizado.
- [ ] Solamente el superadministrador puede gestionar los tipos de servicio.
- [ ] La estructura permite agregar nuevos tipos de servicio posteriormente.
- [ ] El superadministrador puede crear un nuevo tipo de servicio con nombre y descripción.
- [ ] El superadministrador puede editar el nombre y la descripción de un tipo de servicio existente.
- [ ] Las operaciones de gestión generan información de auditoría.

## Auditoría

Las operaciones realizadas sobre los tipos de servicio deberán generar información de auditoría, incluyendo como mínimo:

- Fecha y hora de creación.
- Usuario/identificador de creación.
- Fecha y hora de actualización.
- Usuario/identificador de actualización.
- Estado actual.
- Cambios realizados, cuando aplique.

## Consideraciones de modelo de datos

Los tipos de servicio deberán manejarse mediante una entidad independiente, evitando almacenar directamente el nombre del servicio dentro de la entidad `Servicio`.

La relación propuesta es:

```text
TipoServicio
    |
    | 1:N
    |
Servicio
```

Un tipo de servicio puede estar asociado a múltiples servicios, mientras que cada servicio deberá tener un único tipo de servicio.

La referencia deberá realizarse mediante el identificador de TipoServicio.

## Fuera de alcance para esta historia

- Creación de solicitudes de servicio.
- Asignación de enfermeros.
- Definición de tarifas.
- Cálculo del precio del servicio.
- Definición de duración del servicio.
- Disponibilidad de enfermeros.
- Pagos.
- Calificaciones.
- Chat.
- Inicio y finalización de servicios.
- Configuración avanzada de los tipos de servicio.

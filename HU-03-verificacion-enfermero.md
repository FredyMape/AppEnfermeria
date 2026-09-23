# HU-03 — Verificación y Aprobación de Enfermero

## Identificación

- **ID:** HU-03
- **Nombre:** Verificación y aprobación de enfermero
- **Actor principal:** Superadministrador
- **Actor secundario:** Enfermero
- **Prioridad:** Alta
- **Estado:** Definida para MVP

## Historia de Usuario

> Como Superadministrador, quiero revisar toda la información personal, profesional y documental de un enfermero y aprobar, rechazar o solicitar correcciones, para garantizar que únicamente los enfermeros verificados puedan prestar servicios en la plataforma.

## Descripción

Cuando un enfermero completa su registro, el perfil queda en estado **Pendiente de revisión**.

El Superadministrador tendrá acceso a toda la información proporcionada por el enfermero para realizar la validación manual.

La información disponible incluirá:

- Información personal.
- Información profesional.
- Foto.
- Documentos cargados.
- Título profesional.
- Tarjeta profesional.
- Experiencia profesional.
- Certificados.
- Documentación adicional.
- Estado actual del proceso de verificación.

El Superadministrador podrá tomar una de las siguientes acciones:

1. Aprobar.
2. Rechazar.
3. Solicitar corrección.

## Información visible para el Superadministrador

El Superadministrador podrá consultar toda la información registrada por el enfermero.

Esto incluye:

### Información personal

- Nombre completo.
- Tipo de documento.
- Número de documento.
- Teléfono.
- Correo electrónico.
- Foto.
- Zona de residencia.

### Información profesional

- Título profesional.
- Tarjeta profesional.
- Experiencia.
- Lugares donde ha trabajado.
- Años de experiencia.
- Certificaciones.
- Documentación adicional.

### Documentos

El Superadministrador podrá consultar y revisar los documentos cargados como parte del proceso de registro.

## Estados de verificación

### Pendiente de revisión

Estado inicial después de completar el registro.

El enfermero está esperando la revisión manual.

No puede aceptar servicios.

### Aprobado

Indica que el Superadministrador verificó satisfactoriamente la información requerida.

El enfermero queda habilitado para aceptar servicios.

### Rechazado

Indica que la información proporcionada no fue aprobada durante la revisión.

El motivo deberá quedar registrado mediante un comentario.

El enfermero podrá corregir la información indicada y solicitar una nueva revisión.

### Corrección solicitada

El Superadministrador requiere modificaciones o información adicional antes de aprobar el perfil.

El comentario del Superadministrador deberá indicar qué información debe corregirse o complementarse.

## Flujo principal — Aprobación

1. El Superadministrador ingresa al módulo de verificación.
2. El sistema muestra los enfermeros pendientes de revisión.
3. El Superadministrador selecciona un enfermero.
4. El sistema muestra toda la información registrada.
5. El Superadministrador revisa la información personal.
6. El Superadministrador revisa los documentos.
7. El Superadministrador revisa la información profesional.
8. El Superadministrador revisa la experiencia y documentación adicional.
9. El Superadministrador selecciona **Aprobar**.
10. El sistema cambia el estado del enfermero a **Aprobado**.
11. El sistema registra quién realizó la aprobación.
12. El sistema registra la fecha y hora de aprobación.
13. El sistema notifica al enfermero que su perfil fue aprobado.
14. El enfermero queda habilitado para aceptar servicios.

## Flujo alternativo — Corrección solicitada

1. El Superadministrador revisa el perfil.
2. Detecta información incompleta, inconsistente o que requiere verificación adicional.
3. Selecciona **Solicitar corrección**.
4. El Superadministrador registra un comentario explicando qué debe corregirse o complementarse.
5. El sistema cambia el estado a **Corrección solicitada**.
6. El sistema notifica al enfermero.
7. El enfermero corrige la información solicitada.
8. El enfermero reenvía el perfil para revisión.
9. El sistema cambia nuevamente el estado a **Pendiente de revisión**.
10. El Superadministrador puede realizar una nueva revisión.

## Flujo alternativo — Rechazo

1. El Superadministrador revisa el perfil.
2. Determina que la información no puede ser aprobada en su estado actual.
3. Selecciona **Rechazar**.
4. El Superadministrador registra el motivo del rechazo mediante un comentario.
5. El sistema cambia el estado a **Rechazado**.
6. El sistema notifica al enfermero.
7. El enfermero puede corregir la información relacionada con el motivo del rechazo.
8. El enfermero solicita una nueva revisión.
9. El sistema vuelve a colocar el perfil en **Pendiente de revisión**.
10. El Superadministrador realiza una nueva revisión.

## Reglas de negocio

### RN-01 — Aprobación manual

La aprobación debe ser realizada manualmente por un Superadministrador.

### RN-02 — Acceso a servicios

Un enfermero solamente puede aceptar servicios cuando su perfil está en estado **Aprobado**.

### RN-03 — Información completa

El Superadministrador puede consultar toda la información proporcionada por el enfermero para realizar la validación.

### RN-04 — Registro de aprobación

Cuando un enfermero es aprobado, el sistema debe almacenar:

- Identificador del Superadministrador.
- Nombre del Superadministrador.
- Fecha de aprobación.
- Hora de aprobación.
- Estado resultante.

### RN-05 — Comentario de rechazo

Todo rechazo debe incluir un comentario que indique el motivo.

### RN-06 — Correcciones

El enfermero puede corregir la información indicada en el comentario y solicitar una nueva revisión.

### RN-07 — Nueva revisión

Después de corregir la información, el perfil vuelve al flujo de revisión del Superadministrador.

### RN-08 — Historial

Las acciones de aprobación, rechazo y solicitud de corrección deben quedar registradas para auditoría.

### RN-09 — SLA de revisión

El objetivo de revisión de una solicitud de verificación será de **24 a 36 horas**.

Este SLA representa un objetivo operativo para la revisión administrativa.

## Criterios de aceptación

- [ ] El Superadministrador puede consultar los perfiles pendientes de revisión.
- [ ] El Superadministrador puede consultar toda la información del enfermero.
- [ ] El Superadministrador puede consultar los documentos cargados.
- [ ] El Superadministrador puede aprobar un perfil.
- [ ] El sistema cambia el estado a Aprobado después de una aprobación.
- [ ] Un enfermero aprobado queda habilitado para aceptar servicios.
- [ ] El sistema registra el Superadministrador que realizó la aprobación.
- [ ] El sistema registra la fecha y hora de aprobación.
- [ ] El Superadministrador puede solicitar correcciones.
- [ ] Una solicitud de corrección permite registrar un comentario.
- [ ] El enfermero recibe una notificación cuando se solicitan correcciones.
- [ ] El enfermero puede corregir la información.
- [ ] El enfermero puede solicitar una nueva revisión.
- [ ] El Superadministrador puede rechazar un perfil.
- [ ] Un rechazo requiere registrar un comentario con el motivo.
- [ ] El enfermero recibe una notificación del rechazo.
- [ ] El enfermero puede corregir la información después de un rechazo.
- [ ] El enfermero puede solicitar una nueva revisión después de corregir la información.
- [ ] Todas las acciones relevantes quedan registradas para auditoría.
- [ ] El sistema contempla un SLA objetivo de revisión de 24 a 36 horas.

## Auditoría

Para cada acción de verificación deberá registrarse como mínimo:

- ID del enfermero.
- ID del Superadministrador.
- Nombre del Superadministrador.
- Acción realizada:
  - Aprobación.
  - Rechazo.
  - Solicitud de corrección.
- Comentario, cuando aplique.
- Estado anterior.
- Estado nuevo.
- Fecha y hora.
- Información adicional necesaria para trazabilidad.

## Notificaciones

El sistema deberá notificar al enfermero cuando:

- Su perfil sea aprobado.
- Su perfil sea rechazado.
- Se soliciten correcciones.
- Su perfil vuelva a quedar pendiente de revisión, cuando corresponda.

El canal de notificación queda inicialmente abierto a la implementación del sistema y podrá incluir notificación dentro de la aplicación y/o correo electrónico.

## Fuera de alcance para esta historia

- Aceptación de servicios.
- Asignación de servicios.
- Inicio de servicios mediante PIN.
- Finalización de servicios.
- Extensión de servicios.
- Calificación.
- Pagos.

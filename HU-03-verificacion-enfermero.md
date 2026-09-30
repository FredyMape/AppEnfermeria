# HU-03 — Verificación y Aprobación de Enfermero

## Identificación

- **ID:** HU-03
- **Nombre:** Verificación y aprobación de enfermero
- **Actor principal:** Superadministrador
- **Actor secundario:** Enfermero
- **Prioridad:** Alta
- **Estado:** Definida para MVP

## Historia de Usuario

> Como Superadministrador, quiero revisar toda la información personal, profesional y documental de un enfermero y aprobar, solicitar correcciones, rechazar de forma definitiva o suspender un perfil ya aprobado, para garantizar que únicamente los enfermeros verificados puedan prestar servicios en la plataforma.

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
- Resultado de la verificación de identidad facial (coincide / no coincide / pendiente) y número de intentos utilizados (`HU-02`).
- Estado actual del proceso de verificación.

El Superadministrador podrá tomar una de las siguientes acciones:

1. Aprobar.
2. Solicitar corrección.
3. Rechazar de forma definitiva (casos no corregibles, p. ej. documento falsificado o suplantación evidente, o tras agotar el límite de ciclos de corrección).
4. Suspender un perfil ya **Aprobado** (p. ej. por una sanción, una denuncia o la revocación del consentimiento biométrico).

> Los estados/acciones `Rechazado` y `Corrección solicitada` se unificaron el 2026-09-29 (resolución INC-16) para el caso corregible: ambos tenían el mismo comportamiento. El 2026-09-30 (resolución INC-29) se reintrodujo `Rechazado` como estado **terminal**, distinto de `Corrección solicitada`: no admite reenvío ni nueva revisión bajo el mismo registro. También se agregó `Suspendido` para revocar perfiles ya aprobados, inexistente hasta ahora.

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

### Corrección solicitada

El Superadministrador requiere modificaciones o información adicional antes de aprobar el perfil.

El comentario del Superadministrador deberá indicar qué información debe corregirse o complementarse. El comentario es obligatorio.

### Rechazado (terminal)

Indica que el perfil no podrá continuar en el proceso de verificación bajo el mismo registro: por una causal grave (documento falsificado, suplantación evidente) o por haber agotado el límite de 3 ciclos de solicitud de corrección (RN-10, valor por defecto sujeto a ajuste).

El motivo deberá quedar registrado mediante un comentario obligatorio. No admite reenvío ni nueva revisión. Qué puede hacer el enfermero después (p. ej. crear un nuevo registro) queda como vacío para una historia futura.

### Suspendido

Un perfil previamente **Aprobado** puede pasar a `Suspendido` cuando el Superadministrador identifica una causal (sanción, denuncia, revocación del consentimiento biométrico, vigencia vencida de la tarjeta profesional, etc.).

Un enfermero `Suspendido` no puede aceptar nuevos servicios. El mecanismo de reactivación queda como vacío para una historia futura.

## Flujo principal — Aprobación

1. El Superadministrador ingresa al módulo de verificación.
2. El sistema muestra los enfermeros pendientes de revisión.
3. El Superadministrador selecciona un enfermero.
4. El sistema muestra toda la información registrada.
5. El Superadministrador revisa la información personal.
6. El Superadministrador revisa los documentos.
7. El Superadministrador revisa la información profesional.
8. El Superadministrador revisa la experiencia y documentación adicional.
9. El Superadministrador revisa el resultado de la verificación de identidad facial; si no coincide, puede habilitar intentos adicionales para que el enfermero repita la captura (ver `HU-02`) en lugar de decidir de inmediato.
10. El Superadministrador selecciona **Aprobar**.
11. El sistema cambia el estado del enfermero a **Aprobado**.
12. El sistema registra quién realizó la aprobación.
13. El sistema registra la fecha y hora de aprobación.
14. El sistema notifica al enfermero que su perfil fue aprobado.
15. El enfermero queda habilitado para aceptar servicios.

## Flujo alternativo — Corrección solicitada

1. El Superadministrador revisa el perfil.
2. Detecta información incompleta, inconsistente o que requiere verificación adicional (incluida una verificación facial sin coincidencia).
3. Selecciona **Solicitar corrección**.
4. El Superadministrador registra un comentario explicando qué debe corregirse o complementarse.
5. El sistema cambia el estado a **Corrección solicitada** e incrementa el contador de ciclos de corrección del perfil.
6. El sistema notifica al enfermero.
7. El enfermero corrige la información solicitada.
8. El enfermero reenvía el perfil para revisión.
9. El sistema cambia nuevamente el estado a **Pendiente de revisión**.
10. El Superadministrador puede realizar una nueva revisión.

> Al alcanzar 3 ciclos de corrección sin llegar a **Aprobado** (RN-10), el Superadministrador debe decidir entre aprobar o rechazar de forma definitiva; no se permite un cuarto ciclo de corrección.

## Flujo alternativo — Rechazo definitivo

1. El Superadministrador revisa el perfil.
2. Determina que no puede aprobarse bajo el mismo registro (causal grave, o se agotó el límite de ciclos de corrección).
3. Selecciona **Rechazar**.
4. El Superadministrador registra un comentario obligatorio con el motivo/causal.
5. El sistema cambia el estado a **Rechazado** (terminal).
6. El sistema notifica al enfermero.
7. El perfil no admite reenvío ni nueva revisión.

## Flujo alternativo — Suspensión de un perfil aprobado

1. El Superadministrador identifica una causal sobre un perfil en estado **Aprobado**.
2. Selecciona **Suspender**.
3. El Superadministrador registra un comentario obligatorio con la causal.
4. El sistema cambia el estado a **Suspendido**.
5. El enfermero deja de poder aceptar nuevos servicios.
6. El sistema notifica al enfermero.
7. El mecanismo de reactivación queda como vacío para una historia futura.

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

### RN-05 — Comentario de corrección

Toda solicitud de corrección debe incluir un comentario que indique el motivo.

### RN-06 — Correcciones

El enfermero puede corregir la información indicada en el comentario y solicitar una nueva revisión.

### RN-07 — Nueva revisión

Después de corregir la información, el perfil vuelve al flujo de revisión del Superadministrador.

### RN-08 — Historial

Las acciones de aprobación y solicitud de corrección deben quedar registradas para auditoría.

### RN-09 — SLA de revisión

El objetivo de revisión de una solicitud de verificación será de **24 a 36 horas**.

Este SLA representa un objetivo operativo para la revisión administrativa.

### RN-10 — Límite de ciclos de corrección

Un perfil admite como máximo 3 ciclos de solicitud de corrección (valor por defecto, resolución INC-29, 2026-09-30). Al alcanzar el límite, el Superadministrador debe aprobar o rechazar de forma definitiva.

### RN-11 — Rechazo definitivo

Un perfil `Rechazado` no admite reenvío ni nueva revisión bajo el mismo registro. Todo rechazo definitivo debe incluir un comentario obligatorio con la causal.

### RN-12 — Suspensión de perfiles aprobados

Un perfil `Aprobado` puede pasar a `Suspendido` por decisión del Superadministrador, con un comentario obligatorio indicando la causal. Un enfermero `Suspendido` no puede aceptar nuevos servicios.

### RN-13 — Verificación facial en la revisión

El resultado de la verificación facial (`HU-02`) es un insumo obligatorio para la decisión del Superadministrador, quien puede aprobar, solicitar corrección (para repetir la captura) o rechazar considerándolo junto con el resto de la información.

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
- [ ] Al alcanzar 3 ciclos de corrección, el sistema exige aprobar o rechazar de forma definitiva.
- [ ] El Superadministrador puede rechazar un perfil de forma definitiva, con comentario obligatorio, sin admitir reenvío.
- [ ] El Superadministrador puede suspender un perfil ya aprobado, con comentario obligatorio.
- [ ] Un enfermero suspendido no puede aceptar nuevos servicios.
- [ ] El Superadministrador puede consultar el resultado de la verificación facial y habilitar intentos adicionales.
- [ ] Todas las acciones relevantes quedan registradas para auditoría.
- [ ] El sistema contempla un SLA objetivo de revisión de 24 a 36 horas.

## Auditoría

Para cada acción de verificación deberá registrarse como mínimo:

- ID del enfermero.
- ID del Superadministrador.
- Nombre del Superadministrador.
- Acción realizada:
  - Aprobación.
  - Solicitud de corrección.
  - Rechazo definitivo.
  - Suspensión.
- Comentario, cuando aplique.
- Estado anterior.
- Estado nuevo.
- Fecha y hora.
- Información adicional necesaria para trazabilidad.

## Notificaciones

El sistema deberá notificar al enfermero cuando:

- Su perfil sea aprobado.
- Se soliciten correcciones.
- Su perfil sea rechazado de forma definitiva.
- Su perfil sea suspendido.
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
- Reactivación de un perfil `Suspendido` o reingreso tras un `Rechazado` definitivo (queda como vacío para una historia futura).

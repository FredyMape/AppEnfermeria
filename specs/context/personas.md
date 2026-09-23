# Personas / Actores — AppEnfermeria

> Generado por Context Builder. Re-ejecutable. Última consolidación: 2026-09-22.
> Fuentes: `Requirements-Context.md`, `HU-01`, `HU-02`, `HU-03`, `HU-04`.

---

## Usuario / Cliente

**Definido en:** `Requirements-Context.md` §4; `HU-01`.

### Responsabilidades

- Crear una cuenta con datos personales mínimos y contraseña.
- Verificar su correo electrónico mediante código de 6 dígitos.
- Registrar información médica propia (opcionalmente) durante el registro.
- Registrar datos de un tercero al solicitar un servicio para otra persona (futuro, HU pendiente).
- Calificar al enfermero tras finalizar el servicio (futuro, HU pendiente).
- Consultar y dictar el PIN al enfermero para iniciar el servicio (futuro, HU pendiente).

### Datos que gestiona

- Datos de `Persona`: nombre completo, tipo/número de documento, teléfono, correo, contraseña.
- Datos médicos propios: tipo de sangre, RH, información médica relevante (sin catálogo definido).
- Datos médicos/personales de terceros cuando solicita para otra persona.

### HUs que ya lo cubren

- `HU-01` — Registro de Usuario (registro, verificación de correo, recordatorio).

### Necesidades sin HU todavía

- Solicitar, publicar y dar seguimiento a un servicio (`HU-05` a `HU-09`, ya listadas como pendientes en `Requirements-Context.md` §29).
- Iniciar el servicio mediante PIN, extenderlo, finalizarlo y calificar (`HU-10` a `HU-13`, pendientes en §29).
- **Recuperación de contraseña** — no existe ni está mencionada en la lista de pendientes §29. HU-01 crea contraseñas sin que exista ningún mecanismo de recuperación en todo el corpus.
- **Cancelación de un servicio por el usuario** — el estado `Cancelado` existe en los diagramas (§8.2, §26) pero sus reglas se remiten a "la historia de usuario correspondiente" (§26.1), inexistente incluso en la lista de pendientes.
- **Cambio de correo antes de verificar** — no contemplado si el usuario comete un error tipográfico al registrarse.
- **Eliminación/supresión de cuenta** — no mencionado en ningún documento.

---

## Enfermero

**Definido en:** `Requirements-Context.md` §5; `HU-02`; actor secundario en `HU-03`.

### Responsabilidades

- Registrarse seleccionando el rol Enfermero, aportando información personal y profesional.
- Cargar los cuatro documentos obligatorios (documento de identidad, foto, título profesional, tarjeta profesional) y, opcionalmente, documentación adicional.
- Esperar la revisión manual del Superadministrador (`HU-03`).
- Corregir información/documentación cuando se le solicite y reenviar a revisión.
- Una vez aprobado: aceptar servicios, iniciar mediante PIN, solicitar extensiones y calificar al usuario (funcionalidades futuras, HU pendientes).

### Datos que gestiona

- Datos de `Persona` (compartidos con Usuario).
- Datos profesionales: título, tarjeta profesional, años y descripción de experiencia, lugares donde ha trabajado.
- Documentos: identidad, foto, título, tarjeta profesional, certificaciones, documentación adicional opcional.
- Zona de residencia (sin radio de servicio).

### HUs que ya lo cubren

- `HU-02` — Registro y creación del perfil de enfermero.
- `HU-03` — Verificación y aprobación (como sujeto de la revisión).

### Necesidades sin HU todavía

- Aceptar, iniciar, finalizar, extender y calificar servicios (`HU-08` a `HU-13`, pendientes en §29).
- **Recuperación de contraseña** — mismo vacío que para Usuario; no existe en ningún documento.
- **Revocación o suspensión tras la aprobación** — `Requirements-Context.md` y `HU-03` solo contemplan `Pendiente de revisión`, `Aprobado`, `Rechazado`/`Corrección solicitada`. No existe estado ni flujo para retirar la aprobación a un enfermero ya habilitado (p. ej. tras una sanción). No está mencionado ni siquiera en la lista de pendientes §29.
- **Apelación de un rechazo definitivo** — no contemplada.
- **Estado de borrador / reanudación del registro** — el flujo de `HU-02` es lineal sin persistencia parcial declarada.
- **Verificación de identidad** (más allá de la verificación de credenciales documentales) — no mencionada en ningún documento; el registro exige documentos autoportados sin mecanismo de contraste declarado.

---

## Superadministrador

**Definido en:** `Requirements-Context.md` §22; actor principal en `HU-03` y `HU-04`.

### Responsabilidades

- Revisar manualmente perfiles de enfermeros pendientes (`HU-03`): aprobar, rechazar o solicitar corrección.
- Configurar el tiempo de habilitación de datos médicos sensibles y del PIN (`Requirements-Context.md` §23).
- Gestionar el catálogo de tipos de servicio (`HU-04`): consultar y cambiar estado activo/inactivo.
- Consultar información de auditoría (`Requirements-Context.md` §22).

### Datos que gestiona

- Decisiones de aprobación/rechazo/corrección sobre perfiles de enfermero, con comentario asociado.
- Configuración global: minutos de habilitación de datos sensibles, minutos de habilitación del PIN.
- Catálogo `TipoServicio`: nombre, descripción, estado.

### HUs que ya lo cubren

- `HU-03` — Verificación y aprobación de enfermero.
- `HU-04` — Gestión de tipos de servicio.

### Necesidades sin HU todavía

- **Gestión administrativa general y configuraciones** (`HU-15`, pendiente en §29) — incluye la configuración de las ventanas de tiempo mencionadas en `Requirements-Context.md` §13/§23, que hoy no tiene una historia propia.
- **Consulta de auditoría** (`HU-16`, pendiente en §29).
- **Desbloqueo del servicio tras 5 intentos fallidos de PIN** — `Requirements-Context.md` §15 dice textualmente que "el mecanismo exacto para desbloquear el servicio... queda pendiente de definición", y no aparece en la lista de historias pendientes §29. Es un vacío no planificado.
- **Revocación/suspensión de enfermeros aprobados** — ver sección Enfermero arriba; el Superadministrador no tiene ninguna acción definida para esto.
- **Segregación de funciones / doble autorización** — ninguna regla del corpus impide que el Superadministrador apruebe su propio perfil si también se registra como enfermero, ni exige doble autorización para acciones sensibles (p. ej. desactivar el tipo de servicio principal).

---

## Vacíos detectados (transversal a personas)

Estas funcionalidades se mencionan en `Requirements-Context.md` pero no tienen ninguna HU asociada, ni siquiera en la lista de pendientes `§29` (HU-05 a HU-17). **No se redactan aquí historias nuevas — esta lista es la entrada principal para User Story Completer:**

- Recuperación de contraseña para Usuario y Enfermero (creación de credenciales sin ningún mecanismo de recuperación en todo el corpus).
- Cancelación de un servicio (el estado `CANCELADO` existe en los diagramas §8.2/§26 sin reglas de transición propias; el texto remite a "la historia de usuario correspondiente", que no existe).
- Desbloqueo del servicio tras agotar los 5 intentos de PIN (§15, explícitamente "pendiente de definición").
- Revocación o suspensión de un enfermero ya aprobado (sin estado ni flujo en `HU-03` ni en ningún otro documento).
- Cambio de correo electrónico antes de completar la verificación.
- Gestión/gestión administrativa de las configuraciones de ventanas de tiempo (§13/§23) — mencionada como capacidad del Superadministrador en §22 pero sin HU propia; podría quedar cubierta por la futura `HU-15` si su alcance se define para incluirla explícitamente.

> Nota de trazabilidad: `Auditoria-Especificacion.md` (documento de auditoría externa, no de negocio) profundiza sobre estos mismos vacíos y añade otros no derivados directamente del texto de `Requirements-Context.md` (p. ej. verificación de identidad biométrica, integración con ReTHUS, modelo de pagos, disputas). Esos hallazgos adicionales son recomendaciones de un análisis externo y **no están respaldados todavía por el texto de negocio**; se dejan fuera de este documento a la espera de que el developer decida incorporarlos como HUs mediante `User Story Completer`.

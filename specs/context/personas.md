# Personas / Actores — AppEnfermeria

> Generado por Context Builder. Re-ejecutable. Última consolidación: 2026-09-30 (refinamiento a partir de `Hallazgos-Auditoria-AppEnfermeria.md`/`Sugerencias-Auditoria-AppEnfermeria.md`, ver `business-rules-index.md` CN-10 a CN-31).
> Fuentes: `Requirements-Context.md`, `HU-01`, `HU-02`, `HU-03`, `HU-04`, `specs/context/vision.md`.

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

- Datos de `Persona` (compartidos con Usuario), incluidos contraseña y verificación de correo (resolución INC-06, 2026-09-30).
- Datos profesionales: título, tarjeta profesional, años y descripción de experiencia, lugares donde ha trabajado.
- Documentos: identidad, foto (usada también para verificación facial), título, tarjeta profesional, certificaciones, documentación adicional opcional.
- Zona de residencia (sin radio de servicio).
- Resultado de la verificación de identidad facial y consentimiento biométrico separado (resolución INC-04/INC-05, 2026-09-30).

### HUs que ya lo cubren

- `HU-02` — Registro y creación del perfil de enfermero (incluye credenciales, verificación facial y consentimientos desde el 2026-09-30).
- `HU-03` — Verificación y aprobación (como sujeto de la revisión; incluye rechazo definitivo y suspensión desde el 2026-09-30).

### Necesidades sin HU todavía

- Aceptar, iniciar, finalizar, extender y calificar servicios (`HU-08` a `HU-13`, pendientes en §29).
- **Reactivación de un perfil suspendido** — `HU-03` define cómo se suspende un perfil aprobado, pero no cómo se reactiva (resolución INC-29, 2026-09-30).
- **Apelación de un rechazo definitivo** — `HU-03` no contempla ninguna vía de apelación tras un rechazo terminal.
- **Estado de borrador / reanudación del registro** — el flujo de `HU-02` es lineal sin persistencia parcial declarada.

---

## Superadministrador

**Definido en:** `Requirements-Context.md` §22; actor principal en `HU-03` y `HU-04`.

### Responsabilidades

- Revisar manualmente perfiles de enfermeros pendientes (`HU-03`): aprobar o solicitar corrección.
- Configurar el tiempo de habilitación de datos médicos sensibles y del PIN (`Requirements-Context.md` §23).
- Gestionar el catálogo de tipos de servicio (`HU-04`): crear, editar nombre/descripción y cambiar estado activo/inactivo (CRUD completo, `HU04-RN-09`).
- Consultar información de auditoría (`Requirements-Context.md` §22).

### Datos que gestiona

- Decisiones de aprobación/corrección sobre perfiles de enfermero, con comentario asociado.
- Configuración global: minutos de habilitación de datos sensibles, minutos de habilitación del PIN.
- Catálogo `TipoServicio`: nombre, descripción, estado.

### HUs que ya lo cubren

- `HU-03` — Verificación y aprobación de enfermero.
- `HU-04` — Gestión de tipos de servicio.

### Necesidades sin HU todavía

- **Gestión administrativa general y configuraciones** (`HU-15`, pendiente en §29) — incluye la configuración de las ventanas de tiempo mencionadas en `Requirements-Context.md` §13/§23, que hoy no tiene una historia propia.
- **Consulta de auditoría** (`HU-16`, pendiente en §29).
- **Desbloqueo del servicio tras 5 intentos fallidos de PIN** — `Requirements-Context.md` §15 dice textualmente que "el mecanismo exacto para desbloquear el servicio... queda pendiente de definición", y no aparece en la lista de historias pendientes §29. Es un vacío no planificado; el Superadministrador asume la función de soporte para estos incidentes (resolución INC-11, 2026-09-30), pero el proceso concreto sigue sin definir.
- **Reactivación de perfiles suspendidos** — ver sección Enfermero arriba.
- **Segregación de funciones / doble autorización** — ninguna regla del corpus impide que el Superadministrador apruebe su propio perfil si también se registra como enfermero, ni exige doble autorización para acciones sensibles (p. ej. desactivar el tipo de servicio principal).

---

## Vacíos detectados (transversal a personas)

Estas funcionalidades se mencionan en `Requirements-Context.md` pero no tienen ninguna HU asociada, ni siquiera en la lista de pendientes `§29` (HU-05 a HU-17). **No se redactan aquí historias nuevas — esta lista es la entrada principal para User Story Completer:**

- Recuperación de contraseña para Usuario y Enfermero (creación de credenciales sin ningún mecanismo de recuperación en todo el corpus).
- Cancelación de un servicio (el estado `CANCELADO` existe en los diagramas §8.2/§26 sin reglas de transición propias; el texto remite a "la historia de usuario correspondiente", que no existe). El efecto sobre el cobro y la calificación al cancelar "En curso" tampoco está definido (INC-12).
- Desbloqueo del servicio tras agotar los 5 intentos de PIN (§15, explícitamente "pendiente de definición").
- Reactivación de un perfil `Suspendido` o reingreso tras un `Rechazado` definitivo (resolución INC-29, 2026-09-30).
- Creación y protección (MFA) de la cuenta del Superadministrador.
- Cambio de correo electrónico antes de completar la verificación.
- Gestión/gestión administrativa de las configuraciones de ventanas de tiempo (§13/§23) — mencionada como capacidad del Superadministrador en §22 pero sin HU propia; podría quedar cubierta por la futura `HU-15` si su alcance se define para incluirla explícitamente.
- **Negociación de precio, pago y liquidación** (`vision.md` §5, MVP confirmado el 2026-09-29) — la tarifa sugerida ya vive en `TipoServicio` (`HU-04`, 2026-09-30); falta el mecanismo de oferta/contraoferta, pago y liquidación.
- **Reconciliación entre asignación atómica y el modelo de directorio/invitación directa** (`vision.md` §5, MVP confirmado el 2026-09-29) — sin máquina de estados ni HU.
- **Ubicación en tiempo real del enfermero/paciente y contacto de emergencia** (`vision.md` §5, MVP confirmado el 2026-09-29) — sin entidades, modelo de datos ni HU.
- **Canal y momento de coordinación previa al servicio** — `Requirements-Context.md` §11/§12.2 no define qué canal cubre "lo estrictamente necesario para coordinar" ni cómo tratar servicios aceptados con menos de 180 minutos de margen.

> Nota de trazabilidad: `Auditoria-Especificacion.md` (documento de auditoría externa, no de negocio) profundiza sobre estos mismos vacíos y añade otros no derivados directamente del texto de `Requirements-Context.md` (p. ej. verificación de identidad biométrica, integración con ReTHUS, modelo de pagos, disputas). Esos hallazgos adicionales son recomendaciones de un análisis externo y **no están respaldados todavía por el texto de negocio**; se dejan fuera de este documento a la espera de que el developer decida incorporarlos como HUs mediante `User Story Completer`.

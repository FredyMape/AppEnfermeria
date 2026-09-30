# HU-02 — Registro de Enfermero

## Identificación

- **ID:** HU-02
- **Nombre:** Registro y creación del perfil de enfermero
- **Actor principal:** Enfermero
- **Actor secundario:** Superadministrador
- **Prioridad:** Alta
- **Estado:** Definida para MVP

## Historia de Usuario

> Como enfermero, quiero registrarme en la plataforma proporcionando mi información personal y profesional y cargando los documentos que acrediten mi formación y experiencia, para que un superadministrador pueda verificar mi información y aprobarme antes de permitirme prestar servicios.

## Descripción

El enfermero podrá registrarse desde la aplicación seleccionando el rol de **Enfermero**.

El registro del enfermero utiliza la información común de la persona y agrega información específica relacionada con su perfil profesional.

El registro no implica automáticamente que el enfermero esté habilitado para prestar servicios.

Un **Superadministrador** deberá revisar manualmente la información y documentación proporcionada y aprobar el perfil.

Hasta que el perfil sea aprobado, el enfermero no podrá aceptar servicios.

## Información base de Persona

Los datos comunes deberán mantenerse normalizados en una entidad independiente denominada conceptualmente `Persona`.

Entre otros, podrá contener:

- Nombre completo.
- Tipo de documento.
- Número de documento.
- Teléfono.
- Correo electrónico.
- Foto.
- Zona de residencia.

Los datos comunes no deberán duplicarse innecesariamente dentro de la entidad específica del enfermero.

## Información específica del Enfermero

El perfil profesional del enfermero deberá incluir:

### Información profesional obligatoria

- Título profesional.
- Número de tarjeta profesional.
- Documentación que respalde el título.
- Documento de identidad.
- Foto.
- Tarjeta profesional.

### Experiencia profesional

El enfermero **deberá** registrar (obligatorio, resolución INC-21, 2026-09-29):

- Años de experiencia.
- Experiencia profesional.
- Lugares donde ha trabajado.

Adicionalmente, podrá registrar de forma opcional:

- Información relevante de cada experiencia profesional.
- Documentos o certificados que respalden la experiencia.

### Documentación adicional

Se podrán almacenar documentos adicionales que permitan fortalecer la validación profesional.

Estos documentos son opcionales en el MVP.

Ejemplos:

- Certificados de formación adicional.
- Certificados de experiencia.
- Certificados de vigencia de tarjeta profesional.
- Antecedentes disciplinarios profesionales.
- Otros documentos definidos posteriormente por el negocio.

## Documentos obligatorios del MVP

Los siguientes elementos son obligatorios para enviar el perfil a revisión:

1. Documento de identidad.
2. Foto (capturada en vivo durante el registro para la verificación de identidad facial, ver sección correspondiente).
3. Título profesional.
4. Tarjeta profesional.
5. Años de experiencia, experiencia profesional y lugares donde ha trabajado (resolución INC-21, 2026-09-29).

Los demás documentos son opcionales.

## Zona de residencia

El perfil deberá almacenar la zona de residencia del enfermero.

No se almacenará ni solicitará un radio de servicio.

La zona de residencia no representa un rango máximo de desplazamiento.

> **Nota de alcance (INC-08, resuelta 2026-09-29):** la geolocalización real del enfermero (no solo zona de residencia como texto libre) forma parte del MVP según `specs/context/vision.md` §5. El nivel de precisión y la frecuencia de actualización siguen como pregunta abierta en `vision.md` §9; esta historia se actualizará cuando se resuelvan.

## Credenciales y verificación de correo

El registro de enfermero crea credenciales de acceso equivalentes a las de `HU-01` (resolución INC-06, 2026-09-30):

- El enfermero define una contraseña que debe cumplir la misma política mínima de `HU-01` (letras, números, carácter especial, longitud entre 12 y 64 caracteres), validada en frontend y backend.
- Tras el registro, el sistema genera un código de verificación numérico de 6 dígitos, lo envía al correo registrado, y aplica las mismas reglas de vigencia e intentos que `HU-01` (TTL de 15 minutos, máximo 5 intentos, máximo 3 reenvíos por hora, recordatorio a las 24 horas con código nuevo).
- El enfermero puede iniciar sesión sin haber verificado su correo, pero no puede enviar su perfil a revisión mientras no lo verifique.

> La creación y protección (MFA) de la cuenta del Superadministrador no se cubre en esta historia; queda como vacío explícito para una historia futura (ver `specs/context/business-rules-index.md`).

## Verificación de identidad facial

Como parte del registro, el enfermero debe capturar una foto en vivo que el sistema contrasta automáticamente contra el documento de identidad cargado (resolución INC-04, 2026-09-30; ver `specs/context/vision.md` §5):

- Máximo 2 intentos automáticos de contraste.
- Si ninguno de los 2 intentos coincide, el perfil pasa a revisión manual del Superadministrador junto con el resultado (no coincide, intentos utilizados).
- El Superadministrador puede habilitar intentos adicionales para que el enfermero repita la captura, según el caso (ver `HU-03`).
- La foto en vivo capturada se convierte en la foto de referencia del perfil (`Persona.foto`) una vez el resultado es satisfactorio o el Superadministrador la valida manualmente.
- La foto de referencia se recaptura periódicamente (cada 6-12 meses); quién dispara esa recaptura y qué ocurre si el enfermero no la realiza a tiempo queda como vacío (`vision.md` §7).
- El proveedor de verificación facial y su costo no están definidos todavía; esta historia describe el comportamiento esperado, no la integración técnica concreta (`vision.md` §7, pregunta abierta).

## Consentimientos

- Antes de registrar sus datos personales, profesionales y documentos, el enfermero debe aceptar un aviso de privacidad general (mismo mecanismo de `HU-01`: versión del aviso, fecha/hora, con posibilidad de revocación).
- La verificación de identidad facial requiere un consentimiento explícito y separado para el tratamiento del dato biométrico, distinto del consentimiento general (`vision.md` §7).
- Si el enfermero revoca el consentimiento biométrico después de estar **Aprobado**, el perfil pasa a **Suspendido** (ver `HU-03`), dado que la verificación de identidad ya no puede sostenerse sin ese dato.
- Ninguno de los dos registros de consentimiento podrá completarse implícitamente: el sistema debe solicitar la aceptación explícita en cada caso (resolución INC-05, 2026-09-30).

## Roles

El sistema utilizará una tabla independiente para los roles.

Para el MVP los roles serán fijos.

Roles inicialmente definidos:

- Usuario / Cliente
- Enfermero
- Superadministrador

No se permitirá crear nuevos roles desde la aplicación.

La incorporación de nuevos roles requerirá cambios directamente en la base de datos y/o código de la aplicación.

## Aprobación

El registro de un enfermero tendrá un estado de verificación/aprobación.

Estados conceptuales:

- Pendiente de revisión.
- Aprobado.
- Corrección solicitada.
- Rechazado (terminal, sin reenvío).
- Suspendido (perfil ya aprobado, revocado temporalmente).

> Los estados `Rechazado` y `Corrección solicitada` se unificaron el 2026-09-29 (resolución INC-16) para el caso corregible; el 2026-09-30 se reintrodujo `Rechazado` como estado terminal para casos no corregibles o tras agotar el límite de ciclos de corrección, y se agregó `Suspendido` para revocar perfiles ya aprobados (resolución INC-29; ver `HU-03` y `specs/context/business-rules-index.md`).

Un enfermero solamente podrá aceptar servicios cuando su perfil tenga estado **Aprobado**.

## Flujo principal

1. El usuario selecciona la opción **Registrarse como enfermero**.
2. El sistema solicita la información básica de Persona.
3. El enfermero diligencia sus datos personales y define una contraseña.
4. El enfermero acepta el aviso de privacidad general.
5. El enfermero diligencia su información profesional.
6. El enfermero registra su experiencia profesional.
7. El enfermero carga los documentos obligatorios, incluida la foto en vivo para verificación de identidad.
8. El enfermero acepta el consentimiento biométrico separado.
9. El sistema contrasta la foto en vivo contra el documento de identidad (máx. 2 intentos automáticos).
10. El enfermero puede cargar documentos adicionales opcionales.
11. El sistema valida que los campos obligatorios estén completos.
12. El sistema valida que los documentos obligatorios estén cargados.
13. El sistema crea el perfil del enfermero.
14. El perfil queda en estado **Pendiente de revisión**.
15. El sistema genera un código de verificación de correo y lo envía (mismas reglas que `HU-01`).
16. El sistema notifica al Superadministrador que existe un perfil pendiente, incluido el resultado de la verificación facial.
17. El Superadministrador revisa posteriormente el perfil mediante `HU-03`.
18. Hasta que el Superadministrador apruebe el perfil, el enfermero no puede aceptar servicios.

## Reglas de negocio

### RN-01 — Aprobación obligatoria

Un enfermero no puede aceptar servicios mientras su perfil no haya sido aprobado por un Superadministrador.

### RN-02 — Documentos obligatorios

El enfermero debe proporcionar:

- Documento de identidad.
- Foto.
- Título profesional.
- Tarjeta profesional.

### RN-03 — Documentación adicional

Los documentos adicionales son opcionales y pueden utilizarse para complementar la verificación profesional.

### RN-04 — Aprobación manual

La aprobación del enfermero debe realizarse manualmente por un Superadministrador.

### RN-05 — Zona de residencia

Se almacena únicamente la zona de residencia.

No se almacena un radio de servicio.

### RN-06 — Normalización

La información común entre los diferentes tipos de usuario deberá mantenerse normalizada en la entidad `Persona`.

### RN-07 — Rol

El registro deberá asociarse al rol `Enfermero`.

### RN-08 — Auditoría

Toda creación y modificación relevante del perfil deberá registrar información de auditoría.

### RN-09 — Credenciales

El registro de enfermero exige contraseña y verificación de correo con las mismas reglas que `HU-01` (política de contraseña, TTL y límites del código de verificación).

### RN-10 — Verificación facial

El registro debe incluir el contraste automático entre una foto en vivo y el documento de identidad, con un máximo de 2 intentos; si ninguno coincide, el perfil pasa a revisión manual con ese resultado visible para el Superadministrador.

### RN-11 — Consentimientos

El registro no puede completarse sin el consentimiento general (datos personales/profesionales) ni, de forma separada, sin el consentimiento biométrico explícito.

## Criterios de aceptación

- [ ] El usuario puede seleccionar el rol de enfermero durante el registro.
- [ ] El sistema crea la información común en la entidad Persona.
- [ ] El sistema crea la información específica del perfil de enfermero.
- [ ] El enfermero puede registrar su experiencia profesional.
- [ ] El sistema exige años de experiencia, experiencia profesional y lugares donde ha trabajado como campos obligatorios.
- [ ] El enfermero puede cargar documentos que respalden su experiencia.
- [ ] El sistema exige documento de identidad.
- [ ] El sistema exige foto.
- [ ] El sistema exige título profesional.
- [ ] El sistema exige tarjeta profesional.
- [ ] El sistema permite cargar documentos adicionales opcionales.
- [ ] El sistema almacena la zona de residencia.
- [ ] El sistema no solicita un radio de servicio.
- [ ] El perfil queda inicialmente en estado Pendiente de revisión.
- [ ] El enfermero no puede aceptar servicios mientras no esté aprobado.
- [ ] El Superadministrador puede revisar posteriormente el perfil.
- [ ] El sistema registra información de auditoría.
- [ ] El enfermero puede definir una contraseña con la misma política que `HU-01`.
- [ ] El sistema verifica el correo del enfermero con el mismo mecanismo que `HU-01`.
- [ ] El sistema captura una foto en vivo y la contrasta automáticamente contra el documento de identidad.
- [ ] Tras 2 intentos sin coincidencia, el perfil pasa a revisión manual con el resultado visible.
- [ ] El sistema exige el consentimiento general y el consentimiento biométrico por separado antes de completar el registro.

## Auditoría

El perfil deberá registrar, como mínimo:

- Fecha y hora de creación.
- Usuario de creación.
- Fecha y hora de última modificación.
- Usuario de última modificación.
- Estado actual de verificación.
- Fecha y hora de aprobación, cuando aplique.
- Superadministrador responsable de la aprobación, cuando aplique.

## Fuera de alcance para esta historia

- Aprobación/rechazo detallado del perfil.
- Solicitud de correcciones.
- Notificaciones específicas de aprobación/rechazo.
- Aceptación de servicios.
- Asignación de servicios.
- Inicio de servicios.
- Finalización de servicios.
- Calificaciones.

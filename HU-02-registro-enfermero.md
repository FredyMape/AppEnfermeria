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

El enfermero podrá registrar:

- Años de experiencia.
- Experiencia profesional.
- Lugares donde ha trabajado.
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
2. Foto.
3. Título profesional.
4. Tarjeta profesional.

Los demás documentos son opcionales.

## Zona de residencia

El perfil deberá almacenar la zona de residencia del enfermero.

No se almacenará ni solicitará un radio de servicio.

La zona de residencia no representa un rango máximo de desplazamiento.

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
- Rechazado.
- Corrección solicitada.

Un enfermero solamente podrá aceptar servicios cuando su perfil tenga estado **Aprobado**.

## Flujo principal

1. El usuario selecciona la opción **Registrarse como enfermero**.
2. El sistema solicita la información básica de Persona.
3. El enfermero diligencia sus datos personales.
4. El enfermero diligencia su información profesional.
5. El enfermero registra su experiencia profesional.
6. El enfermero carga los documentos obligatorios.
7. El enfermero puede cargar documentos adicionales opcionales.
8. El sistema valida que los campos obligatorios estén completos.
9. El sistema valida que los documentos obligatorios estén cargados.
10. El sistema crea el perfil del enfermero.
11. El perfil queda en estado **Pendiente de revisión**.
12. El sistema notifica al Superadministrador que existe un perfil pendiente.
13. El Superadministrador revisa posteriormente el perfil mediante HU-03.
14. Hasta que el Superadministrador apruebe el perfil, el enfermero no puede aceptar servicios.

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

## Criterios de aceptación

- [ ] El usuario puede seleccionar el rol de enfermero durante el registro.
- [ ] El sistema crea la información común en la entidad Persona.
- [ ] El sistema crea la información específica del perfil de enfermero.
- [ ] El enfermero puede registrar su experiencia profesional.
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

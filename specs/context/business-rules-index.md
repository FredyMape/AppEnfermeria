# Índice de Reglas de Negocio — AppEnfermeria

> Generado por Context Builder. Re-ejecutable. Última consolidación: 2026-09-22 (incluye resolución de contradicciones CN-01 a CN-09, decidida por el developer el 2026-09-22).
> Fuentes: `Requirements-Context.md`, `HU-01`, `HU-02`, `HU-03`, `HU-04`, `specs/context/vision.md`.
> Los identificadores `RN-XX` colisionan entre documentos (se reinician en cada archivo). Este índice los desambigua prefijando el origen: `CTX-RN-XX` (Requirements-Context.md §27.1), `HU01-RN-XX`, `HU02-RN-XX`, `HU03-RN-XX`, `HU04-RN-XX`.

## 1. Registro y autenticación de Usuario

| ID         | Regla                                                                                                                                                                                                                 | Origen  |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| HU01-RN-01 | No debe existir más de una cuenta asociada al mismo correo electrónico.                                                                                                                                               | `HU-01` |
| HU01-RN-02 | El número de documento debe ser único entre las personas registradas.                                                                                                                                                 | `HU-01` |
| HU01-RN-03 | Un usuario puede iniciar sesión sin verificar su correo, pero no puede solicitar servicios hasta verificarlo.                                                                                                         | `HU-01` |
| HU01-RN-04 | Los datos médicos registrados en el registro pertenecen al propietario de la cuenta.                                                                                                                                  | `HU-01` |
| HU01-RN-05 | En solicitudes para terceros, los datos de esa persona se registran en la solicitud o perfil correspondiente.                                                                                                         | `HU-01` |
| HU01-RN-06 | El backend debe rechazar cualquier contraseña que no cumpla la política mínima de seguridad: longitud entre 12 y 64 caracteres, letras + números + carácter especial. Actualizada el 2026-09-22 (ver CN-09 resuelta). | `HU-01` |
| HU01-RN-07 | El código de verificación es numérico y de 6 dígitos.                                                                                                                                                                 | `HU-01` |

## 2. Registro y verificación de Enfermero

| ID         | Regla                                                                                                        | Origen                                         |
| ---------- | ------------------------------------------------------------------------------------------------------------ | ---------------------------------------------- |
| HU02-RN-01 | Un enfermero no puede aceptar servicios mientras su perfil no esté Aprobado.                                 | `HU-02`                                        |
| HU02-RN-02 | Documentos obligatorios: documento de identidad, foto, título profesional, tarjeta profesional.              | `HU-02`                                        |
| HU02-RN-03 | La documentación adicional es opcional.                                                                      | `HU-02`                                        |
| HU02-RN-04 | La aprobación del enfermero debe ser manual, realizada por un Superadministrador.                            | `HU-02`                                        |
| HU02-RN-05 | Se almacena únicamente la zona de residencia; no se almacena un radio de servicio.                           | `HU-02`                                        |
| HU02-RN-06 | La información común entre roles debe normalizarse en la entidad `Persona`.                                  | `HU-02`                                        |
| HU02-RN-07 | El registro debe asociarse al rol `Enfermero`.                                                               | `HU-02`                                        |
| HU02-RN-08 | Toda creación/modificación relevante del perfil debe registrar auditoría.                                    | `HU-02`                                        |
| HU03-RN-01 | La aprobación debe ser manual, realizada por un Superadministrador.                                          | `HU-03` (duplica la intención de `HU02-RN-04`) |
| HU03-RN-02 | Un enfermero solo puede aceptar servicios en estado Aprobado.                                                | `HU-03` (duplica `HU02-RN-01`)                 |
| HU03-RN-03 | El Superadministrador puede consultar toda la información proporcionada por el enfermero.                    | `HU-03`                                        |
| HU03-RN-04 | La aprobación debe registrar: identificador y nombre del Superadministrador, fecha, hora, estado resultante. | `HU-03`                                        |
| HU03-RN-05 | Todo rechazo debe incluir un comentario con el motivo.                                                       | `HU-03`                                        |
| HU03-RN-06 | El enfermero puede corregir la información indicada y solicitar nueva revisión.                              | `HU-03`                                        |
| HU03-RN-07 | Tras corregir, el perfil vuelve al flujo de revisión.                                                        | `HU-03`                                        |
| HU03-RN-08 | Las acciones de aprobación/rechazo/corrección deben quedar auditadas.                                        | `HU-03`                                        |
| HU03-RN-09 | SLA objetivo de revisión: 24 a 36 horas.                                                                     | `HU-03`                                        |

## 3. Tipos de servicio

| ID         | Regla                                                                                                                                                                                              | Origen  |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| HU04-RN-01 | El sistema debe contar inicialmente con los 4 tipos de servicio definidos para el MVP.                                                                                                             | `HU-04` |
| HU04-RN-02 | Cada tipo de servicio tiene un identificador único.                                                                                                                                                | `HU-04` |
| HU04-RN-03 | No deben existir dos tipos de servicio **activos** con el mismo nombre.                                                                                                                            | `HU-04` |
| HU04-RN-04 | Solo los tipos activos pueden seleccionarse para nuevas solicitudes.                                                                                                                               | `HU-04` |
| HU04-RN-05 | La desactivación no elimina los registros históricos asociados.                                                                                                                                    | `HU-04` |
| HU04-RN-06 | Las solicitudes históricas conservan la referencia al tipo de servicio aunque este se desactive después.                                                                                           | `HU-04` |
| HU04-RN-07 | La estructura debe permitir agregar nuevos tipos posteriormente.                                                                                                                                   | `HU-04` |
| HU04-RN-08 | La gestión de tipos de servicio está reservada al Superadministrador.                                                                                                                              | `HU-04` |
| HU04-RN-09 | El superadministrador puede crear nuevos tipos de servicio y editar el nombre/descripción de los existentes (CRUD completo, no solo cambio de estado). Añadida el 2026-09-22 (ver CN-05 resuelta). | `HU-04` |

## 4. Ciclo de vida del servicio, aceptación, PIN, extensión y calificación

| ID        | Regla                                                                                                                                                                              | Origen                          |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------- |
| CTX-RN-01 | Un servicio solo puede estar asignado a un enfermero (asignación única).                                                                                                           | `Requirements-Context.md` §27.1 |
| CTX-RN-02 | La aceptación y asignación deben ejecutarse de forma atómica.                                                                                                                      | `Requirements-Context.md` §27.1 |
| CTX-RN-03 | Los reintentos de aceptación no deben generar asignaciones duplicadas (idempotencia).                                                                                              | `Requirements-Context.md` §27.1 |
| CTX-RN-04 | El enfermero solo ve la información definida para evaluar la solicitud antes de aceptarla.                                                                                         | `Requirements-Context.md` §27.1 |
| CTX-RN-05 | Los datos médicos sensibles no están disponibles inmediatamente después de la aceptación; requieren además alcanzar la ventana configurada (§12.2). Actualizada el 2026-09-22.     | `Requirements-Context.md` §27.1 |
| CTX-RN-06 | El tiempo de habilitación de información sensible es configurable por el Superadministrador. Valor por defecto: 180 minutos (3 horas) antes del inicio. Actualizada el 2026-09-22. | `Requirements-Context.md` §27.1 |
| CTX-RN-07 | El PIN es numérico de 6 dígitos.                                                                                                                                                   | `Requirements-Context.md` §27.1 |
| CTX-RN-08 | Cada servicio tiene su propio PIN.                                                                                                                                                 | `Requirements-Context.md` §27.1 |
| CTX-RN-09 | El PIN solo puede visualizarse durante la ventana configurada antes del inicio.                                                                                                    | `Requirements-Context.md` §27.1 |
| CTX-RN-10 | El servicio solo pasa a En curso tras validar correctamente el PIN.                                                                                                                | `Requirements-Context.md` §27.1 |
| CTX-RN-11 | Máximo 5 intentos incorrectos para validar el PIN.                                                                                                                                 | `Requirements-Context.md` §27.1 |
| CTX-RN-12 | El servicio no puede finalizar antes de cumplir la duración contratada.                                                                                                            | `Requirements-Context.md` §27.1 |
| CTX-RN-13 | Cualquiera de las partes puede solicitar una extensión.                                                                                                                            | `Requirements-Context.md` §27.1 |
| CTX-RN-14 | La extensión solo es efectiva si la otra parte la aprueba.                                                                                                                         | `Requirements-Context.md` §27.1 |
| CTX-RN-15 | Una extensión aprobada actualiza tiempo y valor del servicio.                                                                                                                      | `Requirements-Context.md` §27.1 |
| CTX-RN-16 | Usuario y enfermero deben calificar obligatoriamente al finalizar el servicio.                                                                                                     | `Requirements-Context.md` §27.1 |
| CTX-RN-17 | Una calificación pendiente bloquea el resto de la aplicación hasta completarla.                                                                                                    | `Requirements-Context.md` §27.1 |
| CTX-RN-18 | El PIN debe almacenarse mediante una función hash criptográfica; nunca en texto plano. La validación compara hashes. Añadida el 2026-09-22 (ver CN-08 resuelta).                   | `Requirements-Context.md` §27.1 |

## 5. Privacidad, datos sensibles y principios técnicos

| ID          | Regla                                                                                                                               | Origen                          |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------- | ------------------------------- |
| CTX-PRIV-01 | El acceso a información depende del estado del servicio y de las reglas de negocio definidas.                                       | `Requirements-Context.md` §21   |
| CTX-PRIV-02 | Los datos médicos sensibles no están disponibles antes del momento configurado.                                                     | `Requirements-Context.md` §21   |
| CTX-PRIV-03 | Los permisos deben implementarse en backend, no solo mediante restricciones visuales de frontend.                                   | `Requirements-Context.md` §21   |
| CTX-TEC-01  | La base de datos debe aplicar principios de normalización; datos comunes centralizados en `Persona`.                                | `Requirements-Context.md` §20.1 |
| CTX-TEC-02  | Las operaciones críticas (asignación, PIN, cambios de estado, extensión, calificaciones) deben garantizar integridad transaccional. | `Requirements-Context.md` §20.2 |
| CTX-TEC-03  | Las operaciones concurrentes (aceptación de un servicio por múltiples enfermeros) deben garantizar atomicidad.                      | `Requirements-Context.md` §20.3 |
| CTX-TEC-04  | Las operaciones sensibles deben soportar reintentos sin efectos duplicados (idempotencia).                                          | `Requirements-Context.md` §20.4 |

---

## Contradicciones resueltas (decisión del developer, 2026-09-22)

Todas las contradicciones `CN-01` a `CN-09` fueron presentadas al developer mediante preguntas explícitas y resueltas en esta fecha. Las decisiones ya se aplicaron a los documentos fuente correspondientes (`Requirements-Context.md`, `HU-01`, `HU-04`) y a este índice. Se conserva el registro para trazabilidad.

| #     | Contradicción original                                                                                                              | Decisión                                                                                                                                                                                 | Cambios aplicados                                                                                                                                                                                                                                                           |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CN-01 | Duplicación divergente de "Información visible antes de aceptar un servicio" (§9 vs §12.1).                                         | Se adopta la versión de §9 (incluye "Si el servicio es programado") como fuente única de verdad.                                                                                         | `Requirements-Context.md` §12.1 reescrita como referencia a §9; eliminada la lista duplicada.                                                                                                                                                                               |
| CN-02 | Dos secciones numeradas "20." ("Auditoría del servicio" y "Principios técnicos") con jerarquía de encabezados inconsistente.        | Corregir la numeración directamente en el documento fuente.                                                                                                                              | `Requirements-Context.md`: "Principios técnicos" conserva el número 20 (subsecciones 20.1–20.4 uniformadas a `##`); "Auditoría del servicio" pasa a ser 20.5, agrupada como principio técnico transversal (auditabilidad). Sin impacto en la numeración de §21 en adelante. |
| CN-03 | "Los datos sensibles incluyen, entre otros:" sin lista.                                                                             | Enumerar: diagnósticos y condiciones médicas, alergias, tipo de sangre/RH.                                                                                                               | `Requirements-Context.md` §12 ahora incluye esa lista explícita.                                                                                                                                                                                                            |
| CN-04 | Nomenclatura de estados del enfermero inconsistente entre §5.2 (un solo valor combinado) y `HU-02`/`HU-03` (dos estados separados). | `Rechazado` y `Corrección solicitada` son dos estados independientes (se alinea §5.2 con `HU-02`/`HU-03`, que ya los trataban por separado).                                             | `Requirements-Context.md` §5.2 actualizada a 4 estados separados: Pendiente de revisión, Aprobado, Rechazado, Corrección solicitada.                                                                                                                                        |
| CN-05 | Ambigüedad de alcance en `HU-04`: ¿CRUD completo o solo cambio de estado sobre catálogo sembrado?                                   | `HU-04` incluye CRUD completo: crear nuevos tipos de servicio y editar nombre/descripción de los existentes, además de activar/desactivar.                                               | `HU-04`: Descripción, Reglas de negocio (nueva `HU04-RN-09`), Flujo principal y Criterios de aceptación actualizados; se retira la exclusión de "Fuera de alcance".                                                                                                         |
| CN-06 | Ningún documento define cómo se determina el "Precio ofrecido"/"valor adicional" de un servicio.                                    | Se confirma el modelo híbrido ya propuesto en `specs/context/vision.md` §5: tarifa sugerida por tipo de servicio (Superadministrador) + oferta del usuario + contraoferta del enfermero. | Sin cambios en `Requirements-Context.md`/HUs actuales (el precio se define recién en `HU-05`+). Queda como decisión de producto confirmada para cuando se redacte esa historia; ver nota de seguimiento abajo.                                                              |
| CN-07 | Cardinalidad `Persona ↔ Rol` no aclarada explícitamente (¿1:1 o N:M?).                                                              | Un solo rol fijo por persona (1:1), tal como sugiere el texto actual de `Requirements-Context.md` §3.2. Una persona no puede tener perfiles bajo más de un rol en el MVP.                | Sin cambios de texto necesarios (el documento ya usaba el singular); se retira la ambigüedad de `domain-model.md`.                                                                                                                                                          |
| CN-08 | El PIN se describe en términos que sugieren almacenamiento y comparación en texto plano.                                            | Se agrega como regla de negocio explícita: el PIN debe almacenarse mediante hash criptográfico y validarse comparando hashes.                                                            | `Requirements-Context.md` §14.1/§14.3 actualizadas; nueva `CTX-RN-18` en la sección 4 de este índice.                                                                                                                                                                       |
| CN-09 | La política de contraseña (`HU01-RN-06`) no fijaba longitud mínima ni máxima.                                                       | Mínimo 12 caracteres, máximo 64 (alineado a buenas prácticas NIST SP 800-63B).                                                                                                           | `Requirements-Context.md` §4.3 y `HU-01` (Política de contraseña, RN-06) actualizadas; `HU01-RN-06` actualizada en la sección 1 de este índice.                                                                                                                             |

> **Nota de seguimiento sobre CN-06:** confirmar el modelo de precio no crea todavía el mecanismo concreto de captura/negociación ni añade un campo de "tarifa sugerida" a `TipoServicio` (`HU-04`). Esa traducción a datos/flujo concretos queda pendiente para cuando `User Story Completer`/`Spec Builder` redacten `HU-05` en adelante — se señala aquí para que no se pierda la trazabilidad de la decisión.

---

## Vacíos detectados

Funcionalidades mencionadas en `Requirements-Context.md` que **no tienen ninguna HU asociada**. Esta lista es la entrada principal de **User Story Completer** — no se redactan aquí historias nuevas.

### Ya planificadas como pendientes (`Requirements-Context.md` §29)

- Creación y publicación de solicitud de servicio (`HU-05`, `HU-06`).
- Consulta de servicios disponibles por el enfermero (`HU-07`).
- Aceptación de servicio (`HU-08`).
- Visualización de información posterior a la asignación (`HU-09`).
- Inicio de servicio mediante PIN (`HU-10`).
- Finalización de servicio (`HU-11`).
- Extensión de servicio (`HU-12`).
- Calificación de usuario y enfermero (`HU-13`).
- Notificaciones (`HU-14`).
- Gestión administrativa y configuraciones (`HU-15`).
- Auditoría (`HU-16`).
- Chat/comunicación entre usuario y enfermero (`HU-17`).

### No planificadas — sin mención en la lista de pendientes §29

- **Cancelación de servicio** — el estado `CANCELADO` existe en los diagramas (§8.2, §26) y sus reglas se remiten a "la historia de usuario correspondiente" (§26.1), que no existe ni está en la lista de §29.
- **Recuperación de contraseña / cambio de correo** — `HU-01` crea credenciales sin que exista mecanismo de recuperación en ningún documento.
- **Desbloqueo del servicio tras agotar los 5 intentos de PIN** — `Requirements-Context.md` §15 lo deja explícitamente "pendiente de definición" sin asignarlo a ninguna historia futura.
- **Revocación o suspensión de un enfermero ya aprobado** — no existe estado ni flujo en `HU-03` ni en ningún otro documento para retirar la aprobación.

> Nota: `Auditoria-Especificacion.md` (auditoría externa) identifica vacíos adicionales — modelo de pagos, verificación de identidad biométrica, integración con el ReTHUS, disputas, requisitos no funcionales, rate limiting — que no están mencionados en el texto de negocio (`Requirements-Context.md`) ni en las HU actuales. No se incluyen en la lista anterior porque este documento solo indexa vacíos **respaldados por el propio corpus de negocio**. Se recomienda al developer revisar `Auditoria-Especificacion.md` directamente antes de ejecutar `User Story Completer`, para decidir cuáles de esos hallazgos externos se incorporan como nuevas historias.

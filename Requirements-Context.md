# Requirements — Plataforma de Servicios de Enfermería

## 1. Contexto general

La plataforma permitirá conectar usuarios que requieren servicios de enfermería con enfermeros previamente registrados, verificados y aprobados por un superadministrador.

El sistema deberá permitir:

- Registro y autenticación de usuarios.
- Registro de enfermeros.
- Validación y aprobación de enfermeros.
- Creación y publicación de solicitudes de servicio.
- Visualización de servicios disponibles por parte de los enfermeros.
- Aceptación atómica de servicios.
- Asignación de servicios a enfermeros.
- Gestión de información personal y médica sensible.
- Inicio del servicio mediante PIN.
- Seguimiento del estado del servicio.
- Finalización del servicio.
- Extensión de servicios.
- Calificación obligatoria de las partes.
- Gestión administrativa mediante un superadministrador.
- Auditoría de las operaciones relevantes.

---

# 2. Roles del sistema

Los roles definidos inicialmente son:

1. Usuario / Cliente
2. Enfermero
3. Superadministrador

Los roles serán fijos para el MVP.

No se permitirá crear nuevos roles desde la interfaz administrativa.

La incorporación de nuevos roles deberá realizarse mediante cambios controlados directamente en la base de datos y/o código de la aplicación.

---

# 3. Modelo de personas y usuarios

## 3.1 Entidad Persona

Los datos comunes entre los diferentes tipos de usuario deberán estar normalizados en una entidad base `Persona`.

Los datos comunes incluyen, como mínimo:

- Nombre completo.
- Tipo de documento.
- Número de documento.
- Teléfono.
- Correo electrónico.
- Foto.
- Zona de residencia.

`Foto` y `Zona de residencia` se modelan como datos de `Persona` (no exclusivos de `Enfermero`), aunque en el MVP solo se diligencian durante el registro de enfermero (resolución INC-24, 2026-09-29; ver `HU-02`).

La información específica de cada rol deberá almacenarse en entidades relacionadas con `Persona`.

## 3.2 Entidad Rol

Debe existir una entidad independiente para representar los roles del sistema.

Roles iniciales:

- Usuario / Cliente.
- Enfermero.
- Superadministrador.

Cada usuario deberá estar asociado a un rol.

La estructura deberá permitir identificar claramente los permisos y capacidades asociados a cada rol.

---

# 4. Usuario / Cliente

## 4.1 Registro

El usuario podrá crear una cuenta desde la aplicación.

Los datos mínimos definidos son:

- Nombre completo.
- Tipo de documento.
- Número de documento.
- Correo electrónico.
- Teléfono.
- Contraseña.

## 4.2 Datos médicos

El usuario podrá registrar información médica personal durante el registro.

Antes de capturar cualquier dato médico, el sistema deberá presentar un aviso de privacidad y solicitar el consentimiento explícito del usuario para el tratamiento de datos sensibles de salud (Ley 1581 de 2012). El registro de datos médicos no podrá completarse sin este consentimiento (resolución INC-05, 2026-09-29).

Como mínimo se contempla:

- Tipo de sangre.
- RH.
- Información médica relevante definida por el sistema.

Estos datos podrán reutilizarse cuando el usuario solicite un servicio para sí mismo.

Cuando el usuario solicite un servicio para otra persona, deberá proporcionar los datos correspondientes a dicha persona, junto con el consentimiento correspondiente del titular o de quien lo represente.

## 4.3 Contraseña

La contraseña deberá cumplir como mínimo:

- Contener letras.
- Contener números.
- Contener al menos un carácter especial.
- Tener una longitud mínima de 12 caracteres.
- Tener una longitud máxima de 64 caracteres.

La validación deberá realizarse en frontend y backend.

El backend será la autoridad final de validación.

## 4.4 Verificación de correo

Después del registro:

1. El sistema crea la cuenta.
2. Genera un código numérico de 6 dígitos.
3. Envía el código al correo registrado.
4. El usuario puede iniciar sesión aunque no haya verificado su correo.
5. El usuario no puede crear solicitudes de servicio mientras el correo no esté verificado.
6. El usuario introduce el código.
7. El sistema valida el código.
8. Si es correcto, el correo queda marcado como verificado.

## 4.5 Recordatorio

Si el usuario no verifica el correo dentro de las primeras 24 horas, el sistema deberá enviar un recordatorio.

La frecuencia de recordatorios adicionales queda pendiente de definición.

---

# 5. Enfermero

## 5.1 Registro

El enfermero podrá registrarse directamente desde la aplicación seleccionando el rol de enfermero.

El registro deberá incluir información personal y profesional.

Los datos de `Persona` (nombre completo, tipo/número de documento, teléfono, correo electrónico, foto, zona de residencia — ver §3.1) se diligencian como parte de este registro. Adicionalmente, el registro captura de forma obligatoria:

- Título profesional.
- Tarjeta profesional.
- Años de experiencia.
- Experiencia profesional.
- Lugares donde ha trabajado.
- Documentos que certifiquen experiencia.
- Certificaciones.
- Documentación profesional adicional.

Los campos de experiencia (años de experiencia, experiencia profesional y lugares donde ha trabajado) son obligatorios para enviar el perfil a revisión (resolución INC-21, 2026-09-29; ver `HU-02`).

La geolocalización real del enfermero (no solo la zona de residencia como texto libre) forma parte del alcance del MVP, de acuerdo con la decisión de alcance registrada en `specs/context/vision.md` §5 (resolución INC-01/INC-08, 2026-09-29). El nivel de precisión (ciudad/barrio vs. coordenadas exactas) y la frecuencia de actualización siguen como pregunta abierta en `vision.md` §9 y deben resolverse antes de la especificación técnica.

No se almacenará ni solicitará un radio máximo de desplazamiento del enfermero.

## 5.2 Estado del enfermero

El enfermero deberá pasar por un proceso de revisión y aprobación manual antes de poder prestar servicios.

El perfil deberá contemplar estados asociados al proceso de verificación, incluyendo como mínimo:

- Pendiente de revisión.
- Aprobado.
- Corrección solicitada.
- Rechazado (terminal, sin reenvío).
- Suspendido (perfil ya aprobado, revocado temporalmente).

Los estados `Rechazado` y `Corrección solicitada`, antes tratados como independientes pese a tener el mismo comportamiento (corregir y volver a revisión), se unificaron en un solo estado (`Corrección solicitada`) el 2026-09-29 (resolución INC-16). El 2026-09-30 se reintrodujo `Rechazado` como estado **terminal** para casos no corregibles o tras agotar el límite de ciclos de corrección, y se agregó `Suspendido` para revocar perfiles ya aprobados (resolución INC-29; ver `HU-03` y `specs/context/business-rules-index.md`). Toda solicitud de corrección, rechazo definitivo o suspensión debe incluir un comentario del Superadministrador que indique el motivo.

## 5.3 Aprobación

El superadministrador deberá revisar manualmente la información proporcionada por el enfermero.

El superadministrador podrá visualizar toda la información del registro y los documentos cargados.

Una vez aprobado:

- Se registra quién realizó la aprobación.
- Se registra la fecha y hora.
- El enfermero queda habilitado para prestar servicios.

---

# 6. Verificación y aprobación del enfermero

El superadministrador podrá revisar:

- Información personal.
- Fotografía.
- Información profesional.
- Título profesional.
- Tarjeta profesional.
- Experiencia.
- Certificaciones.
- Documentos cargados.
- Información adicional proporcionada durante el registro.

## 6.1 Decisiones del superadministrador

El superadministrador podrá:

- Aprobar.
- Solicitar corrección (hasta un máximo de 3 ciclos, ver `HU-03` RN-10).
- Rechazar de forma definitiva (terminal, sin reenvío bajo el mismo registro).
- Suspender un perfil ya aprobado.

Cuando sea necesaria una corrección, un rechazo definitivo o una suspensión, el superadministrador deberá agregar un comentario que indique el motivo.

El comentario será enviado/notificado al enfermero.

## 6.2 Correcciones

Si el perfil requiere correcciones:

1. El enfermero recibe el motivo mediante el comentario.
2. Corrige la información o documentación correspondiente.
3. Solicita una nueva revisión.
4. El perfil vuelve a quedar pendiente de revisión.
5. El superadministrador realiza nuevamente la validación.

---

# 7. Tipos de servicio

Para el MVP se definirán inicialmente los siguientes tipos:

1. Atención domiciliaria.
2. Acompañamiento.
3. Acompañamiento y transporte.
4. Atención hospitalaria.

Los tipos de servicio deberán manejarse como una entidad independiente.

Cada tipo deberá tener como mínimo:

- Identificador único.
- Nombre.
- Descripción.
- Estado.
- Fecha de creación.
- Fecha de actualización.

Estados:

- Activo.
- Inactivo.

Los tipos de servicio inactivos no deberán aparecer como opciones para nuevas solicitudes.

La desactivación de un tipo de servicio no deberá eliminar información histórica.

La arquitectura deberá permitir que posteriormente el superadministrador pueda agregar nuevos tipos de servicio.

---

# 8. Servicio

## 8.1 Estados

Los estados definidos para el servicio son:

- Publicado.
- Asignado.
- En curso.
- Finalizado.
- Cancelado.

## 8.2 Transiciones principales

```text
Publicado
    |
    v
Asignado
    |
    v
En curso
    |
    v
Finalizado
```

Un servicio también podrá pasar a estado Cancelado de acuerdo con las reglas de negocio que se definan para cancelaciones.

# 9. Información visible antes de aceptar un servicio

Antes de aceptar un servicio, el enfermero podrá visualizar únicamente la información necesaria para evaluar y aceptar la solicitud.

La información disponible será:

- Tipo de servicio.
- Fecha del servicio.
- Hora de inicio.
- Si el servicio es programado.
- Duración estimada/contratada.
- Punto de origen.
- Destino, cuando aplique.
- Precio ofrecido.
- Distancia aproximada.
- Descripción general del servicio.
- Información general necesaria sobre el paciente que no corresponda a datos médicos sensibles.

Los datos personales adicionales y los datos médicos sensibles del paciente no estarán disponibles en esta etapa.

Los datos de contacto y la información médica sensible se habilitarán posteriormente de acuerdo con las reglas de tiempo configuradas por el superadministrador.

> **Nota de alcance (INC-02, resuelta 2026-09-29):** `vision.md` §5 confirma que la pasarela de pagos y el modelo de precio híbrido (tarifa sugerida + oferta del usuario + contraoferta del enfermero) están dentro del alcance del MVP. El mecanismo concreto de captura, negociación, comisión y liquidación no se define en este documento; queda pendiente para `HU-05` en adelante (ver `specs/context/business-rules-index.md`, CN-06).
>
> **Nivel de precisión del origen y del texto libre (INC-09):** queda sin definir de forma deliberada — se deja como vacío para que `Architecture Definer`/`Spec Builder` lo resuelvan al diseñar la solicitud de servicio, dado el riesgo de exponer identidad o salud de una persona vulnerable antes de la asignación.

---

# 10. Aceptación del servicio

## 10.1 Confirmación de aceptación

Cuando el enfermero seleccione la opción **Aceptar servicio**, el sistema deberá mostrar una ventana de confirmación.

Ejemplo:

> ¿Estás seguro de que deseas aceptar este servicio?

El servicio no deberá asignarse únicamente por seleccionar el botón inicial de aceptación.

La asignación solamente deberá ejecutarse cuando el enfermero confirme explícitamente la operación.

## 10.2 Asignación atómica

La asignación del servicio deberá ejecutarse de manera atómica.

Si varios enfermeros intentan aceptar el mismo servicio simultáneamente:

1. El sistema deberá validar que el servicio continúe disponible.
2. El primer enfermero cuya confirmación sea procesada correctamente obtendrá la asignación.
3. El servicio deberá cambiar de estado `Publicado` a `Asignado`.
4. Las solicitudes posteriores deberán ser rechazadas.
5. El servicio no podrá quedar asignado a más de un enfermero.

> **Nota de alcance (INC-03, resuelta 2026-09-29):** `vision.md` §5 confirma para el MVP un segundo modelo de descubrimiento (directorio con invitación directa a un enfermero específico) y una negociación de precio previa a la asignación, que conviven con este modelo de asignación atómica por orden de llegada. La máquina de estados y las reglas concretas para reconciliar ambos modelos (p. ej. nuevos estados `Ofertado`/`Invitado`/`Contraofertado`) quedan pendientes de diseño para `Spec Builder`/`Architecture Definer` cuando se redacten las historias `HU-05` en adelante; no se inventan en este documento.

## 10.3 Idempotencia

La operación de aceptación deberá ser idempotente.

El sistema deberá evitar que reintentos producidos por:

- Pérdida de conexión.
- Retransmisión de una solicitud.
- Doble interacción del usuario.
- Reintentos automáticos del cliente.

puedan generar asignaciones duplicadas.

## 10.4 Servicio ya asignado

Si un enfermero está visualizando un servicio que acaba de ser aceptado por otro enfermero, al intentar aceptarlo el sistema deberá informar que el servicio ya no está disponible.

Si el servicio se encuentra dentro de la lista de servicios disponibles, deberá desaparecer de dicha lista una vez sea asignado.

## 10.5 Error durante la aceptación

Si ocurre un error técnico durante la confirmación de aceptación:

- El sistema deberá informar al enfermero que no fue posible completar la operación.
- El backend deberá determinar el estado real de la operación.
- El sistema no deberá asumir que la asignación falló únicamente porque el cliente no recibió la respuesta.
- El enfermero podrá intentar nuevamente únicamente si el servicio continúa disponible.

---

# 11. Información disponible después de aceptar el servicio

Una vez que el servicio haya sido asignado correctamente al enfermero:

- El servicio quedará asociado al enfermero.
- El enfermero podrá acceder a la información personal que corresponda según las reglas de negocio.
- Se habilitará la información necesaria para coordinar el servicio.
- El sistema podrá habilitar los mecanismos de comunicación definidos para el servicio.

La información médica sensible y los datos de contacto estarán sujetos a una ventana de disponibilidad configurable.

---

# 12. Información médica y datos sensibles

Los datos médicos sensibles del paciente no estarán disponibles inmediatamente después de la aceptación del servicio.

El sistema deberá habilitar esta información automáticamente cuando se alcance el tiempo configurado antes del inicio del servicio.

El tiempo de anticipación será configurable desde el módulo del superadministrador. El valor por defecto es de 180 minutos (3 horas) antes de la hora de inicio del servicio.

Ejemplo:

```text
Hora de inicio del servicio: 15:00
Configuración del administrador: 180 minutos antes (3 horas)

Información sensible disponible desde: 12:00
```

La configuración deberá permitir modificar posteriormente el número de minutos.

Los datos sensibles incluyen, entre otros:

- Diagnósticos y condiciones médicas.
- Alergias.
- Tipo de sangre / RH.

## 12.1 Información visible antes de aceptar un servicio

Antes de aceptar un servicio, el enfermero podrá visualizar únicamente la información necesaria para evaluar y aceptar la solicitud. La lista completa de estos datos es la definida en la sección 9 (fuente única de verdad); no se repite aquí para evitar que ambas listas diverjan con el tiempo.

## 12.2 Información médica sensible visible después de aceptar el servicio

Los datos médicos sensibles del paciente se habilitarán para el enfermero únicamente cuando se cumplan ambas condiciones:

1. El servicio haya sido aceptado y asignado a ese enfermero.
2. Se haya alcanzado la ventana de tiempo configurada por el superadministrador antes de la hora de inicio del servicio (valor por defecto: 180 minutos / 3 horas antes; ver sección 13).

Los datos médicos sensibles visibles en esta ventana son:

- Diagnósticos y condiciones médicas.
- Alergias.
- Tipo de sangre / RH.

Los datos de contacto se habilitarán de acuerdo con las mismas reglas de tiempo configuradas por el superadministrador.

La clasificación definitiva de los datos sensibles deberá mantenerse centralizada en las reglas de autorización del backend.

# 13. Configuración de información sensible

El superadministrador podrá configurar el tiempo previo al inicio del servicio en el cual se habilitará la información sensible.

La configuración deberá ser administrable desde el módulo de superadministración.

No deberá estar definida como un valor fijo en el código.

Ejemplo:

```text
Configuración:
Minutos antes del inicio = 180 (valor por defecto: 3 horas)

Servicio:
Inicio = 15:00

Resultado:
Información sensible disponible = 12:00
```

El sistema deberá calcular automáticamente la hora de habilitación utilizando:

```text
Hora de habilitación =
Hora de inicio del servicio - minutos configurados
```

La configuración deberá aplicarse a los servicios según las reglas definidas para el momento de su creación/asignación.

# 14. Inicio del servicio mediante PIN

El servicio utilizará un PIN para validar el inicio.

## 14.1 Generación

Cuando el servicio sea aceptado:

1. El sistema deberá generar un PIN numérico de 6 dígitos.
2. El PIN **no se almacenará en la base de datos, ni siquiera como hash**. Se derivará mediante una función HMAC (p. ej. HMAC-SHA256) a partir del identificador del servicio y un secreto criptográfico gestionado fuera de la base de datos (variable de entorno, vault o KMS), truncado a 6 dígitos.
3. El backend deberá recalcular este valor cada vez que necesite mostrarlo al usuario (dentro de la ventana configurada, §14.2) o validarlo contra lo introducido por el enfermero (§14.3).
4. Deberá ser único, como mínimo, entre servicios simultáneamente activos (ver RN-08 en §27.1).

Una fuga de la base de datos por sí sola no permite reconstruir los PIN, porque el secreto usado para derivarlos no reside en ella (resolución INC-28, 2026-09-30; reemplaza el enfoque de hash simple, que era irreversible y por tanto incompatible con mostrar el PIN al usuario en §14.2).

## 14.2 Visualización del PIN

El usuario podrá visualizar el PIN únicamente durante la ventana de tiempo configurada por el superadministrador. El valor por defecto es de 5 minutos antes de la hora de inicio del servicio (resolución INC-25, 2026-09-29).

Ejemplo:

```text
Hora de inicio del servicio: 15:00
Configuración (valor por defecto): 5 minutos antes

PIN disponible desde: 14:55
```

Antes de alcanzar dicha ventana, el usuario no deberá poder visualizar el PIN.

## 14.3 Validación

Cuando el enfermero se encuentre con el usuario/paciente:

1. El usuario consulta el PIN desde su aplicación.
2. El usuario proporciona el PIN al enfermero.
3. El enfermero introduce el PIN en su aplicación.
4. El backend recalcula el valor HMAC del PIN asociado al servicio (ver §14.1).
5. El sistema compara el valor ingresado (recalculado con el mismo HMAC) con el valor esperado.
6. Si la validación es correcta, el servicio cambia a estado En curso.
7. Si la validación es incorrecta, se registra el intento.

La validación deberá realizarse en backend.

# 15. Intentos de PIN

El enfermero tendrá un máximo de 5 intentos para ingresar correctamente el PIN.

Si el PIN ingresado es incorrecto:

- El sistema deberá informar que el PIN no es válido.
- El intento deberá quedar registrado.
- Se deberá informar el número de intentos restantes.

Si se alcanzan los 5 intentos fallidos:

- El inicio del servicio deberá quedar bloqueado.
- El sistema deberá registrar el bloqueo.
- El sistema deberá notificar o generar el evento correspondiente para el Superadministrador, que asume la función de soporte ante este tipo de incidentes (resolución INC-11, 2026-09-30; no se crea un rol nuevo).
- El servicio no deberá pasar automáticamente a estado En curso.

El mecanismo exacto para desbloquear el servicio después de alcanzar el límite de intentos queda pendiente de definición.

# 16. Finalización del servicio

Un servicio solamente podrá ser finalizado cuando se haya cumplido como mínimo la duración contratada.

La duración deberá calcularse a partir del inicio efectivo del servicio.

Ejemplo:

```text
Hora de inicio: 10:00
Duración contratada: 2 horas

Hora mínima de finalización: 12:00

El sistema no deberá permitir finalizar el servicio antes de las 12:00.
```

## 16.1 Validación de duración

Para permitir la finalización, el backend deberá validar:

- Estado actual del servicio.
- Fecha y hora de inicio efectivo.
- Duración contratada.
- Tiempo transcurrido.
- Extensiones aprobadas, si existen.

La validación deberá ejecutarse en backend.

## 16.2 Intento de finalización anticipada

Si el enfermero intenta finalizar el servicio antes de cumplir el tiempo contratado:

- El sistema deberá rechazar la operación.
- El servicio deberá permanecer en estado En curso.
- El sistema deberá informar que todavía no se ha cumplido el tiempo mínimo requerido.

# 17. Extensión del servicio

Cuando el servicio necesite continuar más allá del tiempo originalmente contratado, cualquiera de las partes podrá solicitar una extensión:

- Usuario.
- Enfermero.

La extensión no será automática.

La extensión deberá contar con la aprobación explícita de la otra parte para hacerse efectiva.

## 17.1 Flujo de extensión

1. Una de las partes selecciona la opción para solicitar una extensión.
2. Se indica el tiempo adicional solicitado.
3. El sistema calcula el valor adicional correspondiente.
4. Se genera una solicitud de extensión.
5. La otra parte recibe una notificación.
6. La otra parte revisa la solicitud.
7. Puede aceptar o rechazar.
8. Si acepta, la extensión queda confirmada.
9. El sistema actualiza la duración total del servicio.
10. El sistema actualiza el valor total del servicio.
11. La nueva hora mínima de finalización queda registrada.

## 17.2 Rechazo de extensión

Si la otra parte rechaza la extensión:

- La extensión no se aplica.
- El tiempo y valor originales permanecen sin cambios.
- El servicio deberá finalizar de acuerdo con la duración previamente contratada.

## 17.3 Auditoría de extensión

Toda solicitud de extensión deberá registrar:

- Quién solicitó la extensión.
- Fecha y hora de solicitud.
- Tiempo adicional solicitado.
- Valor adicional.
- Quién aprobó o rechazó.
- Fecha y hora de la decisión.
- Estado final de la solicitud.

# 18. Finalización y calificación

Una vez cumplido el tiempo contratado o el tiempo adicional aprobado mediante una extensión:

1. El servicio podrá ser finalizado.
2. El sistema deberá solicitar confirmación.
3. El servicio pasará a estado Finalizado.
4. Se registrará la fecha y hora de finalización.
5. Se habilitará la calificación correspondiente.

La calificación deberá realizarse tanto por:

- Usuario.
- Enfermero.

# 19. Calificación obligatoria

Después de finalizar el servicio, tanto el usuario como el enfermero deberán realizar una calificación.

La calificación deberá incluir como mínimo:

- Calificación de 0 a 5 estrellas.
- Comentario opcional.

La calificación será obligatoria.

## 19.1 Bloqueo por calificación pendiente

Si una de las partes tiene una calificación pendiente:

- Al abrir la aplicación deberá mostrarse primero la ventana de calificación.
- No podrá crear nuevas solicitudes de servicio ni aceptar nuevos servicios mientras la calificación esté pendiente.
- Sí podrá continuar con servicios ya en curso: consultar el PIN, solicitar ayuda/soporte, y completar su ciclo de vida (extensión, finalización).
- El bloqueo de nuevas solicitudes/aceptaciones permanecerá hasta que complete la calificación.

La misma regla deberá aplicarse tanto al usuario como al enfermero.

> Alcance acotado el 2026-09-29 (resolución INC-10): el bloqueo original impedía cualquier acción dentro de la aplicación, incluso continuar un servicio ya en curso; se limita a la creación/aceptación de nuevas solicitudes.

## 20. Principios técnicos

## 20.1 Normalización

La base de datos deberá diseñarse aplicando principios de normalización.

Los datos comunes entre diferentes roles no deberán duplicarse innecesariamente.

Los datos comunes deberán estar centralizados en Persona.

Los datos específicos deberán mantenerse en entidades relacionadas.

## 20.2 Integridad

Las operaciones críticas deberán garantizar integridad transaccional.

Especialmente:

- Asignación de servicios.
- Generación y validación de PIN.
- Cambio de estados.
- Extensión de servicios.
- Calificaciones.

## 20.3 Atomicidad

Las operaciones que puedan ser ejecutadas simultáneamente deberán garantizar atomicidad.

El caso principal identificado es la aceptación de un servicio por múltiples enfermeros.

## 20.4 Idempotencia

Las operaciones sensibles deberán diseñarse para soportar reintentos sin producir efectos duplicados.

## 20.5 Auditoría del servicio

Las acciones relevantes relacionadas con el servicio deberán generar información de auditoría.

Como mínimo:

- Creación de la solicitud.
- Publicación.
- Aceptación.
- Asignación.
- Intentos de aceptación.
- Inicio del servicio.
- Intentos de validación del PIN.
- Validación exitosa del PIN.
- Solicitud de extensión.
- Aprobación o rechazo de extensión.
- Finalización.
- Calificación.
- Cancelación, cuando aplique.

Cada evento deberá registrar, cuando corresponda:

- Fecha y hora.
- Usuario que realizó la acción.
- Identificador del servicio.
- Estado anterior.
- Estado nuevo.
- Información relevante de la operación.

# 21. Privacidad y datos sensibles

La plataforma manejará información personal y médica sensible.

El acceso deberá depender del estado del servicio y de las reglas de negocio definidas.

Los datos médicos sensibles no deberán estar disponibles antes del momento configurado para su habilitación.

Los permisos deberán implementarse en backend y no únicamente mediante restricciones visuales en frontend.

# 22. Superadministrador

El superadministrador tendrá capacidades administrativas sobre el sistema.

Entre las funciones definidas se encuentran:

- Revisar y aprobar enfermeros.
- Solicitar correcciones a enfermeros.
- Rechazar de forma definitiva o suspender/revocar un perfil, cuando aplique (ver `HU-03`).
- Consultar información y documentos de enfermeros.
- Gestionar estados de enfermeros.
- Configurar tipos de servicio, incluida la tarifa sugerida por tipo (ver `HU-04`).
- Configurar el tiempo de habilitación de información médica sensible.
- Configurar el tiempo de habilitación del PIN.
- Atender incidentes de soporte reportados por usuarios/enfermeros (p. ej. bloqueo de PIN, ayuda durante un servicio en curso) — función de soporte asumida por este rol, no un rol nuevo (resolución INC-11, 2026-09-30). El proceso concreto (SLA, horario, facultades) queda como vacío para una historia futura.
- Consultar información de auditoría.

# 23. Configuraciones administrables

Las siguientes configuraciones deberán poder modificarse desde el módulo del superadministrador:

## 23.1 Habilitación de datos sensibles

Tiempo previo al inicio del servicio en que se habilitan los datos médicos sensibles.

## 23.2 Habilitación del PIN

Tiempo previo al inicio del servicio en que el usuario puede visualizar el PIN.

## 23.3 Tipos de servicio

Gestión del catálogo de tipos de servicio.

# 24. Entidades principales identificadas

Como mínimo, el modelo deberá contemplar las siguientes entidades:

```text
Persona
Rol
Usuario
Enfermero
SuperAdministrador
TipoServicio
Servicio
InformacionMedica
ExperienciaProfesional
Documento
Certificacion
Calificacion
Notificacion
Configuracion
Auditoria
```

Las entidades podrán dividirse o complementarse durante el diseño técnico detallado.

# 25. Relaciones principales

```text
Persona
   |
   +---- Usuario
   |
   +---- Enfermero
   |
   +---- SuperAdministrador

Rol
   |
   +---- Usuario / Enfermero / SuperAdministrador

TipoServicio
   |
   +---- Servicio

Usuario
   |
   +---- Servicio

Enfermero
   |
   +---- Servicio

Servicio
   |
   +---- Calificacion
   |
   +---- Notificacion
   |
   +---- Auditoria
```

# 26. Estados del servicio

Los estados iniciales definidos son:

```text
PUBLICADO
    |
    v
ASIGNADO
    |
    v
EN_CURSO
    |
    v
FINALIZADO
```

También deberá existir:

```text
CANCELADO
```

Las reglas específicas para cada transición deberán respetar las condiciones de negocio definidas.

## 26.1 Flujo principal de estados

```text
Publicado
    |
    | Aceptación confirmada
    v
Asignado
    |
    | PIN validado
    v
En curso
    |
    | Tiempo mínimo cumplido
    v
Finalizado
```

El servicio también podrá pasar a:

```text
Publicado / Asignado / En curso
            |
            | Cancelación válida
            v
        Cancelado
```

Las condiciones específicas de cancelación deberán definirse en la historia de usuario correspondiente.

> **Nota de alcance (INC-12, 2026-09-30):** este documento no define qué ocurre con el cobro ni con la calificación obligatoria (§19) cuando un servicio se cancela estando `En curso`. Esas consecuencias quedan pendientes de definición en la futura historia de cancelación (vacío ya registrado en `specs/context/business-rules-index.md`); no se resuelven aquí para no inventar alcance.

# 27. Reglas críticas del MVP

Esta sección remite a §27.1 (fuente única de verdad); no se repite la lista aquí para evitar que ambas diverjan con el tiempo (resolución INC-10, 2026-09-30).

## 27.1 Reglas de negocio críticas

## RN-01 — Asignación única

Un servicio solamente podrá estar asignado a un enfermero.

## RN-02 — Atomicidad

La aceptación y asignación del servicio deberá ejecutarse de forma atómica.

## RN-03 — Idempotencia

Los reintentos de aceptación no deberán generar asignaciones duplicadas.

## RN-04 — Información previa a la aceptación

El enfermero solamente podrá visualizar la información definida para evaluar la solicitud antes de aceptarla.

## RN-05 — Datos médicos restringidos

Los datos médicos sensibles no estarán disponibles inmediatamente después de la aceptación.

## RN-06 — Ventana configurable

El tiempo para habilitar información sensible será configurable desde el superadministrador. El valor por defecto es de 180 minutos (3 horas) antes de la hora de inicio del servicio.

## RN-07 — PIN de seis dígitos

El PIN será numérico y tendrá exactamente 6 dígitos.

## RN-08 — PIN por servicio

Cada servicio cuenta con su propio PIN, generado de forma independiente. La unicidad se garantiza como mínimo entre los PIN de servicios que estén simultáneamente en estado Asignado o En curso; dado el espacio limitado de 10^6 combinaciones, no se exige unicidad global entre todos los servicios históricos (aclaración INC-31, 2026-09-30).

## RN-09 — Visualización del PIN

El PIN solamente podrá ser visualizado durante la ventana configurada antes del inicio.

## RN-10 — Inicio mediante PIN

El servicio solamente podrá pasar a En curso después de validar correctamente el PIN.

## RN-11 — Máximo de intentos

Se permitirán como máximo 5 intentos incorrectos para validar el PIN.

## RN-12 — Duración mínima

El servicio no podrá finalizar antes de cumplir la duración contratada.

## RN-13 — Extensión

Cualquiera de las partes podrá solicitar una extensión.

## RN-14 — Aprobación de extensión

La extensión solamente será efectiva cuando sea aprobada por la otra parte.

## RN-15 — Actualización de extensión

Una extensión aprobada deberá actualizar el tiempo y valor del servicio.

## RN-16 — Calificación obligatoria

Usuario y enfermero deberán completar la calificación después de finalizar el servicio.

## RN-17 — Bloqueo por calificación

Una calificación pendiente impedirá crear nuevas solicitudes de servicio o aceptar nuevos servicios hasta completarla; no bloquea continuar con servicios ya en curso (alineada con §19.1 el 2026-09-30, resolución INC-10; antes contradecía el alcance acotado por CN-17).

## RN-18 — Derivación segura del PIN

El PIN no deberá almacenarse en la base de datos en ninguna forma (ni en claro ni como hash): se deriva mediante HMAC a partir del identificador del servicio y un secreto externo a la base de datos. El backend recalcula el valor tanto para mostrarlo al usuario como para validarlo (resolución INC-28, 2026-09-30; reemplaza el enfoque de hash simple).

# 28. Historias de usuario definidas

## HU-01 — Registro de Usuario

El usuario puede registrarse con información básica, establecer una contraseña segura, registrar información médica propia y verificar su correo mediante un código de 6 dígitos.

El usuario puede iniciar sesión antes de verificar el correo, pero no puede solicitar servicios hasta completar la verificación.

Después de 24 horas sin verificar, se genera un recordatorio.

## HU-02 — Registro y gestión del perfil de enfermero

El enfermero puede registrarse seleccionando el rol de enfermero, definir credenciales (contraseña y verificación de correo, igual que `HU-01`) y proporcionar información personal, profesional, experiencia y documentación de respaldo, incluida una foto en vivo contrastada automáticamente contra el documento de identidad.

El perfil queda pendiente de aprobación por parte del superadministrador.

## HU-03 — Verificación y aprobación del enfermero

El superadministrador puede consultar toda la información y documentación del enfermero, incluido el resultado de la verificación facial.

Puede:

- Aprobar.
- Solicitar correcciones/revisión (hasta 3 ciclos).
- Rechazar de forma definitiva (terminal, sin reenvío).
- Suspender un perfil ya aprobado.

Las correcciones y rechazos/suspensiones deberán incluir un comentario.

El enfermero podrá corregir la información y solicitar una nueva revisión mientras no alcance el límite de ciclos.

La aprobación deberá registrar:

- Superadministrador.
- Fecha.
- Hora.

## HU-04 — Gestión de tipos de servicio

El sistema contará con los tipos de servicio iniciales:

- Atención domiciliaria.
- Acompañamiento.
- Acompañamiento y transporte.
- Atención hospitalaria.

Cada tipo de servicio incluye una tarifa sugerida configurable por el superadministrador.

Los tipos podrán estar activos o inactivos.

La estructura deberá permitir incorporar nuevos tipos posteriormente.

# 29. Historias pendientes de definición

Las siguientes historias deberán definirse posteriormente:

- HU-05 — Creación de solicitud de servicio.
- HU-06 — Publicación de solicitud.
- HU-07 — Consulta de servicios disponibles por enfermero.
- HU-08 — Aceptación de servicio.
- HU-09 — Visualización de información posterior a la asignación.
- HU-10 — Inicio de servicio mediante PIN.
- HU-11 — Finalización de servicio.
- HU-12 — Extensión de servicio.
- HU-13 — Calificación de usuario y enfermero.
- HU-14 — Notificaciones.
- HU-15 — Gestión administrativa y configuraciones.
- HU-16 — Auditoría.
- HU-17 — Chat/comunicación entre usuario y enfermero.

Estas historias deberán definirse una por una antes de considerarlas cerradas.

# 30. Principio de definición

Cada historia de usuario deberá documentarse en un archivo Markdown independiente.

Cada archivo deberá contener como mínimo:

- Identificación.
- Historia de usuario.
- Descripción.
- Datos involucrados.
- Reglas de negocio.
- Flujo principal.
- Flujos alternativos.
- Criterios de aceptación.
- Auditoría.
- Consideraciones técnicas cuando sean necesarias.
- Fuera de alcance.

La definición funcional deberá completarse antes de generar la versión definitiva de cada historia para desarrollo.

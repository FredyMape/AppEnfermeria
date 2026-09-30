# HU-01 — Registro de Usuario

## Identificación

- **ID:** HU-01
- **Nombre:** Registro de usuario
- **Actor principal:** Usuario / Cliente
- **Prioridad:** Alta
- **Estado:** Definida para MVP

## Historia de Usuario

> Como usuario de la plataforma, quiero registrarme proporcionando mi información básica y una contraseña segura, para poder acceder al sistema y posteriormente solicitar servicios de enfermería.

## Descripción

El usuario podrá crear una cuenta desde la aplicación.

Durante el registro deberá proporcionar únicamente la información mínima necesaria para crear su cuenta. Los datos médicos personales podrán registrarse durante este proceso para facilitar futuras solicitudes de servicio para sí mismo.

El usuario podrá iniciar sesión aunque todavía no haya verificado su correo electrónico, pero no podrá solicitar servicios hasta completar dicha verificación.

## Datos del registro

### Datos obligatorios

Los datos mínimos definidos para el registro son:

- Nombre completo
- Tipo de documento
- Número de documento
- Correo electrónico
- Teléfono
- Contraseña

### Datos médicos

El usuario podrá registrar información médica personal, incluyendo:

- Tipo de sangre / RH
- Información médica relevante definida por el sistema

Antes de capturar cualquier dato médico, el sistema deberá mostrar un aviso de privacidad y solicitar el consentimiento explícito del usuario para el tratamiento de datos sensibles de salud. El registro de datos médicos no podrá completarse sin este consentimiento (resolución INC-05, 2026-09-29).

Estos datos permitirán reutilizar la información cuando el usuario solicite un servicio para sí mismo.

Si el usuario solicita un servicio para otra persona, deberá proporcionar los datos correspondientes a esa persona durante la creación de la solicitud, junto con el consentimiento correspondiente.

## Política de contraseña

La contraseña deberá cumplir como mínimo con:

- Contener letras.
- Contener números.
- Contener al menos un carácter especial.
- Longitud mínima de 12 caracteres.
- Longitud máxima de 64 caracteres.

La política deberá validarse tanto en frontend como en backend, siendo el backend la autoridad final de validación.

## Verificación de correo electrónico

Después del registro:

1. El sistema crea la cuenta.
2. El sistema genera un código de verificación numérico de 6 dígitos.
3. El sistema envía el código al correo registrado.
4. El usuario puede iniciar sesión aunque todavía no haya verificado su correo.
5. El usuario no podrá crear solicitudes de servicio mientras su correo no esté verificado.
6. El usuario introduce el código recibido para completar la verificación.
7. Una vez validado correctamente, la cuenta queda marcada como correo verificado.

## Recordatorio de verificación

Si el usuario no verifica su correo dentro de las primeras **24 horas**, el sistema deberá generar/enviar un recordatorio de verificación.

El mecanismo de envío y frecuencia adicional de recordatorios queda pendiente de definición.

## Reglas de negocio

### RN-01 — Correo único

No debe existir más de una cuenta asociada al mismo correo electrónico.

### RN-02 — Documento único

El número de documento deberá ser único dentro de las personas registradas en el sistema.

### RN-03 — Verificación obligatoria para solicitar servicios

Un usuario puede iniciar sesión sin haber verificado su correo, pero no puede solicitar servicios hasta completar la verificación.

### RN-04 — Datos médicos propios

Los datos médicos registrados durante el registro corresponden al usuario propietario de la cuenta.

### RN-05 — Solicitudes para terceros

Cuando una solicitud sea realizada para una persona diferente al usuario autenticado, los datos de dicha persona deberán registrarse dentro de la solicitud o del perfil correspondiente.

### RN-06 — Seguridad de contraseña

El backend deberá rechazar cualquier contraseña que no cumpla la política mínima de seguridad: longitud entre 12 y 64 caracteres, y contener letras, números y al menos un carácter especial.

### RN-07 — Código de verificación

El código de verificación será numérico y tendrá 6 dígitos.

### RN-08 — Consentimiento para datos médicos

El sistema no podrá almacenar datos médicos sin haber registrado el consentimiento explícito del usuario para su tratamiento.

### RN-09 — Vigencia e intentos del código de verificación

El código de verificación tendrá una vigencia (TTL) de 15 minutos desde su generación, un máximo de 5 intentos de validación y un máximo de 3 reenvíos por hora (resolución INC-23, 2026-09-29).

## Flujo principal

1. El usuario selecciona la opción **Registrarse**.
2. El sistema muestra el formulario de registro.
3. El usuario diligencia sus datos básicos.
4. El usuario define una contraseña.
5. El sistema valida los datos.
6. El sistema valida la política de contraseña.
7. El sistema verifica que el correo y documento no estén registrados previamente.
8. El sistema crea la cuenta.
9. El sistema genera un código de verificación de 6 dígitos.
10. El sistema envía el código al correo electrónico.
11. El usuario puede iniciar sesión.
12. El sistema identifica que el correo está pendiente de verificación.
13. Mientras no se complete la verificación, el sistema bloquea la creación de solicitudes de servicio.
14. El usuario introduce el código recibido.
15. El sistema valida el código.
16. Si es correcto, marca el correo como verificado.
17. El usuario queda habilitado para solicitar servicios.

## Flujos alternativos

### Código incorrecto

Si el usuario introduce un código incorrecto:

- El sistema deberá informar que el código no es válido.
- El usuario podrá intentar nuevamente hasta un máximo de 5 intentos (RN-09).
- Al alcanzar el límite de intentos, el sistema deberá invalidar el código vigente y exigir la generación de uno nuevo.

### Código expirado

Si el código ha expirado (transcurridos 15 minutos desde su generación, RN-09):

- El sistema deberá informar al usuario.
- El usuario deberá solicitar/generar un nuevo código, respetando el límite de 3 reenvíos por hora (RN-09).

## Criterios de aceptación

- [ ] El usuario puede crear una cuenta con los datos mínimos definidos.
- [ ] El sistema valida la política mínima de contraseña.
- [ ] El correo electrónico no puede estar asociado a otra cuenta.
- [ ] El documento no puede estar asociado a otra persona.
- [ ] El sistema genera un código de verificación de 6 dígitos.
- [ ] El código es enviado al correo registrado.
- [ ] El usuario puede iniciar sesión antes de verificar el correo.
- [ ] El usuario no puede solicitar servicios antes de verificar el correo.
- [ ] El usuario puede verificar su correo mediante el código recibido.
- [ ] Después de verificar el correo, puede solicitar servicios.
- [ ] El sistema contempla un recordatorio después de 24 horas sin verificación.
- [ ] Los datos médicos propios pueden almacenarse durante el registro.
- [ ] El sistema exige consentimiento explícito antes de almacenar datos médicos.
- [ ] Las solicitudes para terceros permiten registrar los datos correspondientes a la otra persona.

## Auditoría

La creación de la cuenta deberá generar información de auditoría, incluyendo como mínimo:

- Fecha y hora de creación.
- Usuario/identificador de creación, cuando aplique.
- Fecha y hora de verificación del correo.
- Estado de verificación.

## Fuera de alcance para esta historia

- Registro y aprobación profesional de enfermeros.
- Asignación de servicios.
- Pago.
- Calificaciones.
- Chat.
- Inicio y finalización de servicios.

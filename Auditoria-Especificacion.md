# Auditoría de Especificación — Plataforma de Servicios de Enfermería

**Rol de la revisión:** Lead Product Owner / Technical Product Manager senior, perspectiva de marketplaces transaccionales de alta concurrencia.
**Alcance:** `Requirements-Context.md`, `HU-01`, `HU-02`, `HU-03`, `HU-04`.
**Fecha:** 22 de septiembre de 2026.
**Estado del backlog auditado:** MVP en definición, 13 historias pendientes (HU-05 a HU-17).

---

## Cómo usar este documento

1. **Sección 1** — veredicto y hallazgos transversales. Léela antes de planear el siguiente sprint.
2. **Secciones 3 a 7** — análisis documento por documento bajo cinco dimensiones, con criterios BDD listos para pegar en Jira y un checklist de cambios por archivo.
3. **Sección 8** — matriz de priorización (ID, severidad, esfuerzo, acción). Es el entregable operativo.
4. **Secciones 9 a 12** — historias faltantes, re-slicing, plantilla de Definition of Ready y requisitos no funcionales propuestos.
5. **Anexo A** — los cálculos que sustentan las afirmaciones cuantitativas.
6. **Anexo B** — fuentes.

Los identificadores `B-xx` (backlog), `CA-xx` (criterios de aceptación) y `C-xx / U-xx / E-xx / V-xx / T-xx` (edge cases) son estables: úsalos como referencia cruzada en los tickets.

---

## 1. Resumen ejecutivo

### 1.1 Veredicto por documento

| Documento                       | DoR (0-10) | Veredicto      | Bloqueante principal                                                                                                                                       |
| ------------------------------- | ---------- | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Requirements-Context.md`       | 3,5        | **No Ready**   | El modelo económico no existe: se habla de precio sin que ninguna historia lo defina, capture, custodie o liquide                                          |
| HU-01 Registro de usuario       | 4,0        | **No Ready**   | Política de contraseña que contradice el estándar vigente en dos direcciones; OTP sin TTL ni límite de intentos; sin base legal para tratar datos de salud |
| HU-02 Registro de enfermero     | 3,0        | **No Ready**   | Verifica credenciales sin verificar identidad; antecedentes disciplinarios opcionales; ignora el ReTHUS                                                    |
| HU-03 Verificación y aprobación | 3,5        | **No Ready**   | SLA aritméticamente imposible; no existe revocación de un enfermero ya aprobado                                                                            |
| HU-04 Tipos de servicio         | 5,5        | **Casi Ready** | Contradicción interna entre título, flujo y alcance; el catálogo no soporta los requisitos que sus propios tipos implican                                  |

### 1.2 Dato sobre el corpus

En 1.035 líneas de `Requirements-Context.md` más las cuatro historias **no aparece ni una sola vez**:

`pago` · `pasarela` · `comisión` · `rate limit` · `cifrado` · `autorización de tratamiento de datos` · `habilitación` · `recuperación de contraseña` · `ReTHUS` · `zona horaria`

Sí aparece **14 veces** la expresión _"pendiente de definición"_, y en varios casos dentro de flujos alternativos que determinan el alcance real de la historia.

### 1.3 Los ocho hallazgos transversales

| #    | Hallazgo                                                                                                                                                                                                                                                     | Evidencia                                                                                                                |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| TR-1 | **El MVP no tiene centro económico.** La sección 9 muestra "Precio ofrecido", la 17.1 calcula el "valor adicional" de una extensión, y HU-04 declara fuera de alcance la definición de tarifas y el cálculo del precio. Ninguna historia produce ese precio. | Contradicción textual entre `Requirements-Context.md` §9/§17.1 y HU-04 "Fuera de alcance"                                |
| TR-2 | **Se confunde privacidad con control de acceso.** La sección 21 se titula "Privacidad y datos sensibles" y su contenido íntegro es que los permisos se implementan en backend. Eso es autorización, no privacidad.                                           | Ley 1581/2012 art. 23 lit. d: cierre inmediato y definitivo de la operación que involucre tratamiento de datos sensibles |
| TR-3 | **Se verifican credenciales sin verificar identidad.** Cuatro imágenes autoportadas, cero contraste con fuente externa, cero biometría.                                                                                                                      | Precedente Care.com: acuerdos de USD 1M (2020), USD 8,5M (FTC, 2024) y USD 480K (Massachusetts, 2018)                    |
| TR-4 | **La fuente de verdad es pública y gratuita, y no se usa.** El ReTHUS devuelve profesión, entidad inscriptora, vigencia y sanciones ético-disciplinarias con solo el tipo y número de documento.                                                             | Ley 1164/2007; consulta pública del SISPRO                                                                               |
| TR-5 | **La aprobación manual es un cuello de botella aritméticamente insostenible.** El SLA de 24-36 h se rompe a ~21 registros/día con reprocesos, y a ~7/día si el revisor atiende también el resto de sus funciones.                                            | Anexo A.2                                                                                                                |
| TR-6 | **La idempotencia declarada no es implementable ni testeable.** Falta clave, alcance, ventana de retención, respuesta en replay y manejo de payload divergente.                                                                                              | `draft-ietf-httpapi-idempotency-key-header-07` (IETF, oct. 2025)                                                         |
| TR-7 | **Cero requisitos no funcionales y cero volumetría.** Sin objetivo numérico, la pregunta "¿escala a millones?" no tiene respuesta verificable.                                                                                                               | Ausencia total en los 5 documentos                                                                                       |
| TR-8 | **Ninguna historia es pequeña ni estimable.** HU-01 cubre registro, contraseña, datos médicos, OTP, recordatorio programado y auditoría en 17 pasos, 7 RN y 13 criterios.                                                                                    | Criterios INVEST (S y E)                                                                                                 |

---

## 2. Defectos de higiene documental (corregir de inmediato, esfuerzo XS)

- [ ] `Requirements-Context.md`: la sección **"Información visible antes de aceptar un servicio" está duplicada** (§9 y §12.1) y las dos versiones **divergen**: §9 incluye "Si el servicio es programado" y §12.1 no. Dejar una sola fuente de verdad.
- [ ] `Requirements-Context.md`: **hay dos secciones numeradas "20"** ("Auditoría del servicio" y "Principios técnicos"), y la numeración de encabezados mezcla `#` y `##` sin jerarquía consistente.
- [ ] `Requirements-Context.md` §12: la frase "Los datos sensibles incluyen, entre otros:" queda **sin lista debajo**.
- [ ] HU-04: el título dice "Gestión", el flujo principal describe gestión, y "Fuera de alcance" excluye la gestión desde la interfaz. **Resolver en una frase** (ver B-27).
- [ ] Unificar la nomenclatura de estados: el documento usa `Rechazado / requiere corrección` (§5.2) y `Rechazado` + `Corrección solicitada` como estados separados (HU-02, HU-03). No son lo mismo.
- [ ] Los identificadores `RN-01`, `RN-02`… se reinician en cada archivo y colisionan. Prefijar por historia: `HU01-RN-01`, `CTX-RN-01`.

---

## 3. `Requirements-Context.md`

### 3.1 Visión de Producto y Fit de Negocio

**No Ready (3,5/10).** No falta detalle: falta la mitad del producto.

**a) Ciclo lógico irresoluble en el precio.** El sistema debe mostrar un precio que ninguna historia produce. Las tres respuestas posibles (tarifa fija por tipo de servicio, precio propuesto por el usuario, subasta inversa entre enfermeros) producen arquitecturas, modelos de datos y experiencias radicalmente distintas. Cualquier equipo que inicie HU-05 se detendrá en la primera hora.

**b) El producto declara un modelo de confianza que no construye.** La propuesta de valor implícita es "enfermeros verificados y aprobados". El mecanismo real es que un humano mire cuatro JPG autoportados. Es el mismo riesgo estructural que llevó a Care.com a tres acuerdos sucesivos por tergiversar el alcance de su verificación, con un agravante: aquí el servicio es clínico y ocurre dentro del domicilio.

**c) La fricción está mal repartida.** Hay cuatro puntos de fricción dura (calificación obligatoria con bloqueo total de la app, modal de confirmación, intercambio verbal de PIN, bloqueo tras 5 intentos sin desbloqueo) y cuatro ausencias graves (recuperación de contraseña, borrador en el registro de enfermeros, cancelación, soporte). La fricción molesta al usuario legítimo y no protege al negocio.

**Condiciones de salida para Ready:**

1. Decidir el modelo de precio y escribir la historia de pagos.
2. Determinar si la plataforma es intermediario o prestador de servicios de salud.
3. Definir volumetría objetivo del MVP (usuarios, enfermeros, solicitudes/día, pico).
4. Eliminar las 14 ocurrencias de "pendiente de definición" o moverlas a un backlog con dueño y fecha.

### 3.2 Arquitectura, Escalabilidad y Resiliencia

#### 3.2.1 La idempotencia no es implementable tal como está escrita

§10.3 dice que la operación "deberá ser idempotente" y enumera cuatro causas de reintento. No dice cómo. Faltan las cinco decisiones sin las cuales no hay código ni caso de prueba:

| Decisión                               | Recomendación                                                          |
| -------------------------------------- | ---------------------------------------------------------------------- |
| Quién genera la clave                  | El cliente, UUID v4, cabecera `Idempotency-Key`                        |
| Alcance                                | Par `(enfermero_id, clave)`, con huella del cuerpo de la petición      |
| Ventana de retención                   | 24 h (Stripe purga las claves a las 24 h)                              |
| Respuesta en replay                    | `200` con la respuesta original almacenada + indicador de reproducción |
| Clave reutilizada con payload distinto | `422 Unprocessable Entity`, sin ejecutar nada                          |

Referencia normativa a citar en el documento: `draft-ietf-httpapi-idempotency-key-header-07`.

**Consecuencia directa:** el escenario §10.5 ("error técnico durante la confirmación") no tiene solución sin esto. El documento pide que "el backend determine el estado real de la operación", que es precisamente lo que el backend no puede hacer sin un identificador estable de la intención del cliente. Un reintento tras timeout es indistinguible de un segundo enfermero.

#### 3.2.2 Fila caliente: especificar la sentencia, no la intención

**Trampa que el equipo implementará si no se especifica** (`SELECT` y luego `UPDATE`): bajo `READ COMMITTED` dos transacciones leen `PUBLICADO` y ambas escriben. El documento describe el resultado deseado y no prohíbe la implementación que lo rompe.

**Patrón obligatorio — bloqueo optimista con actualización condicional:**

```sql
UPDATE servicio
   SET estado       = 'ASIGNADO',
       enfermero_id = :enfermero,
       version      = version + 1,
       asignado_en  = now()
 WHERE id = :servicio
   AND estado = 'PUBLICADO';
-- rowcount = 0  ->  409 Conflict (SERVICIO_NO_DISPONIBLE)
-- rowcount = 1  ->  asignación efectiva
```

Un solo viaje a la base, sin bloqueos explícitos, sin deadlocks. Con 500 intentos concurrentes, 499 fallan rápido.

**Contraste cuantitativo con `SELECT ... FOR UPDATE`:** serializa. Si cada transacción ocupa la fila 8 ms, 500 intentos producen una cola de hasta 4 s y saturan el pool de conexiones. Para el feed de HU-07, si se opta por un modelo _pull_, el patrón es `SELECT ... FOR UPDATE SKIP LOCKED`.

#### 3.2.3 El modal de confirmación amplía la ventana de carrera

§10.1 introduce deliberadamente un diálogo entre "ver" y "aceptar". Es sensato en UX y **empeora la concurrencia**: si el modal tarda 4 s y 12 enfermeros miran el mismo servicio, la tasa de "ya fue tomado" se dispara. Ese es el peor resultado posible para el lado de la oferta, porque el enfermero confirma con intención y recibe un rechazo. Dos soluciones estándar, ninguna presente en la especificación:

- **Reserva temporal:** _soft lock_ de 30 s al abrir el modal, con liberación por TTL.
- **Aceptar y deshacer:** asignar de inmediato y ofrecer revertir durante N segundos (patrón "deshacer" de Gmail).

#### 3.2.4 Notificaciones y correo: punto único de fallo no declarado

Todo el flujo crítico depende del correo (verificación en HU-01, notificaciones en HU-03) y no hay una línea sobre entrega. Faltan: **outbox transaccional**, reintentos con backoff exponencial, cola de mensajes muertos, manejo de rebotes duros, lista de supresión y canal alternativo.

- Envío **dentro** de la transacción: se pierden correos en rollback y se acopla la latencia del proveedor SMTP a la transacción de negocio.
- Envío **fuera** de la transacción: se pierden correos si el proceso muere entre el commit y el envío.
- El outbox es la única solución correcta y **cambia el modelo de datos** (tabla `outbox` con estado, intentos y próxima ejecución).

#### 3.2.5 Ausencia total de rate limiting

Única mención de límite en todo el corpus: "5 intentos" del PIN. No hay límite en registro, login, reenvío de código, validación de código, carga de documentos, feed ni aceptación. Mapea a **OWASP API Security Top 10 (2023) API4 — Unrestricted Resource Consumption** y **API6 — Unrestricted Access to Sensitive Business Flows**. El reenvío de OTP sin límite es además coste directo facturable.

#### 3.2.6 El reloj no está definido

§13 calcula `Hora de habilitación = Hora de inicio − minutos configurados`. No dice zona horaria, no dice que el cálculo sea server-side, y no resuelve **qué pasa con los servicios ya agendados cuando el superadministrador cambia la configuración de 60 a 5 minutos**. El documento remite a "las reglas definidas para el momento de su creación/asignación", reglas que no existen en ningún archivo.

**Regla correcta:** la configuración se **versiona y se congela (snapshot) en el propio servicio al momento de la asignación**. Si se lee en tiempo de consulta, un cambio administrativo puede retirar retroactivamente el acceso a datos médicos a un enfermero a 20 minutos de iniciar, o abrirlo antes de tiempo para 10.000 servicios a la vez. Ambos son incidentes de cumplimiento, no bugs.

Colombia es UTC-5 sin horario de verano, lo que reduce el riesgo pero no lo elimina: el reloj del dispositivo es manipulable. Almacenar instantes en UTC, presentar en `America/Bogota`, evaluar ventanas **solo** en servidor.

#### 3.2.7 La tabla de auditoría será la más caliente y la más grande

§20 enumera 13 tipos de evento, incluidos "intentos de aceptación" e "intentos de validación del PIN". Con esa granularidad `Auditoria` recibe más escrituras que cualquier tabla de negocio. Escrita de forma síncrona dentro de la transacción de aceptación, añade latencia a la operación más contendida del sistema.

Falta especificar: **append-only** (sin `UPDATE` ni `DELETE`, garantizado a nivel de base de datos), particionamiento temporal, política de retención, y escritura asíncrona vía outbox.

#### 3.2.8 `Persona ↔ Rol` es 1:1 y eso cierra un segmento entero

§3.2 dice "cada usuario deberá estar asociado a un rol" (singular). HU-01 RN-02 exige documento único entre personas registradas. Combinadas: **una enfermera de la plataforma no puede registrarse como usuaria para contratar un servicio para su madre**, y un superadministrador no puede probar el flujo de usuario.

**Corrección:** relación N:M entre `Persona` y `Rol`, con perfiles independientes por rol colgando de la misma persona. Hacerlo ahora cuesta horas; hacerlo en el mes 6 es una migración con datos en producción.

### 3.3 Edge cases y manejo de errores

| #    | Escenario no cubierto                                                     | Impacto                                                                    | Probabilidad | Mitigación propuesta                                                                           |
| ---- | ------------------------------------------------------------------------- | -------------------------------------------------------------------------- | ------------ | ---------------------------------------------------------------------------------------------- |
| C-01 | El enfermero acepta y nunca llega (no-show)                               | Servicio congelado en ASIGNADO, paciente desatendido                       | Alta         | Estado `NO_SHOW`, temporizador desde la hora de inicio, republicación automática, penalización |
| C-02 | Se agotan los 5 intentos de PIN con el enfermero en el domicilio          | Servicio bloqueado sin ruta de recuperación, incidente P1 garantizado      | Alta         | Desbloqueo por soporte con doble autorización, o regeneración de PIN notificada a ambas partes |
| C-03 | El paciente está inconsciente o es menor y no puede dictar el PIN         | El servicio no puede iniciar en el caso clínico más crítico                | Media-alta   | PIN entregable a un cuidador designado en la solicitud                                         |
| C-04 | El usuario dicta el PIN por teléfono sin que el enfermero esté presente   | Fraude de facturación por tiempo no prestado                               | Media        | El PIN no prueba presencia: añadir verificación geográfica o QR dinámico                       |
| C-05 | Se desactiva un tipo de servicio con solicitudes en PUBLICADO sin asignar | HU-04 solo cubre históricas; el limbo no está definido                     | Media        | Regla explícita: conservar o cancelar con notificación                                         |
| C-06 | Cambio de configuración de ventana con servicios ya agendados             | Acceso retirado u otorgado retroactivamente a miles de servicios           | Media        | Snapshot de la configuración en el servicio al asignar                                         |
| C-07 | El usuario pierde acceso a su correo y nunca verifica                     | Cuenta muerta; no existe historia de recuperación                          | Alta         | Cambio de correo verificado y recuperación de cuenta                                           |
| C-08 | Reenvío múltiple del código de verificación                               | N códigos válidos simultáneos multiplican por N la probabilidad de acierto | Alta         | Invalidar el anterior en cada emisión y limitar reenvíos                                       |
| C-09 | El servicio se extiende más allá de la medianoche                         | Cálculo de duración y valor cruzando días                                  | Media        | Almacenar instantes UTC, nunca fecha + hora local por separado                                 |
| C-10 | El enfermero aprobado recibe una sanción posterior                        | Sigue prestando servicios indefinidamente                                  | Media        | Re-verificación periódica contra ReTHUS y estado `SUSPENDIDO`                                  |
| C-11 | Ambas partes solicitan una extensión simultáneamente                      | Dos solicitudes activas, duración y valor indeterminados                   | Baja-media   | Una sola solicitud activa por servicio, con bloqueo optimista                                  |
| C-12 | El enfermero cancela tras aceptar y antes de iniciar                      | No existe ninguna regla                                                    | Alta         | Historia de cancelación con ventanas, penalizaciones y republicación                           |
| C-13 | Una solicitud publicada que nadie acepta llega a su hora de inicio        | Se queda en PUBLICADO para siempre y contamina el feed                     | Alta         | Estado `EXPIRADO` con proceso de barrido                                                       |
| C-14 | Disputa sobre la duración o la calidad del servicio                       | Se resuelve fuera del sistema, sin trazabilidad                            | Media        | Estado `EN_DISPUTA` y flujo de resolución                                                      |

#### Estados faltantes en la máquina de estados

`CANCELADO` aparece en los diagramas (§8.2 y §26) y sus reglas se remiten a "la historia de usuario correspondiente", **que no existe ni siquiera en la lista de pendientes HU-05 a HU-17**. Un estado sin reglas de transición es una invitación a que cada desarrollador invente las suyas.

Añadir además: `EXPIRADO`, `NO_SHOW`, `EN_DISPUTA`. Y sustituir el diagrama ASCII por una **tabla explícita de transiciones permitidas con sus guardas**:

| Origen     | Destino    | Guarda                                                               | Actor               |
| ---------- | ---------- | -------------------------------------------------------------------- | ------------------- |
| —          | PUBLICADO  | usuario verificado, tipo de servicio activo                          | Usuario             |
| PUBLICADO  | ASIGNADO   | enfermero APROBADO, cumple requisitos del tipo, `estado = PUBLICADO` | Enfermero           |
| PUBLICADO  | EXPIRADO   | `now() > hora_inicio`                                                | Sistema             |
| PUBLICADO  | CANCELADO  | dentro de ventana de cancelación                                     | Usuario             |
| ASIGNADO   | EN_CURSO   | PIN válido, dentro de ventana                                        | Enfermero           |
| ASIGNADO   | NO_SHOW    | `now() > hora_inicio + tolerancia` sin PIN válido                    | Sistema             |
| ASIGNADO   | PUBLICADO  | cancelación del enfermero o suspensión del perfil                    | Enfermero / Sistema |
| ASIGNADO   | CANCELADO  | según reglas de cancelación                                          | Usuario / Sistema   |
| EN_CURSO   | FINALIZADO | `tiempo_transcurrido >= duración contratada + extensiones`           | Enfermero           |
| EN_CURSO   | EN_DISPUTA | reclamo de cualquiera de las partes                                  | Usuario / Enfermero |
| FINALIZADO | EN_DISPUTA | dentro de la ventana de reclamo                                      | Usuario / Enfermero |

### 3.4 Seguridad y Cumplimiento

#### 3.4.1 La sección 21 no es una sección de privacidad

Su contenido íntegro es que el acceso depende del estado del servicio y que los permisos se implementan en backend. Eso es **autorización**. Falta todo lo demás: base legal, finalidad declarada, minimización, consentimiento, derechos del titular, plazo de conservación, encargados del tratamiento y transferencias internacionales (relevante si la nube está fuera de Colombia).

Importa porque los datos tratados son de la categoría más protegida del ordenamiento colombiano. El **artículo 23 de la Ley 1581 de 2012** faculta a la SIC para imponer:

- multas personales e institucionales de hasta **2.000 SMLMV**, sucesivas mientras subsista el incumplimiento (≈ COP 3.501.810.000 con el salario mínimo de 2026);
- suspensión de actividades hasta por 6 meses;
- cierre temporal;
- **cierre inmediato y definitivo de la operación que involucre el tratamiento de datos sensibles.**

El último literal está redactado específicamente para operaciones como esta. **La sanción de peor caso no es una multa, es el apagado del producto.** Contexto de aplicación: la SIC abrió 101 investigaciones por infracciones a la Ley 1581 en 2025, frente a 83 en 2024.

#### 3.4.2 Hay un titular de datos que nadie autoriza: el tercero

HU-01 RN-05 permite solicitar un servicio para otra persona y registrar sus datos, incluidos datos médicos. Ese tercero **es titular de datos sensibles, no tiene cuenta, no ha dado autorización y no puede ejercer sus derechos**. La especificación no contempla mecanismo alguno para obtenerla ni para notificarle.

Afecta a un porcentaje alto del volumen esperado, porque el caso de uso central de un servicio de enfermería es contratar para un familiar mayor.

#### 3.4.3 ¿Intermediario o prestador?

La plataforma define un catálogo de servicios clínicos, verifica credenciales profesionales, agenda la atención, controla su inicio y su finalización y fija su duración. La **Resolución 3100 de 2019, artículo 4**, establece que todo prestador de servicios de salud debe estar inscrito en el REPS, con al menos una sede y por lo menos un servicio habilitado. La atención domiciliaria es un servicio habilitable.

La respuesta es jurídica y no se resuelve en una auditoría técnica. **El problema es que la especificación ni siquiera formula la pregunta**, y de la respuesta depende si el producto puede operar. Dos agravantes que vienen del propio catálogo de HU-04:

- _"Atención hospitalaria"_ implica ingresar personal a una IPS con su propio régimen de habilitación y talento humano, lo que exige convenio.
- _"Acompañamiento y transporte"_ implica un vehículo cuyas condiciones nadie verifica.

> **Hipótesis a validar** con concepto del Ministerio de Salud o de la Supersalud **antes** de escribir código de HU-05.

#### 3.4.4 Otras brechas transversales

- **Sin cifrado especificado:** ni en reposo, ni a nivel de campo para datos médicos, ni para los documentos cargados.
- **Sin segregación de funciones:** el mismo superadministrador aprueba enfermeros, configura las ventanas de exposición de datos sensibles y consulta la auditoría, sin regla que le impida auditar su propia actividad o aprobarse a sí mismo.
- **Sin gestión de sesión:** duración de token, revocación, cierre en todos los dispositivos.
- **Sin MFA** para el rol con más poder del sistema.

#### 3.4.5 El PIN: dos fallos, uno de implementación y uno de modelo de amenaza

1. **Implementación.** §14.1 dice que el PIN "deberá almacenarse asociado al servicio en la base de datos" y §14.3 que "el sistema compara el PIN ingresado con el PIN almacenado". Eso describe almacenamiento en claro y comparación literal. Debe almacenarse un **hash** con sal y compararse en tiempo constante.
2. **Modelo de amenaza.** El PIN **no prueba presencia**: el usuario puede dictarlo por teléfono desde otra ciudad. Para un servicio que factura por tiempo, el PIN es el control antifraude principal y está roto por diseño. La entropía es correcta (5 intentos sobre 10⁶ = 0,0005 % de probabilidad de acierto); el problema es la colusión y el inicio remoto.
3. **Bloqueo sin salida.** §15 bloquea el inicio tras 5 fallos y deja "pendiente de definición" el mecanismo de desbloqueo. Es un incidente P1 con un paciente esperando, garantizado en las primeras semanas.

### 3.5 Criterios BDD faltantes — contexto general

```gherkin
CA-CTX-01  Aceptación idempotente
  Dado un enfermero aprobado que envía una aceptación con Idempotency-Key "K1"
  Cuando la operación se completa con éxito y el cliente reenvía la misma
        petición con la clave "K1" y el mismo cuerpo dentro de las 24 horas
  Entonces el sistema responde 200 con la respuesta original almacenada
  Y no se crea una segunda asignación
  Y la respuesta indica que se trata de una reproducción

CA-CTX-02  Clave de idempotencia reutilizada con cuerpo distinto
  Dada una Idempotency-Key "K1" ya registrada para el servicio 100
  Cuando llega una petición con la clave "K1" y el servicio 200
  Entonces el sistema responde 422 y no ejecuta ninguna asignación

CA-CTX-03  Concurrencia sobre el mismo servicio
  Dado un servicio en estado PUBLICADO
  Cuando 200 enfermeros aprobados confirman su aceptación dentro de la misma
        ventana de 2 segundos
  Entonces exactamente una aceptación resulta en estado ASIGNADO
  Y las 199 restantes reciben 409 con código SERVICIO_NO_DISPONIBLE
  Y el p99 de respuesta de las peticiones rechazadas es inferior a 300 ms

CA-CTX-04  Reserva temporal durante la confirmación
  Dado un enfermero que abre el diálogo de confirmación de un servicio
  Cuando otro enfermero intenta abrir el mismo diálogo dentro de los 30 segundos
  Entonces el segundo enfermero ve que el servicio está reservado
  Y si el primero no confirma en 30 segundos, la reserva se libera automáticamente

CA-CTX-05  Congelamiento de la configuración de ventana sensible
  Dado un servicio asignado con una configuración vigente de 60 minutos
  Cuando el superadministrador cambia la configuración global a 5 minutos
  Entonces ese servicio conserva su ventana de 60 minutos
  Y los servicios asignados después del cambio usan 5 minutos

CA-CTX-06  Evaluación de ventanas exclusivamente en servidor
  Dado un dispositivo cuyo reloj está adelantado 3 horas
  Cuando el enfermero consulta los datos médicos sensibles de un servicio
        cuya ventana aún no ha abierto según el reloj del servidor
  Entonces la API responde 403 y no devuelve ningún campo sensible

CA-CTX-07  Entrega garantizada de notificaciones
  Dada una transacción de negocio que debe emitir una notificación
  Cuando la transacción hace commit
  Entonces el evento queda persistido en la tabla outbox en la misma transacción
  Y si el proveedor de envío falla, el evento se reintenta con backoff exponencial
  Y tras 5 fallos pasa a la cola de mensajes muertos con alerta

CA-CTX-08  Expiración de publicación
  Dado un servicio en estado PUBLICADO cuya hora de inicio ya pasó
  Cuando se ejecuta el proceso de expiración
  Entonces el servicio pasa a EXPIRADO
  Y desaparece del feed de servicios disponibles
  Y el usuario recibe una notificación con la opción de republicar

CA-CTX-09  No-show del enfermero
  Dado un servicio en estado ASIGNADO cuya hora de inicio pasó hace 20 minutos
        sin validación de PIN
  Cuando se ejecuta el proceso de control
  Entonces el servicio pasa a NO_SHOW
  Y se notifica al usuario con la opción de republicar sin coste
  Y el evento se registra contra el historial del enfermero

CA-CTX-10  Transiciones ilegales
  Dado un servicio en estado FINALIZADO
  Cuando se recibe cualquier petición de transición a ASIGNADO o EN_CURSO
  Entonces el sistema responde 409, no modifica el estado
  Y registra el intento en auditoría

CA-CTX-11  Almacenamiento seguro del PIN
  Dado un PIN generado para un servicio
  Cuando se inspecciona la base de datos
  Entonces no existe ninguna columna que contenga el PIN en texto plano
  Y la validación se realiza comparando hashes en tiempo constante

CA-CTX-12  Desbloqueo tras agotar los intentos de PIN
  Dado un servicio con 5 intentos fallidos de PIN
  Cuando un agente de soporte con doble autorización regenera el PIN
  Entonces se emite un PIN nuevo y se notifica a ambas partes
  Y el contador de intentos se reinicia
  Y toda la operación queda registrada en auditoría

CA-CTX-13  Inmutabilidad de la auditoría
  Dado un registro de auditoría existente
  Cuando cualquier rol, incluido el superadministrador, intenta modificarlo
        o eliminarlo
  Entonces la operación es rechazada a nivel de base de datos

CA-CTX-14  Una sola extensión activa
  Dado un servicio con una solicitud de extensión pendiente de respuesta
  Cuando la otra parte solicita una segunda extensión
  Entonces el sistema la rechaza indicando que ya hay una solicitud en curso
```

### 3.6 Checklist de cambios al documento

- [ ] Decidir el modelo de precio y crear la historia de pagos antes de HU-05.
- [ ] Especificar el mecanismo de idempotencia por referencia al borrador del IETF.
- [ ] Incluir la sentencia de asignación condicional y **prohibir explícitamente** el patrón `SELECT`-luego-`UPDATE`.
- [ ] Añadir tabla de transiciones permitidas con guardas; incorporar `EXPIRADO`, `NO_SHOW`, `EN_DISPUTA`.
- [ ] Cambiar `Persona ↔ Rol` a N:M.
- [ ] Añadir sección de requisitos no funcionales con volumetría, p95/p99, disponibilidad, RPO/RTO y retención (ver §12).
- [ ] Añadir política de rate limiting transversal por endpoint.
- [ ] Reescribir §21 como capítulo de cumplimiento real, no de autorización.
- [ ] Especificar hash del PIN y procedimiento de desbloqueo.
- [ ] Especificar reloj: UTC en almacenamiento, `America/Bogota` en presentación, evaluación solo en servidor.
- [ ] Especificar outbox transaccional para correos y notificaciones.
- [ ] Definir la política de auditoría: append-only, particionada, escritura asíncrona, retención.
- [ ] Eliminar la duplicación de §9 / §12.1 y corregir la numeración duplicada de las dos §20.

---

## 4. HU-01 — Registro de Usuario

### 4.1 Visión de Producto y Fit de Negocio

**No Ready (4,0/10). No es una historia, es una épica.**

Contiene: creación de cuenta, política de contraseña, captura de datos médicos, envío de OTP, verificación de OTP, gestión de reintentos y expiración, recordatorio programado a 24 horas, reglas de autorización para solicitar servicios y auditoría. Son 17 pasos de flujo, 7 reglas de negocio y 13 criterios.

Bajo **INVEST** falla en **S** (small) y en **E** (estimable): ningún equipo puede comprometerla en un sprint con confianza, y los dos bloques marcados "pendiente de definición" dentro de los flujos alternativos hacen que el alcance real sea desconocido.

**Los datos médicos están en el lugar equivocado del embudo.** Pedir tipo de sangre, RH e "información médica relevante" en el registro tiene dos problemas:

1. **Conversión:** fricción en el paso donde el usuario aún no ha recibido ningún valor.
2. **Legal y más grave:** el principio de finalidad de la Ley 1581 exige que la recolección responda a una finalidad determinada y previamente informada. En el registro no hay ninguna solicitud de servicio, luego no hay finalidad concreta. Se recolecta el dato más protegido del ordenamiento "por si acaso".

**Ubicación correcta:** en el momento de crear la primera solicitud, o en un perfil médico opcional posterior a la activación.

**Ambigüedad que impide estimar:** _"información médica relevante definida por el sistema"_. Sin lista, sin tipo de dato, sin cardinalidad.

**Hueco absoluto:** HU-01 crea contraseñas y **no existe ninguna historia de recuperación de contraseña** en todo el corpus, ni definida ni pendiente. Un producto que permite crear credenciales y no permite recuperarlas no es lanzable.

### 4.2 Arquitectura, Escalabilidad y Resiliencia

**El recordatorio de 24 horas es un diseño de batch tratado como una frase.** La implementación ingenua (cron horario que escanea `usuarios WHERE verificado = false AND creado_en < now() - 24h`) tiene tres defectos:

1. Escaneo creciente sobre una tabla que solo crece.
2. No es idempotente: reenvía en cada corrida salvo que exista un campo de control que la especificación no menciona.
3. Sin tope: un usuario que nunca verifica recibe correos indefinidamente, degradando la reputación del dominio de envío.

Especificar: índice parcial sobre no verificados, marca `recordatorio_enviado_en`, número máximo de recordatorios, y purga de la cuenta no verificada tras N días.

**Validación de unicidad bajo pico.** `correo` y `documento` requieren índices únicos. Con miles de registros por minuto, la forma correcta es **insertar y capturar la violación de unicidad**, no consultar antes (otra vez TOCTOU).

**La sesión emitida antes de verificar necesita contrato explícito.** Si el token lleva `email_verified: false` y dura 30 días, el usuario que verifica seguirá bloqueado hasta que expire. Si el bloqueo se consulta en base de datos en cada petición, se añade una lectura por request. El documento no elige. Solución: token de vida corta con refresh, y la regla de autorización **centralizada en una política**, no en un `if` dentro del controlador de creación de solicitudes.

### 4.3 Edge cases y manejo de errores

| #    | Escenario no cubierto                          | Impacto                                                                    | Probabilidad | Mitigación                                                  |
| ---- | ---------------------------------------------- | -------------------------------------------------------------------------- | ------------ | ----------------------------------------------------------- |
| U-01 | Reenvío de código sin invalidar el anterior    | N códigos válidos a la vez; probabilidad de acierto × N                    | Alta         | Invalidar el anterior en cada emisión                       |
| U-02 | Reenvío sin límite                             | Coste de envío, bombardeo de bandeja, pérdida de reputación de dominio     | Alta         | Máx. 3 reenvíos/hora y 10/día, con backoff                  |
| U-03 | Rebote duro del correo                         | El usuario nunca recibe el código; cuenta muerta                           | Media        | Detectar rebotes y permitir corregir el correo              |
| U-04 | Error tipográfico en el correo                 | Código enviado a un tercero, que puede activar una cuenta con datos ajenos | Media-alta   | Cambio de correo previo a la verificación, con nuevo código |
| U-05 | Registro con cédula de otra persona            | Suplantación con acceso posterior a servicios de salud                     | Media        | Validación de identidad, no solo unicidad                   |
| U-06 | Registro masivo automatizado                   | Coste de envío, contaminación de la base                                   | Alta         | CAPTCHA o prueba de trabajo, límite por IP                  |
| U-07 | El usuario solicita eliminar su cuenta         | No hay flujo de supresión; incumplimiento del derecho del titular          | Media        | Flujo de supresión con retención legal diferenciada         |
| U-08 | Datos médicos de un tercero sin autorización   | Tratamiento de dato sensible sin base legal                                | Alta         | Declaración de representación y notificación al titular     |
| U-09 | Cuenta creada y nunca verificada durante meses | Base contaminada, coste de almacenamiento y de recordatorios               | Alta         | Purga automática tras N días con aviso previo               |

### 4.4 Seguridad y Cumplimiento

#### 4.4.1 La política de contraseña contradice el estándar en dos direcciones opuestas

**Demostrable con el texto del propio documento.** RN-06 exige letras, números y al menos un carácter especial, y **no fija longitud mínima**. La cadena `a1!`, de tres caracteres, cumple los tres requisitos literales y el backend la aceptaría.

Contra NIST SP 800-63B (revisión 4):

| Aspecto                            | NIST SP 800-63B-4                                                                                                               | HU-01 RN-06                                 | Veredicto                |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- | ------------------------ |
| Longitud mínima                    | 15 caracteres si la contraseña es el único factor; 8 si forma parte de MFA                                                      | No especificada                             | **Incumple por omisión** |
| Longitud máxima                    | Al menos 64                                                                                                                     | No especificada                             | Incumple por omisión     |
| Reglas de composición              | _No deben imponerse_ otros requisitos de composición (la revisión 4 endureció el lenguaje de "no se recomienda" a "no se debe") | Obliga letras + números + carácter especial | **Incumple por exceso**  |
| Lista de contraseñas comprometidas | Obligatoria; si aparece en la lista, se exige elegir otra                                                                       | No contemplada                              | Incumple                 |

Es decir: **la especificación exige justo lo que el estándar prohíbe y omite justo lo que el estándar exige.**

**RN-06 reescrito:**

> Mínimo 12 caracteres (15 recomendado mientras no exista MFA), máximo de al menos 64, se aceptan todos los caracteres imprimibles y el espacio, sin reglas de composición, y se rechaza toda contraseña presente en la lista de credenciales filtradas. Validación autoritativa en backend.

#### 4.4.2 El OTP sin TTL ni límite de intentos es una cuenta abierta

El documento deja explícitamente pendientes "las reglas de expiración y cantidad máxima de intentos". Aritmética de esa omisión (espacio de 10⁶ combinaciones):

| Configuración                                               | Resultado                                              |
| ----------------------------------------------------------- | ------------------------------------------------------ |
| Sin límite, sin expiración, 50 req/s desde un origen        | Espacio agotado en ~5,5 h; esperanza de acierto ~2,8 h |
| Sin límite, sin expiración, 20 hilos paralelos              | **~17 minutos por cuenta**                             |
| TTL 10 min + 5 intentos                                     | Probabilidad de acierto = 5 × 10⁻⁶ (**0,0005 %**)      |
| TTL 10 min + 5 intentos, pero N códigos válidos simultáneos | Probabilidad × N                                       |

Valores del estándar (NIST SP 800-63B-4), no opinables:

- La autenticación fuera de banda es **inválida si no se completa en 10 minutos**.
- El verificador debe aceptar un secreto dado **una sola vez** durante su periodo de validez (resistencia a repetición).
- Debe generar secretos aleatorios de **al menos seis dígitos decimales** con un generador aprobado.
- Si el secreto tiene menos de 64 bits, **debe** implementar limitación de tasa. El tope de intentos consecutivos fallidos es **100 por cuenta**, descrito explícitamente como límite superior que las organizaciones pueden reducir.

> **Nota de diseño.** El estándar desaconseja entregar el secreto por correo en escenarios de autenticación fuera de banda. Aquí el código verifica una dirección, no autentica una sesión. Conviene que la especificación lo diga de forma explícita para que nadie lo reutilice más tarde como segundo factor.

#### 4.4.3 Enumeración de usuarios que revela un dato sensible por sí mismo

RN-01 y RN-02 obligan a rechazar el registro cuando el correo o el documento ya existen. Un atacante con una lista de cédulas puede determinar quién tiene cuenta en la plataforma. En un dominio genérico sería una molestia; aquí **el hecho de estar registrado en una plataforma de servicios de enfermería es un indicio sobre el estado de salud de la persona**, es decir, la respuesta del formulario es en sí misma revelación de un dato sensible a un tercero no autorizado.

**Mitigación:** mensaje genérico idéntico en ambos casos, notificación por correo al titular del dato existente, y rate limiting del endpoint de registro.

#### 4.4.4 Otras ausencias

Autorización expresa de tratamiento de datos sensibles como paso del flujo; aviso de privacidad enlazado; aceptación de términos con versión y sello de tiempo; política de conservación; y el mencionado flujo de recuperación de contraseña.

### 4.5 Criterios BDD faltantes — HU-01

```gherkin
CA-U-01  Longitud mínima de contraseña
  Dado un usuario en el formulario de registro
  Cuando introduce la contraseña "a1!" que cumple letras, números y símbolo
  Entonces el backend rechaza el registro con el mensaje de longitud mínima
  Y no se crea ninguna cuenta

CA-U-02  Contraseña comprometida
  Dado un usuario que introduce una contraseña de 15 caracteres presente
        en la lista de credenciales filtradas
  Cuando envía el formulario
  Entonces el sistema la rechaza indicando el motivo
  Y le permite elegir otra sin perder el resto del formulario

CA-U-03  Expiración del código de verificación
  Dado un código de verificación emitido hace 11 minutos
  Cuando el usuario lo introduce
  Entonces el sistema responde que el código expiró
  Y ofrece emitir uno nuevo
  Y el código anterior queda invalidado de forma permanente

CA-U-04  Límite de intentos de verificación
  Dado un usuario que ha fallado 5 intentos consecutivos sobre el mismo código
  Cuando intenta un sexto
  Entonces el sistema bloquea la verificación durante 15 minutos
  Y registra el evento en auditoría con IP y agente de usuario
  Y notifica al titular del correo

CA-U-05  Un solo código válido a la vez
  Dado un usuario con un código vigente
  Cuando solicita el reenvío del código
  Entonces el código anterior queda invalidado inmediatamente
  Y solo el nuevo código es aceptado

CA-U-06  Límite de reenvíos
  Dado un usuario que ha solicitado 3 reenvíos en la última hora
  Cuando solicita un cuarto
  Entonces el sistema lo rechaza indicando cuándo podrá intentarlo de nuevo
  Y no se genera ningún envío facturable

CA-U-07  Uso único del código
  Dado un código de verificación ya utilizado con éxito
  Cuando se envía de nuevo el mismo código
  Entonces el sistema lo rechaza
  Y registra el intento como posible reutilización

CA-U-08  Respuesta no enumerable
  Dado un correo que ya está asociado a una cuenta existente
  Cuando alguien intenta registrarse con ese mismo correo
  Entonces el sistema responde con el mismo mensaje genérico que usaría
        para un correo nuevo
  Y envía al titular existente un aviso de intento de registro
  Y no crea una segunda cuenta

CA-U-09  Autorización de tratamiento de datos sensibles
  Dado un usuario que decide registrar su tipo de sangre y RH
  Cuando guarda esa información
  Entonces el sistema exige una autorización expresa y separada para
        datos sensibles
  Y almacena la versión del aviso de privacidad, la fecha y el canal
  Y si el usuario no la otorga, la cuenta se crea igualmente sin esos datos

CA-U-10  Datos médicos de un tercero
  Dado un usuario que registra datos médicos de otra persona
  Cuando envía la solicitud
  Entonces el sistema le exige declarar que cuenta con autorización del titular
  Y registra esa declaración con fecha y hora en auditoría

CA-U-11  Recordatorio idempotente
  Dado un usuario no verificado que ya recibió el recordatorio de 24 horas
  Cuando el proceso de recordatorios se ejecuta de nuevo
  Entonces no se envía un segundo correo
  Y tras 3 recordatorios el sistema deja de enviar y marca la cuenta
        como candidata a purga

CA-U-12  Bloqueo centralizado por correo no verificado
  Dado un usuario autenticado con el correo sin verificar
  Cuando invoca cualquier endpoint de creación de solicitud de servicio
  Entonces la API responde 403 con el código EMAIL_NO_VERIFICADO
  Y la verificación de esta regla se realiza en la capa de autorización,
    no en el controlador

CA-U-13  Corrección del correo antes de verificar
  Dado un usuario que registró un correo con un error tipográfico
  Cuando solicita cambiarlo antes de completar la verificación
  Entonces el sistema invalida el código anterior
  Y emite uno nuevo al correo corregido
  Y registra el cambio en auditoría

CA-U-14  Límite de registros por origen
  Dado un mismo origen que ha creado 5 cuentas en la última hora
  Cuando intenta crear una sexta
  Entonces el sistema exige una verificación adicional antes de continuar
```

### 4.6 Checklist de cambios a HU-01

- [ ] Reescribir RN-06 conforme a NIST SP 800-63B-4 (longitud mínima, sin reglas de composición, lista de comprometidas).
- [ ] Fijar TTL de 10 minutos, 5 intentos, uso único y un solo código vigente.
- [ ] Añadir límites de reenvío y de registro por origen.
- [ ] Mover los datos médicos fuera del registro, o hacerlos explícitamente posteriores a la verificación con autorización separada.
- [ ] Definir el catálogo concreto de "información médica relevante".
- [ ] Añadir aceptación de términos y autorización de tratamiento de datos sensibles al flujo.
- [ ] Añadir respuestas no enumerables en las validaciones de unicidad.
- [ ] Especificar el diseño del recordatorio (índice parcial, marca de control, tope, purga).
- [ ] Especificar el contrato de sesión pre-verificación y centralizar la regla de autorización.
- [ ] Dividir la historia en cinco (ver §10).

---

## 5. HU-02 — Registro de Enfermero

### 5.1 Visión de Producto y Fit de Negocio

**No Ready (3,0/10). La historia más débil del conjunto, y su debilidad es de concepto.**

#### 5.1.1 Verifica credenciales sin verificar identidad, y eso es peor que no verificar

Produce una etiqueta de confianza que el usuario final interpretará como garantía.

Los documentos obligatorios son cuatro (documento de identidad, foto, título profesional, tarjeta profesional) y los cuatro son imágenes autoportadas. No hay prueba de vida, no hay comparación biométrica entre la foto y el documento, y no hay contraste contra ninguna fuente externa. **Un atacante que obtenga el número de cédula y el número de tarjeta profesional de una enfermera real** —datos que circulan en certificados laborales y directorios— **puede registrarse suplantándola**, y el superadministrador vería documentos coherentes y aprobaría.

#### 5.1.2 La fuente de verdad existe, es pública, es gratuita, y no se usa

La consulta del **ReTHUS** es pública y gratuita en el portal de consultas públicas del SISPRO del Ministerio de Salud. Con el tipo y número de documento devuelve:

- profesión u ocupación registrada,
- entidad que hizo la inscripción,
- estado de la inscripción,
- **sanciones ético-disciplinarias reportadas** por los tribunales del área de la salud.

No requiere usuario ni autorización del profesional. La base es la **Ley 1164 de 2007**: la inscripción implica que la persona está autorizada para el ejercicio de una profesión u ocupación del área de la salud.

**La inversión de la relación coste-beneficio es completa:** la especificación pide una fotografía de una tarjeta, falsificable en minutos, y deja **opcional** el certificado de antecedentes disciplinarios, que es exactamente el dato que el ReTHUS entrega gratis.

**Lo que el ReTHUS no resuelve** (para no sobrevender la mitigación): no certifica experiencia, no valida educación continua, no reemplaza certificados específicos de un servicio concreto y no sustituye la verificación de antecedentes judiciales. Siguen haciendo falta verificación de identidad y verificación de antecedentes penales. Pero el ReTHUS convierte el 80 % del trabajo manual de HU-03 en una llamada.

#### 5.1.3 Antecedentes disciplinarios opcionales, en un servicio de cuidado domiciliario

Precedente directo y documentado, el mismo problema castigado tres veces:

| Año  | Autoridad                         | Monto                                                     | Motivo                                                                                                                                                                                        |
| ---- | --------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2018 | Fiscalía General de Massachusetts | USD 480.000                                               | Las verificaciones no cubrían los tribunales de distrito, donde reside la mayoría de los registros de delitos menores                                                                         |
| 2020 | Fiscales de San Francisco y Marin | USD 1.000.000 (700 K en sanciones + 300 K en restitución) | Tergiversación del alcance de la búsqueda en el registro de ofensores sexuales                                                                                                                |
| 2024 | FTC                               | USD 8.500.000                                             | Prácticas engañosas; la demanda sostiene que se hizo creer a los consumidores que los cuidadores habían pasado verificaciones rigurosas cuando se aprobaban personas con antecedentes penales |

El origen común es estructural: se trata de un marketplace, no de un empleador. Esta plataforma está en la misma posición y con mayor exposición, porque el servicio es clínico y ocurre dentro del domicilio.

#### 5.1.4 No distingue auxiliar de enfermería de profesional de enfermería

Son figuras con competencias legalmente distintas (Ley 266 de 1996, Ley 911 de 2004). HU-04 crea el tipo "Atención domiciliaria" sin nivel de competencia asociado. **Resultado combinado: un auxiliar puede aceptar un servicio que requiere un acto reservado al profesional.** La plataforma asume esa responsabilidad clínica y legal sin saberlo.

#### 5.1.5 Cruce demoledor con HU-04

- **"Acompañamiento y transporte"**: HU-02 no pide licencia de conducción, tarjeta de propiedad, SOAT, revisión técnico-mecánica ni seguro de responsabilidad civil. Un enfermero aprobado puede aceptar un servicio en el que transportará a un paciente **en un vehículo que nadie ha verificado que exista**.
- **"Atención hospitalaria"**: exige convenio con la IPS receptora, que tiene su propio régimen de habilitación y de talento humano. No hay nada al respecto.

#### 5.1.6 Fricción de onboarding

El flujo principal es lineal de 9 pasos, con cuatro cargas de archivo, sin estado borrador, típicamente desde el móvil. No poder guardar y continuar es una causa clásica de abandono en onboarding documental.

> **Hipótesis a validar con instrumentación** (no dispongo de una cifra verificada para este segmento). La contramedida —persistencia parcial— es barata y no tiene contraindicación, así que conviene implementarla igualmente y medir.

### 5.2 Arquitectura, Escalabilidad y Resiliencia

- **Carga de archivos sin ninguna restricción declarada.** Sin tamaño máximo, sin tipos permitidos, sin número máximo por perfil. Con "documentación adicional" ilimitada y opcional, un solo usuario puede consumir almacenamiento sin tope: vector de denegación de servicio y coste operativo no dimensionado.
- **Sin estrategia de almacenamiento.** No se dice si los archivos van a un almacén de objetos con URLs firmadas de vida corta o si se sirven desde el backend. La segunda opción convierte cada revisión de perfil en tráfico que atraviesa la aplicación.
- **Sin procesamiento asíncrono.** Escaneo antivirus, miniaturas y normalización deben ser asíncronos. Si son síncronos, el registro tarda decenas de segundos y el timeout del cliente produce registros parciales.
- **"Zona de residencia" sin tipo de dato.** ¿Texto libre? ¿Código DANE? ¿Polígono? Sin normalizar es inutilizable para cualquier matching futuro, y migrar texto libre a datos geográficos es una de las peores migraciones que existen porque requiere intervención manual.
- **Prohibición explícita del radio de servicio.** RN-05 dice que solo se almacena la zona de residencia y que esta "no representa un rango máximo de desplazamiento". Combinado con §9 del contexto, que muestra "distancia aproximada" antes de aceptar, hay contradicción: para calcular distancia hacen falta geocodificación del origen y una referencia geográfica del enfermero. Y el feed de HU-07 sería **global**: todos los enfermeros del país ven todos los servicios, con fan-out de lectura O(enfermeros × servicios) y tasa de aceptación en caída. **Es una decisión de producto tomada implícitamente dentro de una historia de registro.**

### 5.3 Edge cases y manejo de errores

| #    | Escenario no cubierto                                        | Impacto                                         | Probabilidad | Mitigación                                                         |
| ---- | ------------------------------------------------------------ | ----------------------------------------------- | ------------ | ------------------------------------------------------------------ |
| E-01 | Dos enfermeros se registran con la misma tarjeta profesional | Suplantación indetectada                        | Media        | Unicidad sobre el número de tarjeta y contraste con ReTHUS         |
| E-02 | Documento cargado con malware                                | Compromiso del equipo del revisor               | Media        | Escaneo antivirus obligatorio antes de exponer el archivo          |
| E-03 | Archivo con extensión falsificada                            | Bypass de la validación por extensión           | Media        | Validación por bytes mágicos, no por extensión                     |
| E-04 | Registro interrumpido a mitad de la carga                    | Perfil incompleto o abandono total              | Alta         | Estado `BORRADOR` con reanudación                                  |
| E-05 | Tarjeta profesional vencida al momento del registro          | Enfermero aprobado sin habilitación vigente     | Media        | Consulta de vigencia contra ReTHUS y fecha de caducidad almacenada |
| E-06 | Aceptación de un servicio de transporte sin vehículo         | Riesgo clínico y de responsabilidad civil       | Media-alta   | Requisitos documentales por tipo de servicio                       |
| E-07 | Auxiliar acepta un servicio que exige profesional            | Acto reservado ejecutado por quien no puede     | Media-alta   | Nivel de competencia en el perfil y en el tipo de servicio         |
| E-08 | El enfermero quiere también ser usuario                      | Bloqueado por unicidad de documento y rol único | Alta         | `Persona ↔ Rol` N:M                                                |
| E-09 | Enfermero fuera de la ciudad de operación                    | Acepta servicios que no puede atender           | Media        | Zona normalizada + reglas de cobertura                             |

### 5.4 Seguridad y Cumplimiento

- **Prueba de identidad ausente.** El estándar de referencia es NIST SP 800-63A (identity proofing) y el corpus no lo menciona. Para un servicio que envía a una persona al domicilio de un paciente vulnerable, el nivel razonable exige comparación biométrica con prueba de vida.
- **Documentos sensibles sin controles:** sin cifrado en reposo, sin URLs firmadas con expiración, sin marca de agua, sin registro de quién visualizó cada documento.
- **Sin autorización de tratamiento ni términos.** El enfermero entrega documento de identidad, foto y credenciales profesionales sin que el sistema recoja ninguna base legal.
- **Sin contrato de prestador.** Relevante también para la discusión sobre clasificación laboral de trabajadores de plataformas, un asunto abierto en Colombia. **Hipótesis a validar con asesoría antes de escalar.**

### 5.5 Criterios BDD faltantes — HU-02

```gherkin
CA-E-01  Verificación automática contra ReTHUS
  Dado un enfermero que envía su perfil a revisión
  Cuando el sistema consulta el ReTHUS con su tipo y número de documento
  Entonces almacena la profesión registrada, la entidad inscriptora,
        el estado y la existencia de sanciones
  Y adjunta ese resultado al expediente que verá el superadministrador
  Y si el documento no aparece en el ReTHUS, el perfil no puede enviarse
        a revisión

CA-E-02  Discrepancia entre lo declarado y el ReTHUS
  Dado un enfermero que declara ser profesional de enfermería
  Cuando el ReTHUS lo registra como auxiliar de enfermería
  Entonces el sistema marca el perfil con una alerta de discrepancia
  Y el superadministrador no puede aprobarlo sin resolver la alerta

CA-E-03  Sanción ético-disciplinaria activa
  Dado un enfermero cuyo registro ReTHUS reporta una sanción vigente
  Cuando envía su perfil a revisión
  Entonces el sistema rechaza automáticamente el envío
  Y registra el motivo en auditoría

CA-E-04  Indisponibilidad de la fuente externa
  Dado que el servicio de consulta del ReTHUS no responde
  Cuando un enfermero envía su perfil a revisión
  Entonces el perfil se encola como "verificación externa pendiente"
  Y se reintenta de forma automática con backoff
  Y no se aprueba ningún perfil sin resultado de la consulta

CA-E-05  Unicidad de tarjeta profesional
  Dado un número de tarjeta profesional ya asociado a otro perfil
  Cuando un segundo enfermero intenta registrarlo
  Entonces el sistema rechaza el registro
  Y genera una alerta de posible suplantación para el superadministrador

CA-E-06  Prueba de identidad
  Dado un enfermero que carga su documento de identidad y su foto
  Cuando el sistema ejecuta la comparación biométrica con prueba de vida
  Y el resultado está por debajo del umbral definido
  Entonces el perfil no puede enviarse a revisión
  Y se le permite repetir la captura hasta 3 veces

CA-E-07  Validación real del tipo de archivo
  Dado un archivo cuyo contenido real no corresponde a su extensión
  Cuando el enfermero intenta cargarlo
  Entonces el sistema lo rechaza indicando los formatos permitidos
  Y no lo persiste

CA-E-08  Escaneo antivirus
  Dado un documento recién cargado
  Cuando el escaneo antimalware detecta una amenaza
  Entonces el archivo se pone en cuarentena y no se expone a ningún revisor
  Y el enfermero recibe una notificación pidiendo un archivo nuevo

CA-E-09  Límites de carga
  Dado un enfermero que ya cargó 20 documentos adicionales
  Cuando intenta cargar uno más
  Entonces el sistema lo rechaza indicando el límite
  Y cada archivo individual está limitado a 10 MB

CA-E-10  Borrador reanudable
  Dado un enfermero que completó la información personal y abandonó
  Cuando vuelve a ingresar en los 30 días siguientes
  Entonces recupera el formulario en el punto donde lo dejó,
        incluidos los documentos ya cargados

CA-E-11  Requisitos documentales por tipo de servicio
  Dado un enfermero aprobado que no ha acreditado licencia de conducción,
        tarjeta de propiedad y SOAT vigentes
  Cuando intenta aceptar un servicio de tipo "Acompañamiento y transporte"
  Entonces el sistema se lo impide indicando qué documentos faltan

CA-E-12  Nivel de competencia
  Dado un enfermero registrado en el ReTHUS como auxiliar de enfermería
  Cuando consulta el feed de servicios disponibles
  Entonces no ve servicios cuyo tipo exija nivel profesional

CA-E-13  Zona de residencia normalizada
  Dado un enfermero que indica su zona de residencia
  Cuando guarda el formulario
  Entonces el sistema almacena el código DANE de departamento y municipio
        y unas coordenadas aproximadas
  Y no acepta texto libre

CA-E-14  Aceptación de términos de prestador
  Dado un enfermero que envía su perfil a revisión
  Cuando confirma el envío
  Entonces el sistema registra la versión de los términos aceptados,
        la fecha, la hora y el canal
  Y sin esa aceptación el envío no se completa
```

### 5.6 Checklist de cambios a HU-02

- [ ] Integrar consulta al ReTHUS como paso obligatorio del envío a revisión.
- [ ] Hacer obligatorios los antecedentes disciplinarios (vía ReTHUS) y añadir antecedentes judiciales.
- [ ] Añadir prueba de identidad con comparación biométrica y prueba de vida.
- [ ] Añadir unicidad sobre el número de tarjeta profesional.
- [ ] Especificar límites, tipos permitidos, validación por bytes mágicos y escaneo antivirus asíncrono.
- [ ] Añadir estado `BORRADOR` con reanudación.
- [ ] Normalizar la zona de residencia (códigos DANE + coordenadas).
- [ ] Añadir nivel de competencia (auxiliar / profesional / especialista) al perfil.
- [ ] Añadir requisitos documentales condicionados por tipo de servicio.
- [ ] Añadir aceptación de términos de prestador y autorización de tratamiento de datos.
- [ ] Reabrir la decisión sobre el radio de servicio, que hoy bloquea cualquier matching geográfico.
- [ ] Dividir la historia en cuatro (ver §10).

---

## 6. HU-03 — Verificación y Aprobación de Enfermero

### 6.1 Visión de Producto y Fit de Negocio

**No Ready (3,5/10).** Es la historia más completa en flujos y aun así falla en lo esencial: **es un cuello de botella humano en el camino crítico del crecimiento de la oferta, sin instrumentación, sin redundancia y sin capacidad definida.**

### 6.2 Arquitectura, Escalabilidad y Resiliencia

#### 6.2.1 El SLA de 24 a 36 horas se rompe con unos 20 registros al día

Modelo M/M/1 con un único revisor (el documento nunca dice que haya más de uno). Es **optimista**, porque asume un solo tipo de tarea y ninguna interrupción.

**Capacidad de servicio.** La revisión implica, según el propio documento, examinar información personal, documentos, información profesional, experiencia y documentación adicional, y en su caso redactar un comentario. Con 12 minutos por perfil (conservador) y 6 horas netas al día:

```
μ = 360 min / 12 min = 30 perfiles/día
W = 1 / (μ − λ)
```

| Registros nuevos/día (λ) | Utilización ρ | Tiempo total W | ¿Cumple SLA de 24 h? |
| ------------------------ | ------------- | -------------- | -------------------- |
| 10                       | 0,33          | 1,2 h          | Sí                   |
| 15                       | 0,50          | 1,9 h          | Sí                   |
| 20                       | 0,67          | 2,4 h          | Sí                   |
| 25                       | 0,83          | 4,8 h          | Sí                   |
| 28                       | 0,93          | 12 h           | Al límite            |
| 29                       | 0,97          | 24 h           | **Rompe**            |
| 30                       | 1,00          | ∞              | **Colapsa**          |

**Factor que el documento crea y no contabiliza: los reprocesos.** El flujo de corrección devuelve el perfil a la cola. Con un 35 % de perfiles que requieren al menos una corrección, λ_eff = 1,35 λ, y el punto de ruptura baja de 29 a **21,5 registros nuevos por día**.

**Factor que lo hunde: el superadministrador no solo revisa perfiles.** Según §22 del contexto, el mismo rol gestiona tipos de servicio, configura ventanas de datos sensibles y de PIN, y consulta auditoría. Además es quien debe atender los bloqueos de PIN tras 5 intentos fallidos, que son incidentes en tiempo real con un paciente esperando. Con 2 horas diarias de revisión en lugar de 6: μ = 10/día y el SLA se rompe con **7 registros nuevos por día**.

**Conclusión:** el SLA de 24-36 h no es un objetivo operativo, es una promesa que el diseño hace imposible desde la primera semana de adquisición de oferta. Y como el _time to first job_ determina la retención del lado de la oferta, cada día de espera es abandono directo.

#### 6.2.2 No hay forma de saber que el SLA se está rompiendo

La historia no define **ni una métrica**: ni tamaño de cola, ni edad del elemento más antiguo, ni percentil de tiempo de revisión, ni tasa de aprobación, ni alerta por incumplimiento. **Un SLA sin instrumentación no es un SLA.**

#### 6.2.3 El reloj del SLA no está definido

¿Corre desde el primer envío o desde cada reenvío tras corrección? ¿Se pausa en fines de semana y festivos? Sin esa definición, dos personas medirán el mismo SLA de forma distinta.

#### 6.2.4 Otros problemas

- **Sin cola priorizada ni asignación.** Dos revisores trabajando en paralelo revisarían el mismo perfil. Hace falta _claim_ con TTL y liberación automática.
- **Sin ciclos máximos de corrección.** El bucle corrección → reenvío → revisión no tiene tope: es un vector de agotamiento del recurso más escaso del sistema.
- **Las URLs de documentos no expiran.** Nada impide que un enlace compartido siga siendo válido indefinidamente.

### 6.3 Edge cases y manejo de errores

| #    | Escenario no cubierto                                  | Impacto                                               | Probabilidad            | Mitigación                                              |
| ---- | ------------------------------------------------------ | ----------------------------------------------------- | ----------------------- | ------------------------------------------------------- |
| V-01 | Un enfermero aprobado es sancionado después            | Sigue prestando servicios sin habilitación            | Media                   | Re-verificación periódica y estado `SUSPENDIDO`         |
| V-02 | Hay que revocar a un enfermero con servicios ASIGNADOS | Comportamiento indefinido para pacientes ya agendados | Media                   | Regla explícita: devolver a PUBLICADO y notificar       |
| V-03 | Dos revisores abren el mismo perfil                    | Trabajo duplicado, decisiones contradictorias         | Media                   | _Claim_ con expiración                                  |
| V-04 | El único superadministrador se incapacita              | Cola congelada, oferta detenida                       | Baja, impacto total     | Mínimo dos revisores y escalamiento                     |
| V-05 | El superadministrador se aprueba a sí mismo            | Ausencia de segregación de funciones                  | Baja                    | Prohibición explícita y principio de cuatro ojos        |
| V-06 | Bucle infinito de correcciones                         | Agotamiento del revisor                               | Media                   | Máximo 3 ciclos, luego rechazo definitivo con apelación |
| V-07 | La tarjeta caduca con el perfil ya aprobado            | Prestación sin habilitación vigente                   | Alta en horizonte anual | Fecha de vigencia y suspensión automática               |
| V-08 | Decisión errónea por documento mal leído               | Sin trazabilidad de qué vio el revisor                | Media                   | Registro de acceso a cada documento con marca de tiempo |
| V-09 | Rechazo sin vía de apelación                           | Pérdida de oferta legítima y riesgo reputacional      | Media                   | Flujo de apelación con revisor distinto                 |

### 6.4 Seguridad y Cumplimiento

- **El acceso de lectura a datos sensibles también es tratamiento y no se audita.** RN-03 concede acceso a toda la información. La auditoría descrita registra aprobación, rechazo y corrección, pero **no registra quién abrió qué documento y cuándo**. Bajo el régimen de la Ley 1581, la consulta de datos sensibles es una operación de tratamiento y debe ser trazable.
- **Sin marca de agua ni control de descarga.** Un revisor puede descargar cédulas y tarjetas profesionales sin dejar rastro.
- **Sin MFA en el rol con más privilegio.** El superadministrador aprueba prestadores, lee todos los datos sensibles y modifica las ventanas de exposición de información médica. Comprometer esa cuenta compromete la plataforma entera.
- **Sin segregación de funciones.** Un rol que aprueba, configura y audita, y cuyas acciones registra la misma tabla que él consulta, no tiene control interno.

### 6.5 Criterios BDD faltantes — HU-03

```gherkin
CA-V-01  Medición y alerta del SLA
  Dado un perfil enviado a revisión
  Cuando transcurren 24 horas sin decisión
  Entonces el sistema emite una alerta al canal de operaciones
  Y el perfil se marca como en riesgo de SLA
  Y el panel expone la edad del elemento más antiguo y el p95 de revisión

CA-V-02  Definición del reloj del SLA
  Dado un perfil devuelto por corrección y reenviado por el enfermero
  Cuando se recalcula el SLA
  Entonces el reloj se reinicia desde el reenvío
  Y el tiempo acumulado de ciclos previos se conserva como métrica aparte

CA-V-03  Reclamo exclusivo del perfil
  Dado un perfil pendiente de revisión
  Cuando un superadministrador lo abre
  Entonces queda reclamado por él durante 30 minutos
  Y otro superadministrador que intente abrirlo ve que está en revisión
  Y si no hay decisión en 30 minutos, vuelve a la cola

CA-V-04  Suspensión de un enfermero aprobado
  Dado un enfermero en estado APROBADO
  Cuando el superadministrador lo suspende con un motivo registrado
  Entonces el enfermero no puede aceptar nuevos servicios de inmediato
  Y sus servicios ya ASIGNADOS y no iniciados se devuelven a PUBLICADO
  Y tanto el enfermero como los usuarios afectados son notificados

CA-V-05  Servicios en curso durante una revocación
  Dado un enfermero con un servicio en estado EN_CURSO
  Cuando se le revoca la aprobación
  Entonces el servicio en curso puede completarse con normalidad
  Y el enfermero no puede aceptar ningún servicio adicional

CA-V-06  Re-verificación periódica
  Dado un enfermero aprobado hace 12 meses
  Cuando el proceso de re-verificación consulta el ReTHUS
  Y detecta una sanción o una inscripción no vigente
  Entonces el perfil pasa automáticamente a SUSPENDIDO
  Y se genera un caso para revisión humana

CA-V-07  Límite de ciclos de corrección
  Dado un perfil que ya ha pasado por 3 ciclos de corrección
  Cuando el superadministrador solicita una cuarta corrección
  Entonces el sistema exige convertirla en rechazo definitivo
  Y habilita la vía de apelación

CA-V-08  Auditoría de acceso a documentos
  Dado un superadministrador que abre el documento de identidad de un enfermero
  Cuando se genera la URL de acceso
  Entonces la URL expira en 5 minutos
  Y el acceso queda registrado con identificador del revisor, documento,
        fecha, hora e IP
  Y el documento se muestra con una marca de agua que incluye
        al revisor y la fecha

CA-V-09  Segregación de funciones
  Dado un superadministrador cuyo propio perfil de enfermero está en revisión
  Cuando intenta aprobarlo
  Entonces el sistema lo rechaza
  Y deriva el caso a otro superadministrador

CA-V-10  Inmutabilidad frente al rol auditado
  Dado un superadministrador autenticado
  Cuando intenta eliminar o alterar un registro de auditoría de sus
        propias acciones
  Entonces la operación es rechazada y el intento queda registrado

CA-V-11  Autenticación reforzada del rol administrador
  Dado un superadministrador que inicia sesión
  Cuando introduce su contraseña correctamente
  Entonces el sistema exige un segundo factor antes de conceder la sesión

CA-V-12  Comentario obligatorio y estructurado
  Dado un superadministrador que rechaza o solicita corrección
  Cuando intenta confirmar sin seleccionar al menos un motivo tipificado
  Entonces el sistema lo impide
  Y el comentario libre es adicional al motivo tipificado, no sustitutivo
```

### 6.6 Checklist de cambios a HU-03

- [ ] Sustituir la revisión íntegramente manual por revisión asistida: ReTHUS + biometría automáticas, humano solo para excepciones.
- [ ] Definir número mínimo de revisores y procedimiento de escalamiento.
- [ ] Definir el reloj del SLA (inicio, reinicio, pausas) y su instrumentación.
- [ ] Añadir _claim_ exclusivo con TTL sobre cada perfil en revisión.
- [ ] Añadir estados `SUSPENDIDO` y `REVOCADO` con reglas sobre servicios en curso y asignados.
- [ ] Añadir re-verificación periódica contra ReTHUS.
- [ ] Añadir límite de ciclos de corrección y vía de apelación.
- [ ] Añadir auditoría de acceso a documentos, URLs firmadas y marca de agua.
- [ ] Añadir MFA y segregación de funciones para el superadministrador.
- [ ] Tipificar los motivos de rechazo y corrección.
- [ ] Definir el canal de notificación (hoy "abierto a la implementación", lo que impide estimar).
- [ ] Dividir la historia en cuatro (ver §10).

---

## 7. HU-04 — Gestión de Tipos de Servicio

### 7.1 Visión de Producto y Fit de Negocio

**Casi Ready (5,5/10), pero no estimable porque se contradice a sí misma.**

El título dice "Gestión". El flujo principal describe una gestión. Y "Fuera de alcance" excluye _"gestión de nuevos tipos de servicio desde la interfaz, si esta funcionalidad se implementa en una fase posterior"_. Con esa frase, la historia puede ser:

- una migración con un endpoint de lectura (media jornada), o
- un módulo administrativo con CRUD, permisos y auditoría (una a dos semanas).

**Un factor de 20 en la estimación depende de una frase ambigua.** Eso por sí solo la saca del DoR.

**Crítica de fondo: el catálogo no soporta ninguno de los requisitos que sus propios tipos implican.** El modelo propuesto tiene id, nombre, descripción, estado y fechas. Pero:

| Requisito implícito                                                | ¿Lo soporta el modelo?                                                                      |
| ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------- |
| "Acompañamiento y transporte" exige vehículo, licencia y seguro    | No: no hay requisitos documentales por tipo                                                 |
| "Atención hospitalaria" exige convenio con IPS y perfil específico | No: no hay restricciones de competencia                                                     |
| El sistema debe mostrar "precio ofrecido" y calcular extensiones   | No: no hay tarifa, duración mínima ni unidad de cobro, y la historia lo excluye del alcance |

Sin esos tres atributos el tipo de servicio es **una etiqueta de texto**. Con ellos, es la pieza que hace posibles HU-05 a HU-12. **La historia está definida al nivel de ambición equivocado: demasiado grande para lo que entrega, demasiado pequeña para lo que el sistema necesita.**

**Falta el atributo que evita el bug clásico de semilla.** El modelo distingue identificador único y nombre, pero no incluye un **código de negocio estable e inmutable** (p. ej. `ATENCION_DOMICILIARIA`). Sin él, las reglas de negocio y el frontend se acoplan al nombre visible (que cambia) o al UUID sembrado (que difiere entre dev, stage y producción). Es el error que produce la incidencia "en producción el tipo de servicio no existe".

**Sobre RN-03.** "No deberán existir dos tipos de servicio **activos** con el mismo nombre" se implementa como índice único parcial (`WHERE estado = 'ACTIVO'`), pero la regla en sí es discutible: permite un "Acompañamiento" activo y otro inactivo, lo que hace ambiguos los reportes históricos y la propia auditoría. Alternativa correcta: **unicidad global sobre el código inmutable**, libertad en el nombre visible.

### 7.2 Arquitectura, Escalabilidad y Resiliencia

El riesgo aquí no es de escala, es de **sobre-ingeniería**. Cuatro filas que cambian casi nunca son el caso canónico de caché en memoria del proceso con invalidación por evento.

Lo que debe especificarse y no está:

- El catálogo se sirve desde caché, no desde una consulta por petición. Con miles de lecturas del feed por segundo, consultar el catálogo cada vez es tráfico gratuito contra la base.
- Protección contra estampida de caché en la invalidación.
- Índice único parcial para RN-03, o mejor, la unicidad global sobre código inmutable.
- Semilla por migración versionada, con verificación en el arranque.

### 7.3 Edge cases y manejo de errores

| #    | Escenario no cubierto                                         | Impacto                                                  | Probabilidad        | Mitigación                                                    |
| ---- | ------------------------------------------------------------- | -------------------------------------------------------- | ------------------- | ------------------------------------------------------------- |
| T-01 | Se desactiva un tipo con solicitudes en PUBLICADO sin asignar | La historia solo cubre históricas; estas quedan en limbo | Media               | Decidir: conservar o cancelar con notificación                |
| T-02 | Se desactiva un tipo con servicios EN_CURSO                   | Sin regla; el servicio debe poder terminar               | Media               | Los servicios en curso continúan siempre                      |
| T-03 | Nombres duplicados entre activo e inactivo                    | Ambigüedad en reportes y auditoría                       | Media               | Código inmutable único global                                 |
| T-04 | El catálogo se sirve desde caché y se desactiva un tipo       | Un tipo inactivo sigue visible hasta la expiración       | Alta                | Invalidación por evento, no solo por TTL                      |
| T-05 | El usuario tiene el formulario abierto y el tipo se desactiva | Crea una solicitud con tipo inválido                     | Media               | Revalidación en el servidor al enviar                         |
| T-06 | Semilla no ejecutada en producción                            | Nadie puede crear solicitudes                            | Baja, impacto total | Semilla por migración versionada + verificación en arranque   |
| T-07 | Desactivación accidental del tipo principal                   | Se apaga la línea de negocio central                     | Baja, impacto total | Confirmación reforzada con conteo de afectados y notificación |

### 7.4 Seguridad y Cumplimiento

Menor superficie de las cuatro historias, pero:

- **RN-08** da acceso exclusivo al superadministrador sin exigir doble autorización para un cambio que afecta a todo el marketplace.
- **Sin control de cambios completo.** La auditoría registra "cambios realizados, cuando aplique", sin especificar que debe guardar valor anterior y valor nuevo.

### 7.5 Criterios BDD faltantes — HU-04

```gherkin
CA-T-01  Código de negocio inmutable
  Dado un tipo de servicio existente
  Cuando cualquier actor intenta modificar su código de negocio
  Entonces la operación es rechazada
  Y el nombre visible sí puede modificarse sin afectar las referencias

CA-T-02  Solicitudes publicadas al desactivar un tipo
  Dado un tipo de servicio con 15 solicitudes en estado PUBLICADO
  Cuando el superadministrador lo desactiva
  Entonces esas 15 solicitudes permanecen aceptables por los enfermeros
  Y no se permite crear nuevas solicitudes con ese tipo
  Y el sistema informa cuántas quedan activas antes de confirmar

CA-T-03  Servicios en curso
  Dado un servicio EN_CURSO cuyo tipo se desactiva
  Cuando llega el momento de finalizarlo
  Entonces el flujo de finalización opera con normalidad

CA-T-04  Revalidación en el envío
  Dado un usuario con el formulario de solicitud abierto
  Cuando el tipo seleccionado se desactiva antes de que envíe
  Entonces el backend rechaza la creación con un mensaje claro
  Y le ofrece elegir un tipo activo sin perder el resto del formulario

CA-T-05  Invalidación de caché
  Dado un tipo de servicio activo presente en la caché del catálogo
  Cuando el superadministrador lo desactiva
  Entonces desaparece del catálogo servido en menos de 5 segundos
        en todas las instancias

CA-T-06  Requisitos documentales por tipo
  Dado el tipo "Acompañamiento y transporte" con requisitos de licencia,
        tarjeta de propiedad y SOAT
  Cuando un enfermero sin esos documentos vigentes consulta el feed
  Entonces no ve servicios de ese tipo

CA-T-07  Nivel de competencia por tipo
  Dado un tipo de servicio que exige nivel profesional
  Cuando un enfermero registrado como auxiliar intenta aceptarlo
  Entonces el sistema lo rechaza indicando el motivo

CA-T-08  Confirmación reforzada
  Dado un tipo de servicio con solicitudes activas
  Cuando el superadministrador intenta desactivarlo
  Entonces el sistema exige una confirmación explícita que muestre
        el número de solicitudes afectadas

CA-T-09  Trazabilidad del cambio
  Dado cualquier cambio sobre un tipo de servicio
  Cuando se persiste
  Entonces la auditoría almacena el valor anterior, el valor nuevo,
        el actor y la marca de tiempo

CA-T-10  Semilla verificada en el arranque
  Dado el arranque de la aplicación
  Cuando alguno de los cuatro tipos definidos para el MVP no existe
  Entonces la aplicación falla el chequeo de salud y lo reporta
```

### 7.6 Modelo de datos propuesto para `TipoServicio`

```text
TipoServicio
  id                       uuid        PK
  codigo                   varchar     UNIQUE, inmutable  (ATENCION_DOMICILIARIA, ...)
  nombre                   varchar     visible, editable
  descripcion              text
  estado                   enum        ACTIVO | INACTIVO
  nivel_competencia_min    enum        AUXILIAR | PROFESIONAL | ESPECIALISTA
  requiere_destino         boolean     (true para acompañamiento y transporte)
  requiere_convenio_ips    boolean     (true para atención hospitalaria)
  documentos_requeridos    jsonb       lista de códigos de documento exigidos al enfermero
  duracion_minima_min      integer
  unidad_cobro             enum        HORA | SERVICIO
  orden_visualizacion      integer
  creado_en / actualizado_en, creado_por / actualizado_por
```

### 7.7 Checklist de cambios a HU-04

- [ ] Resolver la contradicción entre título, flujo principal y "fuera de alcance" en una frase.
- [ ] Añadir `codigo` inmutable con unicidad global y reemplazar RN-03.
- [ ] Añadir nivel de competencia mínimo y documentos requeridos por tipo.
- [ ] Decidir si tarifa y duración entran aquí o en la historia de pagos, y dejarlo escrito.
- [ ] Definir el comportamiento sobre solicitudes en PUBLICADO al desactivar un tipo.
- [ ] Especificar caché con invalidación por evento.
- [ ] Especificar semilla por migración versionada con verificación en arranque.
- [ ] Añadir confirmación reforzada y registro de valor anterior/nuevo en auditoría.

---

## 8. Matriz de priorización para el backlog

Ordenada por severidad. **Bloqueante** significa que no debe escribirse código de la historia hasta resolverlo.

| ID   | Documento                | Hallazgo                                                                                         | Dimensión                 | Severidad      | Impacto en negocio                                                 | Esfuerzo           | Acción                                                           |
| ---- | ------------------------ | ------------------------------------------------------------------------------------------------ | ------------------------- | -------------- | ------------------------------------------------------------------ | ------------------ | ---------------------------------------------------------------- |
| B-01 | Contexto                 | El modelo de precio no está definido y no existe historia de pagos                               | Producto                  | **Bloqueante** | El marketplace no puede transaccionar                              | L                  | Decidir modelo y escribir la HU antes de HU-05                   |
| B-02 | Contexto / HU-01 / HU-02 | Sin base legal para tratar datos sensibles: no hay autorización, aviso de privacidad ni política | Cumplimiento              | **Bloqueante** | Cierre definitivo de la operación (Ley 1581 art. 23 lit. d)        | M                  | Capítulo de cumplimiento + pasos de consentimiento en los flujos |
| B-03 | Contexto                 | Sin determinar si la plataforma es intermediario o prestador de servicios de salud               | Cumplimiento              | **Bloqueante** | Operación sin habilitación (Res. 3100/2019 art. 4)                 | S (concepto legal) | Concepto jurídico previo a HU-05                                 |
| B-04 | HU-02                    | Se verifican credenciales sin verificar identidad                                                | Seguridad                 | **Bloqueante** | Suplantación de un profesional real en el domicilio de un paciente | M                  | Prueba de identidad con biometría y liveness                     |
| B-05 | HU-02 / HU-03            | No se consulta el ReTHUS, existiendo consulta pública y gratuita                                 | Seguridad / Escalabilidad | **Crítico**    | Aprobación de credenciales falsas y cuello de botella manual       | S                  | Integrar la consulta en el envío a revisión                      |
| B-06 | HU-02                    | Antecedentes disciplinarios opcionales                                                           | Seguridad                 | **Crítico**    | Precedente Care.com: tres acuerdos por el mismo problema           | S                  | Obligatorios vía ReTHUS + antecedentes judiciales                |
| B-07 | HU-03                    | SLA imposible: rompe a ~21 registros/día con reprocesos, ~7 con el revisor multitarea            | Escalabilidad             | **Crítico**    | Abandono de la oferta, MVP sin liquidez                            | M                  | Verificación automática + segundo revisor + métricas             |
| B-08 | HU-01                    | Contraseña: impone composición (prohibida por NIST) y omite longitud mínima                      | Seguridad                 | **Crítico**    | `a1!` es válida hoy                                                | XS                 | Reescribir RN-06                                                 |
| B-09 | HU-01                    | OTP sin TTL ni límite de intentos                                                                | Seguridad                 | **Crítico**    | Cuenta comprometible por fuerza bruta en ~17 min                   | S                  | TTL 10 min, 5 intentos, uso único, un solo código vigente        |
| B-10 | Contexto                 | Idempotencia declarada sin mecanismo                                                             | Arquitectura              | **Crítico**    | Asignaciones duplicadas; requisito no testeable                    | M                  | `Idempotency-Key` según borrador IETF                            |
| B-11 | HU-03                    | No existe revocación ni suspensión de un enfermero aprobado                                      | Producto / Seguridad      | **Crítico**    | Un sancionado sigue atendiendo indefinidamente                     | M                  | Estados `SUSPENDIDO` / `REVOCADO` + re-verificación              |
| B-12 | Contexto                 | PIN en claro y usado como prueba de presencia que no prueba presencia                            | Seguridad                 | **Crítico**    | Fraude de facturación por tiempo no prestado                       | M                  | Hash del PIN + verificación adicional de presencia               |
| B-13 | Contexto                 | Bloqueo de PIN tras 5 intentos sin ruta de desbloqueo                                            | Edge case                 | **Crítico**    | Incidente P1 con paciente esperando, garantizado                   | S                  | Desbloqueo con doble autorización                                |
| B-14 | Contexto                 | `Persona ↔ Rol` 1:1 y documento único                                                            | Arquitectura              | **Alto**       | Un enfermero no puede ser usuario; migración dolorosa después      | M                  | Relación N:M desde el día uno                                    |
| B-15 | Contexto                 | No existen cancelación ni no-show; `CANCELADO` es un estado huérfano                             | Producto                  | **Alto**       | Cada equipo inventará su propia regla                              | M                  | Escribir la historia de cancelación                              |
| B-16 | HU-01                    | No existe recuperación de contraseña en ninguna historia                                         | Producto                  | **Alto**       | Pérdida irreversible de cuentas                                    | S                  | Escribir la historia                                             |
| B-17 | Contexto                 | Sin rate limiting en ningún endpoint                                                             | Seguridad                 | **Alto**       | Abuso, coste de envíos, denegación de servicio                     | M                  | Política transversal por endpoint                                |
| B-18 | HU-04 / HU-02            | "Acompañamiento y transporte" y "Atención hospitalaria" sin requisitos acreditables              | Seguridad / Producto      | **Alto**       | Responsabilidad civil y clínica sin cobertura                      | M                  | Requisitos documentales y de competencia por tipo                |
| B-19 | HU-02                    | No se distingue auxiliar de profesional de enfermería                                            | Cumplimiento              | **Alto**       | Actos reservados ejecutados por quien no puede                     | S                  | Nivel de competencia en perfil y en tipo de servicio             |
| B-20 | Contexto                 | Configuración de ventanas sin versionar ni congelar                                              | Arquitectura              | **Alto**       | Exposición o retirada retroactiva de datos médicos en masa         | S                  | Snapshot de la configuración en el servicio                      |
| B-21 | Contexto                 | Sin patrón outbox para correos y notificaciones                                                  | Arquitectura              | **Alto**       | Pérdida de verificaciones y notificaciones críticas                | M                  | Outbox + reintentos + DLQ                                        |
| B-22 | HU-02                    | Carga de archivos sin límites, antivirus ni validación real de tipo                              | Seguridad                 | **Alto**       | Malware hacia el revisor, coste sin tope                           | S                  | Validación, límites y escaneo asíncrono                          |
| B-23 | HU-03                    | Acceso de lectura a datos sensibles sin auditar ni expirar                                       | Cumplimiento              | **Alto**       | Exfiltración indetectable de cédulas y tarjetas                    | S                  | URLs firmadas, marca de agua, registro de acceso                 |
| B-24 | Contexto                 | RN-17 bloquea toda la aplicación por calificación pendiente                                      | Producto / UX             | **Alto**       | Contrae la oferta y bloquea a un usuario en urgencia               | S                  | Bloqueo suave; duro solo sobre la acción específica              |
| B-25 | HU-01                    | Enumeración de usuarios que revela pertenencia a plataforma de salud                             | Seguridad                 | **Alto**       | Exposición indirecta de dato sensible                              | S                  | Respuestas genéricas + aviso por correo                          |
| B-26 | HU-02                    | Prohibición del radio de servicio impide cualquier matching geográfico                           | Producto / Arquitectura   | **Alto**       | Feed global, fan-out O(n×m), conversión en caída                   | M                  | Reabrir la decisión; normalizar la geografía                     |
| B-27 | HU-04                    | Contradicción entre título, flujo y alcance                                                      | Producto                  | **Medio**      | Factor 20 en la estimación                                         | XS                 | Resolver el alcance en una frase                                 |
| B-28 | HU-04                    | Falta código de negocio inmutable                                                                | Arquitectura              | **Medio**      | Bug clásico de semilla entre ambientes                             | XS                 | Añadir columna y unicidad global                                 |
| B-29 | HU-01                    | Datos médicos recolectados en el registro sin finalidad concreta                                 | Cumplimiento / UX         | **Medio**      | Incumple minimización y daña la conversión                         | S                  | Moverlos a la primera solicitud                                  |
| B-30 | Contexto                 | El modal de confirmación amplía la ventana de carrera                                            | UX / Arquitectura         | **Medio**      | Peor experiencia del lado de la oferta                             | S                  | Reserva temporal o aceptar-y-deshacer                            |
| B-31 | Todos                    | Cero requisitos no funcionales y cero volumetría                                                 | Producto                  | **Medio**      | "Escalar a millones" no tiene objetivo medible                     | S                  | Sección de NFR con cifras (§12)                                  |
| B-32 | HU-03                    | Sin MFA ni segregación de funciones para el superadministrador                                   | Seguridad                 | **Medio**      | Comprometer una cuenta compromete todo                             | S                  | MFA obligatorio + principio de cuatro ojos                       |
| B-33 | Contexto                 | Auditoría síncrona, mutable y sin particionar                                                    | Arquitectura              | **Medio**      | Tabla más caliente del sistema, latencia acoplada                  | M                  | Append-only, particionada, escritura asíncrona                   |
| B-34 | HU-02                    | Sin estado borrador en el registro                                                               | UX                        | **Medio**      | Abandono en onboarding documental                                  | S                  | Persistencia parcial e instrumentación del embudo                |
| B-35 | Contexto                 | Faltan estados `EXPIRADO`, `NO_SHOW`, `EN_DISPUTA`                                               | Producto                  | **Medio**      | Servicios zombis en el feed y reclamos fuera del sistema           | S                  | Ampliar la máquina de estados                                    |
| B-36 | Todos                    | Higiene documental: secciones duplicadas, numeración rota, IDs colisionantes                     | Documentación             | **Bajo**       | Ambigüedad evitable                                                | XS                 | Ver §2                                                           |

---

## 9. Historias bloqueantes que faltan por completo

| Historia propuesta                                         | Por qué bloquea el lanzamiento                                                                    |
| ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| **HU-P1 — Pagos, comisión y liquidación**                  | El sistema muestra precios que nadie define ni cobra; sin esto no hay negocio                     |
| **HU-P2 — Cancelación y no-show**                          | `CANCELADO` existe en el diagrama sin reglas; sin no-show, un servicio abandonado queda congelado |
| **HU-P3 — Recuperación de contraseña y cambio de correo**  | Se crean credenciales que no se pueden recuperar                                                  |
| **HU-P4 — Revocación y suspensión de enfermeros**          | Una aprobación hoy es permanente, incluso tras una sanción                                        |
| **HU-P5 — Consentimiento, términos y aviso de privacidad** | Es la base legal de todo el tratamiento; sin ella la operación no es lícita                       |
| **HU-P6 — Verificación de identidad**                      | Sin esto, la aprobación profesional no protege de la suplantación                                 |
| **HU-P7 — Soporte y desbloqueo operativo**                 | El bloqueo de PIN y el rechazo definitivo no tienen ruta de salida                                |
| **HU-P8 — Observabilidad, métricas y SLA operativo**       | Un SLA sin instrumentación es una frase                                                           |
| **HU-P9 — Retención, supresión y derechos del titular**    | Obligación legal directa, sin flujo asociado                                                      |
| **HU-P10 — Disputas y reclamos**                           | Hoy cualquier reclamo se resuelve fuera del sistema y sin trazabilidad                            |

---

## 10. Re-slicing recomendado

El problema estructural es que las cuatro historias son especificaciones funcionales largas presentadas como historias. Ninguna cabe en un sprint y ninguna es independiente.

### HU-01 → cinco historias

| Nueva  | Alcance                                                                                      |
| ------ | -------------------------------------------------------------------------------------------- |
| HU-01a | Crear cuenta con datos mínimos y política de contraseña conforme al estándar                 |
| HU-01b | Emitir y validar el código de verificación: TTL, uso único, límite de intentos y de reenvíos |
| HU-01c | Regla de autorización centralizada: correo no verificado bloquea la creación de solicitudes  |
| HU-01d | Recordatorio de verificación idempotente, con tope y purga                                   |
| HU-01e | Perfil médico opcional posterior a la verificación, con autorización expresa                 |

### HU-02 → cuatro historias

| Nueva  | Alcance                                                                          |
| ------ | -------------------------------------------------------------------------------- |
| HU-02a | Registro de datos de `Persona` con rol Enfermero, con borrador reanudable        |
| HU-02b | Carga de documentos con validación, límites y escaneo                            |
| HU-02c | Verificación automática contra ReTHUS y resolución de discrepancias              |
| HU-02d | Envío a revisión con validación de completitud y requisitos por tipo de servicio |

### HU-03 → cuatro historias

| Nueva  | Alcance                                                                       |
| ------ | ----------------------------------------------------------------------------- |
| HU-03a | Bandeja de revisión con cola, reclamo exclusivo y priorización                |
| HU-03b | Aprobar, rechazar y solicitar corrección con motivo tipificado y notificación |
| HU-03c | Acceso auditado a documentos con URLs firmadas y marca de agua                |
| HU-03d | Suspensión, revocación y re-verificación periódica                            |

### HU-04 → dos historias

| Nueva  | Alcance                                                                                                                                                                                  |
| ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| HU-04a | Catálogo sembrado por migración, con código inmutable, estado, requisitos documentales y nivel de competencia, servido desde caché con invalidación por evento                           |
| HU-04b | Módulo administrativo de gestión del catálogo, **solo si el negocio lo necesita antes del lanzamiento**. Probablemente no: cuatro filas que cambian una vez al año no justifican un CRUD |

Con este corte, cada elemento es pequeño, independiente, negociable, valioso, estimable y comprobable. Hoy ninguno de los cuatro documentos lo es.

---

## 11. Definition of Ready propuesta

Ninguna historia entra a sprint sin cumplir los 12 puntos.

- [ ] 1. La historia cabe en un sprint para un equipo, y el equipo lo ha confirmado con una estimación.
- [ ] 2. No contiene la expresión "pendiente de definición" ni equivalentes.
- [ ] 3. Todos los campos de datos tienen tipo, obligatoriedad, longitud y validación definidos.
- [ ] 4. Los criterios de aceptación están en formato Gherkin y son verificables por QA sin interpretación.
- [ ] 5. Los flujos de error tienen código de error, mensaje al usuario y comportamiento del sistema definidos.
- [ ] 6. Están definidos los límites: rate limits, tamaños, tiempos de expiración, número de reintentos.
- [ ] 7. Está identificada la base legal para cualquier dato personal tratado, y el consentimiento está en el flujo.
- [ ] 8. Los requisitos no funcionales aplicables están declarados (latencia objetivo, volumen esperado).
- [ ] 9. Los eventos de auditoría y las métricas de observabilidad están enumerados.
- [ ] 10. Las dependencias con otras historias están explícitas y resueltas o planificadas.
- [ ] 11. Los diseños o wireframes de las pantallas afectadas existen.
- [ ] 12. Hay un responsable de producto que puede resolver dudas durante el sprint.

---

## 12. Requisitos no funcionales propuestos (hoy inexistentes)

Sin volumetría objetivo, la pregunta "¿escala a millones?" no tiene respuesta verificable. Propuesta de punto de partida, a validar con negocio:

| Categoría                     | Requisito propuesto                                                                                                                                                                                                                             |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Volumetría objetivo año 1** | 50.000 usuarios registrados, 2.000 enfermeros aprobados, 500 solicitudes/día, pico de 10× en franjas de mañana                                                                                                                                  |
| **Latencia**                  | p95 < 300 ms y p99 < 800 ms en lecturas; p99 < 500 ms en aceptación de servicio                                                                                                                                                                 |
| **Disponibilidad**            | 99,5 % mensual para el MVP, 99,9 % tras el primer semestre                                                                                                                                                                                      |
| **RPO / RTO**                 | RPO 5 min, RTO 1 h                                                                                                                                                                                                                              |
| **Concurrencia**              | 500 intentos de aceptación simultáneos sobre un mismo servicio sin asignación duplicada                                                                                                                                                         |
| **Rate limiting**             | Registro: 5/hora por IP. Reenvío OTP: 3/hora, 10/día por cuenta. Validación OTP: 5 por código. Login: 10/hora por cuenta con backoff. Aceptación: 30/min por enfermero                                                                          |
| **Retención**                 | Auditoría 5 años; documentos de enfermeros mientras el perfil esté activo más 2 años; cuentas no verificadas purgadas a los 30 días                                                                                                             |
| **Cifrado**                   | TLS 1.2+ en tránsito; cifrado en reposo en base y almacén de objetos; cifrado a nivel de campo para datos médicos y PIN hasheado                                                                                                                |
| **Observabilidad**            | Métricas obligatorias: tamaño y edad de la cola de revisión, p95 de tiempo de revisión, tasa de aceptación por servicio publicado, tasa de colisión en aceptación, tasa de entrega de correos, intentos fallidos de PIN, incumplimientos de SLA |
| **Accesibilidad**             | WCAG 2.1 nivel AA en los flujos de registro, verificación y PIN                                                                                                                                                                                 |

---

## Anexo A — Cálculos

### A.1 Fuerza bruta sobre un código de 6 dígitos

Espacio de búsqueda: 10⁶ = 1.000.000 de combinaciones.

| Configuración                                                             | Cálculo                 | Resultado                                                                            |
| ------------------------------------------------------------------------- | ----------------------- | ------------------------------------------------------------------------------------ |
| Sin límite ni expiración, 50 req/s, un origen                             | 10⁶ / 50 = 20.000 s     | ~5,5 h para agotar; esperanza ~2,8 h                                                 |
| Sin límite ni expiración, 20 hilos paralelos                              | 20.000 s / 20 = 1.000 s | **~17 min por cuenta**                                                               |
| TTL 10 min + 5 intentos                                                   | 5 / 10⁶                 | 0,0005 %                                                                             |
| TTL 10 min + 5 intentos, N códigos vigentes                               | 5N / 10⁶                | Riesgo ×N                                                                            |
| PIN de servicio, 5 intentos                                               | 5 / 10⁶                 | 0,0005 % (entropía correcta; el fallo está en el modelo de amenaza, no en el número) |
| Ataque distribuido, 100 códigos probados sobre 100.000 cuentas pendientes | 100.000 × 100 / 10⁶     | **~10 cuentas comprometidas** → el límite debe ser por cuenta **y** por origen       |

### A.2 Capacidad de revisión manual (HU-03)

Supuestos: 12 min por perfil, 6 h netas de revisión al día, un revisor, modelo M/M/1.

```
μ = 360 / 12 = 30 perfiles/día
ρ = λ / μ
W = 1 / (μ − λ)        (tiempo total en el sistema)
```

| λ   | ρ    | W     | SLA 24 h  |
| --- | ---- | ----- | --------- |
| 10  | 0,33 | 1,2 h | Sí        |
| 15  | 0,50 | 1,9 h | Sí        |
| 20  | 0,67 | 2,4 h | Sí        |
| 25  | 0,83 | 4,8 h | Sí        |
| 28  | 0,93 | 12 h  | Al límite |
| 29  | 0,97 | 24 h  | Rompe     |
| 30  | 1,00 | ∞     | Colapsa   |

**Ajuste por reprocesos** (35 % de perfiles con al menos una corrección): λ_eff = 1,35 λ → ruptura del SLA en **λ ≈ 21,5 registros nuevos/día**.

**Ajuste por multitarea del superadministrador** (2 h/día efectivas en lugar de 6): μ = 10/día → ruptura en **λ ≈ 7 registros nuevos/día**.

### A.3 Contención en la aceptación

| Patrón                                  | 500 intentos concurrentes sobre la misma fila                                     | Veredicto             |
| --------------------------------------- | --------------------------------------------------------------------------------- | --------------------- |
| `SELECT` y luego `UPDATE`               | Múltiples ganadores posibles bajo READ COMMITTED                                  | **Incorrecto**        |
| `SELECT ... FOR UPDATE`                 | Serialización; con 8 ms por transacción, hasta ~4 s de cola y saturación del pool | Correcto pero costoso |
| `UPDATE ... WHERE estado = 'PUBLICADO'` | Un ganador, 499 fallos inmediatos, un solo viaje a la base                        | **Recomendado**       |

---

## Anexo B — Fuentes

| Tema                                                  | Fuente                                                                                                                                                                                                                                                                                                                                                                                                            |
| ----------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Contraseñas y OTP                                     | NIST SP 800-63B (Digital Identity Guidelines), revisión 4 — https://pages.nist.gov/800-63-4/sp800-63b.html y https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-63B-4.pdf                                                                                                                                                                                                                          |
| Protección de datos                                   | Ley 1581 de 2012, arts. 5, 23, 25 — https://www.funcionpublica.gov.co/eva/gestornormativo/norma.php?i=49981 ; Decreto 1377 de 2013                                                                                                                                                                                                                                                                                |
| Actividad sancionatoria de la SIC                     | https://xmslatam.com/co/ley-1581-de-2012-proteccion-datos-personales/                                                                                                                                                                                                                                                                                                                                             |
| ReTHUS                                                | Ministerio de Salud — https://www.minsalud.gov.co/salud/PO/Paginas/registro-unico-nacional-del-talento-humano-en-salud-rethus.aspx ; consulta pública SISPRO — https://www.sispro.gov.co/central-prestadores-de-servicios/Pages/ReTHUS-Registro-de-Talento-Humano-en-Salud.aspx ; Ley 1164 de 2007                                                                                                                |
| Habilitación de prestadores                           | Resolución 3100 de 2019, art. 4 — https://www.suin-juriscol.gov.co/viewDocument.asp?ruta=Resolucion/30039964                                                                                                                                                                                                                                                                                                      |
| Precedentes de verificación en plataformas de cuidado | Acuerdo con fiscales de San Francisco y Marin, 2020 — https://www.cbsnews.com/sanfrancisco/news/care-com-settles-lawsuit-claiming-it-misrepresented-background-checks-auto-renewed-subscriptions/ ; acuerdo con la FTC, 2024 — https://www.latintimes.com/care-settlement-caregivers-millions-558457 ; acuerdo en Massachusetts, 2018 — https://www.wbur.org/news/2018/02/27/care-com-background-check-settlement |
| Idempotencia                                          | `draft-ietf-httpapi-idempotency-key-header-07` (IETF, oct. 2025) — https://www.ietf.org/archive/id/draft-ietf-httpapi-idempotency-key-header-07.html ; práctica de Stripe (purga de claves a 24 h) — https://http.dev/idempotency-key                                                                                                                                                                             |
| Seguridad de API                                      | OWASP Top 10 (2021); OWASP API Security Top 10 (2023); OWASP ASVS; Cheat Sheets de Authentication, Forgot Password, User Enumeration y File Upload                                                                                                                                                                                                                                                                |
| Ejercicio de la enfermería                            | Ley 266 de 1996; Ley 911 de 2004                                                                                                                                                                                                                                                                                                                                                                                  |

### Hipótesis pendientes de validación

1. **Naturaleza jurídica de la plataforma** (intermediario vs. prestador de servicios de salud). Requiere concepto del Ministerio de Salud o de la Supersalud.
2. **Clasificación laboral de los enfermeros** en el marco de la discusión colombiana sobre trabajo en plataformas digitales. Requiere asesoría laboral.
3. **Tasa de abandono en el onboarding documental** de HU-02. No dispongo de una cifra verificada para este segmento; debe medirse con instrumentación del embudo.
4. **Si la información médica capturada constituye historia clínica** bajo la Resolución 1995 de 1999 y la Ley 2015 de 2020. Determina obligaciones de reserva, conservación y custodia.

# Sugerencias del auditor — Plataforma de Servicios de Enfermería (AppEnfermeria)

**Fecha:** 2026-09-29
**Destinatario:** agente que procesará las sugerencias.
**Naturaleza:** propuestas del auditor. No forman parte de los hallazgos ni son requisitos: cada una requiere decisión del responsable de producto antes de aplicarse. Los hallazgos están en `Hallazgos-Auditoria-AppEnfermeria.md`; los identificadores (INC, GAP, SUP, PM, COND) son los mismos.

## 1. Acciones prioritarias

1. **Decidir el alcance real del MVP y fijar un documento normativo único.** Resolver, punto por punto, qué de vision.md entra (pagos, negociación, directorio, biometría, geolocalización, tiempo real, atención hospitalaria) y propagarlo a Requirements-Context y a las HU. _Relacionado con: INC-01, INC-02, INC-03, INC-04, INC-07, INC-08._
2. **Obtener concepto jurídico y base legal antes de escribir HU-05.** Definir si la plataforma es intermediario o prestador, y qué autorizaciones y consentimientos exige el tratamiento de salud, biometría y ubicación. _Relacionado con: INC-05, SUP-01, GAP-07, GAP-08._
3. **Definir presupuesto, equipo y cronograma, y dimensionar la revisión de enfermeros.** Hoy no existen. Sin ellos, cuatro dimensiones no son evaluables y el SLA de aprobación no es defendible. _Relacionado con: GAP-01, GAP-03, GAP-04, INC-13._

## 2. Sugerencia por incongruencia

| ID | Severidad | Sugerencia |
| --- | --- | --- |
| INC-01 | Crítica | Declarar un documento normativo único: aprobar o descartar cada ampliación de vision.md, propagar lo aprobado a Requirements-Context y a las HU, y marcar el estado real de cada documento. |
| INC-02 | Crítica | Escribir las historias de precio/negociación y de pagos (proveedor, custodia, comisión, reembolso) antes de HU-05 y añadirlas a la lista de §29. |
| INC-03 | Crítica | Decidir el modelo de asignación del MVP (feed atómico, negociación o directorio) y rediseñar la tabla de transiciones con estados y guardas. |
| INC-04 | Crítica | Incorporar a HU-02/03 el contraste con ReTHUS y la verificación facial (intentos, fallback, proveedor), hacer obligatorios los antecedentes y unificar el número de intentos. |
| INC-05 | Crítica | Añadir a HU-01/HU-02 pasos y criterios de autorización expresa (versión del aviso, fecha, canal), consentimiento separado para biometría y ubicación, y un mecanismo para el tercero titular; reescribir §21 como capítulo de cumplimiento. |
| INC-06 | Alta | Extender política de credenciales y verificación de correo a todos los roles y añadir una HU de alta y protección de cuentas administrativas. |
| INC-07 | Alta | Decidir si el tipo entra o no; si no, sembrarlo inactivo y ajustar RN-01 y los criterios de HU-04. |
| INC-08 | Alta | Reemplazar formalmente la restricción, definir el tipo de dato y la precisión, y actualizar Req §5.1, HU-02 y glosario. |
| INC-09 | Alta | Fijar la precisión máxima del origen hasta la asignación y estructurar o filtrar el texto libre para impedir datos de salud. |
| INC-10 | Alta | Convertir el bloqueo en restricción específica (p. ej. no crear ni aceptar servicios nuevos) y excluir PIN, extensiones, cancelación y soporte. |
| INC-11 | Alta | Escribir la HU de desbloqueo (soporte con doble autorización o regeneración de PIN notificada a ambas partes). |
| INC-12 | Alta | Escribir HU de cancelación, no-show y expiración, con tabla explícita de transiciones y guardas. |
| INC-13 | Alta | Definir revisores, reloj y métricas del SLA; incorporar verificación asistida para que el humano revise solo excepciones. |
| INC-14 | Alta | Modelar las entidades, definir obligatoriedad y excepciones, y escribir la HU correspondiente con su consentimiento. |
| INC-15 | Media | Definir el canal mínimo de coordinación y el comportamiento para servicios con menos margen que la ventana. |
| INC-16 | Media | Definir qué diferencia el rechazo (p. ej. terminal o con límite de ciclos) de la corrección, o fusionar los estados. |
| INC-17 | Media | Añadir el estado de expiración o redefinir la métrica; incluir al menos una métrica de seguridad/confianza y fijar umbrales. |
| INC-18 | Media | Definir quién recibe el PIN en servicios para terceros y qué evidencia adicional de presencia se exige. |
| INC-19 | Media | Corregir el diagrama o añadir una restricción de exclusividad entre los perfiles. |
| INC-20 | Media | Retirar la afirmación o convertirla en regla explícita, y definir el tratamiento de solapamientos. |
| INC-21 | Media | Indicar qué campos de experiencia son obligatorios y añadir criterios de aceptación. |
| INC-22 | Media | Marcar la auditoría como histórica, corregir sus cifras y registrar por qué se descartaron recomendaciones que se rechazaron. |
| INC-23 | Media | Fijar TTL, intentos máximos, uso único y límite de reenvíos. |
| INC-24 | Baja | Unificar la ubicación de ambos campos. |
| INC-25 | Baja | Fijar un valor por defecto para la ventana del PIN. |
| INC-26 | Baja | Sustituir la lista de §12.1 por una referencia a §9. |

## 3. Cómo validar los supuestos críticos

| ID | Supuesto | Estado | Cómo validarlo | Evidencia que lo confirmaría | Evidencia que lo refutaría |
| --- | --- | --- | --- | --- | --- |
| SUP-01 | La plataforma es un intermediario tecnológico y no un prestador de servicios de salud. | SUPUESTO | Concepto escrito de un abogado de salud y consulta a la autoridad sanitaria antes de HU-05. | Concepto que califique el modelo como intermediación. | Concepto que exija habilitación o vea rasgos de prestador. |
| SUP-02 | Hay demanda suficiente de personas dispuestas a contratar enfermería por plataforma en lugar de canales informales. | SUPUESTO | Entrevistas a usuarios y una página de preinscripción con disposición a pagar, antes de construir pagos. | Preinscripciones y disposición de pago a un precio concreto. | Preferencia por el canal actual o rechazo del precio. |
| SUP-03 | Habrá suficientes enfermeros dispuestos a operar por la plataforma con la comisión que se defina. | SIN SUSTENTO | Entrevistar a enfermeros sobre tarifa mínima, comisión tolerable y disponibilidad real. | Compromisos de registro y tarifas compatibles con lo que paga el usuario. | Tarifas mínimas por encima de la disposición de pago. |
| SUP-04 | La aprobación manual por un solo rol puede mantener un SLA de 24–36 h. | SIN SUSTENTO | Cronometrar la revisión de 20 perfiles reales y definir número de revisores y horas efectivas. | Minutos por perfil medidos y capacidad mayor a la demanda proyectada con margen. | Capacidad menor a la llegada esperada o más de un ciclo de corrección por perfil. |
| SUP-05 | Cuatro imágenes autoportadas revisadas por una persona bastan para acreditar identidad, vigencia y ausencia de sanciones. | SIN SUSTENTO | Prueba con 10 perfiles verdaderos y 10 simulados con datos de terceros para medir cuántos pasarían la revisión. | Los perfiles simulados son detectados de forma consistente. | Al menos un perfil simulado es aprobado. |
| SUP-06 | El PIN de 6 dígitos prueba que el enfermero y el paciente están juntos. | SIN SUSTENTO | Pruebas de campo con inicio remoto simulado y comparación con la ubicación del enfermero. | El inicio remoto es rechazado por una segunda señal (ubicación o rostro). | El servicio inicia con el PIN dictado por teléfono. |
| SUP-07 | Existe un proveedor de verificación facial con costo asumible y cumplimiento normativo. | SUPUESTO | Solicitar cotizaciones a al menos 3 proveedores con costo por verificación y ubicación de los datos. | Cotización dentro del presupuesto y contrato de tratamiento de datos. | Costos prohibitivos o datos fuera de lo permitido. |
| SUP-08 | Exigir internet sin modo offline es aceptable para la operación. | SUPUESTO | Mapear cobertura en las zonas de lanzamiento y definir tolerancia a cortes. | Cobertura adecuada en las zonas piloto. | Cortes frecuentes en zonas objetivo. |
| SUP-09 | Una persona solo necesita un rol (usuario, enfermero o superadministrador). | SUPUESTO | Preguntar a 10 enfermeros si contratarían servicios para familiares. | Ningún caso de doble rol relevante. | Varios casos de doble rol. |
| SUP-10 | Con 5 intentos sobre 10^6 combinaciones, adivinar el PIN tiene probabilidad de 0,0005 %. | VERIFICADA | Cálculo: 5 / 1.000.000 = 0,0005 %. Se mantiene si el contador es por servicio y no se reinicia. | El cálculo. | Contador por sesión o reinicio del contador sin control. |
| SUP-11 | Las decisiones CN-02 a CN-09 se aplicaron a los documentos fuente. | VERIFICADA | Comprobado en los archivos: 4 tipos de enfermero en §5.2, lista de datos sensibles en §12, hash del PIN en §14, contraseña 12–64 en HU-01, CRUD en HU-04. Excepción: CN-01 (ver INC-26). | Lectura directa de los archivos. | — |
| SUP-12 | Una ventana de 180 minutos antes del inicio es suficiente para coordinar y a la vez proteger datos sensibles. | SUPUESTO | Simular 10 casos reales de servicios programados e inmediatos con enfermeros y familias. | Coordinación posible en menos de 3 h en la mayoría de casos. | Necesidad recurrente de contacto antes. |
| SUP-13 | El correo electrónico es un canal fiable para códigos y notificaciones críticas. | SUPUESTO | Medir entregabilidad con un proveedor real en un piloto y definir canal alterno. | Tasa de entrega alta y sin rebotes relevantes. | Rebotes o correos en spam frecuentes. |

## 4. Condiciones mínimas sugeridas para aprobar

- [ ] **COND-01.** Existe un único documento normativo: vision.md aprobado o recortado y propagado a Requirements-Context y a HU-01…04; 0 incongruencias críticas abiertas. _(INC-01, GAP-20)_
- [ ] **COND-02.** Modelo de asignación decidido (feed atómico, negociación o directorio) y tabla de transiciones de estados con guardas, incluyendo cancelación, no-show y expiración. _(INC-03, INC-12)_
- [ ] **COND-03.** Historias de precio/negociación y de pagos escritas, con proveedor elegido, porcentaje de comisión y regla de custodia. _(INC-02, GAP-02, GAP-12)_
- [ ] **COND-04.** Concepto jurídico escrito, firmado por un abogado de salud, sobre intermediario vs. prestador de salud. _(SUP-01, GAP-07)_
- [ ] **COND-05.** Autorización de tratamiento, aviso de privacidad y consentimientos separados (salud, biometría, ubicación, tercero) integrados en las HU con criterios de aceptación. _(INC-05, GAP-08)_
- [ ] **COND-06.** Verificación de identidad y de credenciales contra fuente oficial incorporadas a HU-02/03: ningún perfil pasa a «Aprobado» sin resultado. _(INC-04, SUP-05)_
- [ ] **COND-07.** Presupuesto aprobado con una línea por proveedor y por rubro operativo. _(GAP-01, GAP-11)_
- [ ] **COND-08.** Cronograma con hitos, responsables y equipo dimensionado (desarrollo, revisión, soporte). _(GAP-03, GAP-04)_
- [ ] **COND-09.** Requisitos no funcionales documentados: volumetría año 1, latencia p95/p99, disponibilidad, RPO/RTO, retención y reloj del sistema. _(GAP-05, GAP-18)_
- [ ] **COND-10.** Al menos 2 revisores asignados, reloj del SLA definido y métrica de edad de la cola; capacidad medida con 20 perfiles reales. _(INC-13, SUP-04)_
- [ ] **COND-11.** Escritas las HU de cancelación, recuperación de contraseña, desbloqueo de PIN y revocación/suspensión de enfermeros; HU-05…HU-17 con estado «Definida». _(INC-11, INC-12, GAP-10)_
- [ ] **COND-12.** Credenciales de enfermeros y superadministrador definidas, con verificación de correo y segundo factor para el rol administrativo. _(INC-06)_
- [ ] **COND-13.** Idempotencia y código de correo especificados (clave, retención, TTL, intentos, reenvíos) con criterios de aceptación comprobables. _(INC-23, GAP-14, GAP-15)_
- [ ] **COND-14.** Evidencia de demanda y oferta: propuesta del auditor de al menos 15 entrevistas a usuarios y 15 a enfermeros, con disposición de pago y tarifa mínima (umbral ajustable por el equipo). _(SUP-02, SUP-03, GAP-06)_
- [ ] **COND-15.** Métricas de éxito con umbral numérico, incluida al menos una métrica de seguridad/confianza y sin depender de estados inexistentes. _(INC-17, GAP-09)_
- [ ] **COND-16.** Bloqueo por calificación pendiente redefinido para no impedir PIN, extensiones, cancelación ni soporte. _(INC-10)_
- [ ] **COND-17.** Precisión del origen previa a la aceptación fijada y texto libre estructurado o filtrado para impedir datos de salud. _(INC-09, GAP-13)_
- [ ] **COND-18.** Documentos derivados (modelo de dominio, glosario, índice de reglas, auditoría previa) reconciliados con la fuente normativa. _(INC-19, INC-20, INC-22, INC-26)_

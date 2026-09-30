# Sugerencias del auditor — Plataforma de Servicios de Enfermería (AppEnfermeria)

**Fecha del informe:** 2026-09-30
**Destinatario:** agente que procesará las sugerencias.
**Naturaleza del documento:** propuestas del auditor. No forman parte de los hallazgos ni son requisitos: cada una requiere decisión del responsable de producto antes de aplicarse. Los hallazgos están en `Hallazgos-Auditoria-AppEnfermeria.md`; los identificadores (INC, GAP, SUP, PM, COND) son los mismos.
**Fuente de los datos:** `Diagnostico-Viabilidad-AppEnfermeria.html` (objeto `DIAGNOSTICO`).

## Decisión de negocio que enmarca esta auditoría

> Decisión de negocio (2026-09-30): la aplicación se realizará como intermediario tecnológico (marketplace) que conecta usuarios con enfermeros independientes; no como prestador de servicios de salud.

Consecuencias para la auditoría:

- La clasificación regulatoria deja de ser una pregunta abierta de producto y pasa a ser una **decisión declarada** (SUP-01). El concepto jurídico sigue siendo necesario, pero para confirmar que el **diseño** encaja con el modelo elegido, no para decidir el modelo.
- La intención de operar como intermediario no fija por sí sola la calificación jurídica: la determina el diseño real. `vision.md` §7 todavía la registra como «restricción activa, no resuelta» (INC-34).
- El modelo exige un marco contractual propio (términos y condiciones, enfermeros como independientes, límites de responsabilidad, seguros): GAP-25 y COND-21.

## Alcance de esta auditoría

Auditoría acotada a lo que existe: contexto (Requirements-Context, vision, modelo de dominio, reglas, glosario, personas), HU-01…HU-04 y coherencia de la definición del MVP. Las HU aún no escritas no se cuentan como incongruencias: figuran como brechas de información.

## 1. Acciones prioritarias

1. **Reconciliar la definición del MVP con Requirements-Context y las HU existentes.** Decidir el modelo de asignación y su máquina de estados (INC-03), dónde vive la tarifa (INC-02) y actualizar HU-02/03 con la verificación facial que la visión ya define (INC-04); si algo no cabe, sacarlo del MVP. _Relacionado con: INC-03, INC-02, INC-04, INC-27._
2. **Confirmar jurídicamente el modelo de intermediario y completar los consentimientos.** Se decidió operar como intermediario, no como prestador de salud: falta el concepto jurídico sobre el diseño y el marco contractual. HU-01 ya exige consentimiento para datos médicos propios; faltan tercero titular, biometría y datos del enfermero. _Relacionado con: SUP-01, GAP-07, GAP-25, INC-05._
3. **Definir presupuesto, equipo y cronograma, y validar demanda y oferta.** Sin ellos cuatro dimensiones siguen sin poder evaluarse, y el SLA de aprobación y la liquidez del marketplace no son defendibles. _Relacionado con: GAP-01, GAP-03, GAP-04, GAP-06, INC-13._

## 2. Sugerencia por incongruencia

| ID | Severidad | Sugerencia |
| --- | --- | --- |
| INC-03 | Crítica | Decidir el modelo de asignación del MVP y rediseñar la tabla de transiciones con estados y guardas (incluyendo expiración y cancelación). |
| INC-02 | Alta | Decidir dónde vive la tarifa sugerida (TipoServicio o Configuracion), retirar la exclusión de HU-04 o documentar la excepción, y alinear Req §17.1 con el modelo de precio. |
| INC-04 | Alta | Actualizar HU-02/03 para reflejar la verificación facial y el contraste con fuente oficial que define la visión, o retirar esa mitigación del MVP. |
| INC-05 | Alta | Añadir a HU-01/HU-02 criterios de autorización expresa (versión del aviso, fecha, revocación), consentimiento separado para biometría, mecanismo para el tercero titular y reescribir §21 como capítulo de cumplimiento. |
| INC-06 | Alta | Extender política de credenciales y verificación de correo a todos los roles y añadir una HU de alta y protección de cuentas administrativas. |
| INC-09 | Alta | Fijar la precisión máxima del origen hasta la asignación y estructurar o filtrar el texto libre para impedir datos de salud. |
| INC-28 | Alta | Especificar el mecanismo (PIN derivado con HMAC y secreto fuera de la base, o cifrado reversible con clave gestionada) y reescribir §14.1 y RN-18 sin presentar el hash como control suficiente. |
| INC-29 | Alta | Definir un estado terminal (rechazo definitivo o bloqueo) y de suspensión/revocación, con causal, límite de ciclos de corrección, lista de bloqueo del documento y auditoría. |
| INC-10 | Media | Alinear las cuatro copias con §19.1, o sustituirlas por una referencia a §19.1 como fuente única (como se hizo con §9). |
| INC-11 | Media | Definir si «soporte» es una función del superadministrador o un rol nuevo, y reflejarlo en §2 y §22. |
| INC-12 | Media | Precisar en §26.1 qué transiciones a «Cancelado» son válidas desde «En curso» y cómo interactúan con §16 y §19. |
| INC-13 | Media | Definir revisores, reloj y métricas del SLA; incorporar verificación asistida para que el humano revise solo excepciones. |
| INC-14 | Media | Extender domain-model con las entidades y atributos del MVP aprobado, o marcar los componentes sin modelo como condicionados. |
| INC-15 | Media | Definir el canal mínimo de coordinación y el comportamiento para servicios con menos margen que la ventana. |
| INC-17 | Media | Añadir el estado de expiración o redefinir la métrica; incluir al menos una métrica de seguridad/confianza y fijar umbrales. |
| INC-27 | Media | Registrar en el propio documento qué componentes del MVP quedan condicionados a cada pregunta abierta y qué pasa con ellos si la respuesta es negativa. |
| INC-30 | Media | Definir contenido y canal del recordatorio, si genera código nuevo y cómo se sale del agotamiento de reenvíos. |
| INC-31 | Media | Aclarar que basta con estar asociado al servicio (o limitar la unicidad a servicios activos) y eliminar la exigencia global. |
| INC-33 | Media | Definir el fallback para pacientes que no pueden confirmar (p. ej. contacto de emergencia o soporte) y cuándo se bloquea el inicio. |
| INC-32 | Baja | Reconciliar los derivados con la fuente normativa y decidir un único lugar para la foto. |
| INC-34 | Baja | Registrar la decisión en vision.md (§5 y §7) y redactar el catálogo y las funciones de control de modo que describan a la plataforma como intermediaria (el enfermero presta, la plataforma conecta y verifica), sujeto al concepto jurídico. |

## 3. Cómo validar los supuestos críticos

| ID | Supuesto | Estado | Cómo validarlo | Evidencia que lo confirmaría | Evidencia que lo refutaría |
| --- | --- | --- | --- | --- | --- |
| SUP-01 | La operación como intermediario tecnológico (decisión de negocio) será reconocida como tal y no como prestación de servicios de salud. | SUPUESTO | Concepto escrito de un abogado de salud sobre el diseño concreto (no solo la intención), y consulta a la autoridad sanitaria antes de HU-05. | Concepto que califique el diseño actual como intermediación. | Concepto que exija habilitación o que vea rasgos de prestador en funciones específicas. |
| SUP-02 | Hay demanda suficiente de personas dispuestas a contratar enfermería por plataforma en lugar de canales informales. | SUPUESTO | Entrevistas a usuarios y una página de preinscripción con disposición a pagar, antes de construir pagos. | Preinscripciones y disposición de pago a un precio concreto. | Preferencia por el canal actual o rechazo del precio. |
| SUP-03 | Habrá suficientes enfermeros dispuestos a operar por la plataforma con la comisión que se defina. | SIN SUSTENTO | Entrevistar a enfermeros sobre tarifa mínima, comisión tolerable y disponibilidad real. | Compromisos de registro y tarifas compatibles con lo que paga el usuario. | Tarifas mínimas por encima de la disposición de pago. |
| SUP-04 | La aprobación manual por un solo rol puede mantener un SLA de 24–36 h. | SIN SUSTENTO | Cronometrar la revisión de 20 perfiles reales y definir número de revisores y horas efectivas. | Minutos por perfil medidos y capacidad mayor a la demanda proyectada con margen. | Capacidad menor a la llegada esperada o más de un ciclo de corrección por perfil. |
| SUP-05 | Cuatro imágenes autoportadas revisadas por una persona bastan para acreditar identidad, vigencia y ausencia de sanciones. | SIN SUSTENTO | Prueba con 10 perfiles verdaderos y 10 simulados con datos de terceros para medir cuántos pasarían la revisión. | Los perfiles simulados son detectados de forma consistente. | Al menos un perfil simulado es aprobado. |
| SUP-06 | El PIN de 6 dígitos prueba que el enfermero y el paciente están juntos. | SIN SUSTENTO | Pruebas de campo con inicio remoto simulado y comparación con la ubicación del enfermero. | El inicio remoto es rechazado por una segunda señal (ubicación o rostro). | El servicio inicia con el PIN dictado por teléfono. |
| SUP-07 | Existe un proveedor de verificación facial con costo asumible y cumplimiento normativo. | SUPUESTO | Solicitar cotizaciones a al menos 3 proveedores con costo por verificación y ubicación de los datos. | Cotización dentro del presupuesto y contrato de tratamiento de datos. | Costos prohibitivos o datos fuera de lo permitido. |
| SUP-08 | Exigir internet sin modo offline es aceptable para la operación. | SUPUESTO | Mapear cobertura en las zonas de lanzamiento y definir tolerancia a cortes. | Cobertura adecuada en las zonas piloto. | Cortes frecuentes en zonas objetivo. |
| SUP-09 | Una persona solo necesita un rol (usuario, enfermero o superadministrador). | SUPUESTO | Preguntar a 10 enfermeros si contratarían servicios para familiares. | Ningún caso de doble rol relevante. | Varios casos de doble rol. |
| SUP-10 | Con 5 intentos sobre 10^6 combinaciones, adivinar el PIN tiene probabilidad de 0,0005 %. | VERIFICADA | Cálculo: 5 / 1.000.000 = 0,0005 %. Se mantiene si el contador es por servicio y no se reinicia; el desbloqueo por soporte (INC-11) podría reiniciarlo. | El cálculo. | Contador por sesión o reinicio del contador sin control. |
| SUP-11 | Las resoluciones CN-10 a CN-21 quedaron reflejadas en todos los documentos fuente y derivados. | SIN SUSTENTO | Revisión cruzada de cada CN contra los documentos que afecta. Contradicha en: Req §27 regla 16 y RN-17, CTX-RN-17, glosario (INC-10), HU-03 RN-08, personas.md y glosario (INC-32) y recordatorio de 24 h (INC-30). | Búsqueda sin coincidencias residuales de las reglas reemplazadas. | Coincidencias residuales (hoy hay al menos 7) |
| SUP-12 | Una ventana de 180 minutos antes del inicio es suficiente para coordinar y a la vez proteger datos sensibles. | SUPUESTO | Simular 10 casos reales de servicios programados e inmediatos con enfermeros y familias. | Coordinación posible en menos de 3 h en la mayoría de casos. | Necesidad recurrente de contacto antes. |
| SUP-13 | El correo electrónico es un canal fiable para códigos y notificaciones críticas. | SUPUESTO | Medir entregabilidad y latencia con un proveedor real en un piloto y definir canal alterno. | Tasa de entrega alta y latencia muy inferior a 15 min. | Rebotes, spam o retrasos frecuentes. |
| SUP-14 | Un PIN de 6 dígitos guardado como hash sigue protegido si se filtra la base de datos. | SIN SUSTENTO | Reproducido en esta auditoría: con SHA-256 sin sal, la tabla de los 1.000.000 de PIN posibles se generó en 2,87 s y se invirtió un hash de prueba al instante. Con hash lento o HMAC con secreto externo el costo sube; el texto no lo exige. | Diseño con HMAC y secreto fuera de la base, o hash lento, más límite de intentos. | Cualquier hash rápido sin secreto (resultado ya medido). |
| SUP-15 | Un enfermero puede prestar «Acompañamiento y transporte» y «Atención hospitalaria» con los mismos requisitos que un servicio domiciliario. | SUPUESTO | Consultar requisitos legales y de seguro por tipo, y las políticas de acceso de 2 o 3 centros hospitalarios de la zona piloto. | Concepto que no exija requisitos adicionales. | Requisitos adicionales por tipo (licencia, seguro, autorización). |

## 4. Condiciones mínimas sugeridas para aprobar

- [ ] **COND-01.** El modelo de asignación, la tarifa y la verificación de identidad que define vision.md quedan reflejados en Requirements-Context y en HU-02/03/04 (o se retiran del MVP); 0 incongruencias críticas abiertas. _(INC-03, INC-02, INC-04, INC-27)_
- [ ] **COND-02.** Modelo de asignación decidido (feed atómico, negociación o directorio) y tabla de transiciones de estados con guardas, incluyendo cancelación, no-show y expiración. _(INC-03, INC-12)_
- [ ] **COND-03.** Historias de precio/negociación y de pagos escritas, con proveedor elegido, porcentaje de comisión y regla de custodia. _(INC-02, GAP-02, GAP-12)_
- [ ] **COND-04.** Concepto jurídico escrito, firmado por un abogado de salud, que confirme que el diseño es compatible con operar como intermediario tecnológico y no como prestador de salud; decisión registrada en vision.md. _(SUP-01, GAP-07, INC-34)_
- [ ] **COND-05.** Consentimientos separados y registrados (versión del aviso, fecha, revocación) para biometría, ubicación, tercero titular y datos del enfermero, integrados en las HU con criterios de aceptación; política de retención y supresión. _(INC-05, GAP-08)_
- [ ] **COND-06.** Verificación facial y contraste con fuente oficial incorporados a HU-02/03: ningún perfil pasa a «Aprobado» sin resultado. _(INC-04, INC-33, SUP-05)_
- [ ] **COND-07.** Presupuesto aprobado con una línea por proveedor y por rubro operativo. _(GAP-01, GAP-11)_
- [ ] **COND-08.** Cronograma con hitos, responsables y equipo dimensionado (desarrollo, revisión, soporte). _(GAP-03, GAP-04)_
- [ ] **COND-09.** Requisitos no funcionales documentados: volumetría año 1, latencia p95/p99, disponibilidad, RPO/RTO, retención y reloj del sistema. _(GAP-05, GAP-18)_
- [ ] **COND-10.** Al menos 2 revisores asignados, reloj del SLA definido y métrica de edad de la cola; capacidad medida con 20 perfiles reales. _(INC-13, SUP-04, GAP-22)_
- [ ] **COND-11.** Escritas las HU de cancelación, recuperación de contraseña, desbloqueo de PIN y revocación/suspensión con estado terminal para perfiles fraudulentos; HU-05…HU-17 con estado «Definida». _(INC-11, INC-12, INC-29, GAP-10)_
- [ ] **COND-12.** Credenciales de enfermeros y superadministrador definidas, con verificación de correo y segundo factor para el rol administrativo. _(INC-06)_
- [ ] **COND-13.** Idempotencia especificada (clave, alcance, retención, respuesta ante repetición) con criterios de aceptación comprobables, y recordatorio de verificación alineado con el TTL del código. _(INC-30, GAP-14)_
- [ ] **COND-14.** Evidencia de demanda y oferta: propuesta del auditor de al menos 15 entrevistas a usuarios y 15 a enfermeros, con disposición de pago y tarifa mínima (umbral ajustable por el equipo). _(SUP-02, SUP-03, GAP-06)_
- [ ] **COND-15.** Métricas de éxito con umbral numérico, incluida al menos una métrica de seguridad/confianza y sin depender de estados inexistentes. _(INC-17, GAP-09)_
- [ ] **COND-16.** La regla de calificación pendiente aparece igual en §19.1, §27, RN-17, índice y glosario (o solo se referencia §19.1). _(INC-10)_
- [ ] **COND-17.** Precisión del origen previa a la aceptación fijada y texto libre estructurado o filtrado para impedir datos de salud. _(INC-09, GAP-13)_
- [ ] **COND-18.** Documentos derivados (personas, glosario, HU-03 RN-08, modelo de dominio) reconciliados con la fuente normativa. _(INC-32, SUP-11)_
- [ ] **COND-19.** Mecanismo del PIN especificado sin contradicción: cómo se almacena y se muestra, protección frente a fuga de la base y alcance de «único por servicio». _(INC-28, INC-31, SUP-14)_
- [ ] **COND-20.** Rol y proceso de soporte definidos (quién, horario, SLA, facultades) y cubiertos por una HU; regla de solapamiento de horarios del enfermero decidida. _(INC-11, GAP-21, GAP-24)_
- [ ] **COND-21.** Marco contractual del intermediario definido: términos y condiciones, relación con los enfermeros como independientes, límites de responsabilidad y seguros. _(GAP-25, SUP-01)_

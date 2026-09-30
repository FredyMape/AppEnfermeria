# Informe de hallazgos de auditoría — Plataforma de Servicios de Enfermería (AppEnfermeria)

**Fecha del informe:** 2026-09-30
**Destinatario:** agente responsable de atender los hallazgos.
**Naturaleza del documento:** solo describe hallazgos, evidencia y aspectos a mejorar. No prescribe soluciones. Las sugerencias del auditor están en `Sugerencias-Auditoria-AppEnfermeria.md`.
**Fuente de los datos:** `Diagnostico-Viabilidad-AppEnfermeria.html` (objeto `DIAGNOSTICO`).

## Decisión de negocio que enmarca esta auditoría

> Decisión de negocio (2026-09-30): la aplicación se realizará como intermediario tecnológico (marketplace) que conecta usuarios con enfermeros independientes; no como prestador de servicios de salud.

Consecuencias para la auditoría:

- La clasificación regulatoria deja de ser una pregunta abierta de producto y pasa a ser una **decisión declarada** (SUP-01). El concepto jurídico sigue siendo necesario, pero para confirmar que el **diseño** encaja con el modelo elegido, no para decidir el modelo.
- La intención de operar como intermediario no fija por sí sola la calificación jurídica: la determina el diseño real. `vision.md` §7 todavía la registra como «restricción activa, no resuelta» (INC-34).
- El modelo exige un marco contractual propio (términos y condiciones, enfermeros como independientes, límites de responsabilidad, seguros): GAP-25 y COND-21.

## Alcance de esta auditoría

Auditoría acotada a lo que existe: contexto (Requirements-Context, vision, modelo de dominio, reglas, glosario, personas), HU-01…HU-04 y coherencia de la definición del MVP. Las HU aún no escritas no se cuentan como incongruencias: figuran como brechas de información.

## Convenciones

- **Etiquetas de evidencia:** `[VERIFICADA]` comprobada en el corpus o por cálculo; `[SUPUESTO]` declarado o asumido sin evidencia; `[SIN SUSTENTO]` afirmado sin respaldo o contradicho; `[INFERENCIA]` juicio del auditor.
- **Naturaleza del hallazgo:** «Hecho» es verificable en el texto citado; «Inferencia» es interpretación del auditor.
- **Severidad:** Crítica, Alta, Media o Baja.
- **Incongruencia:** solo lo que contradice a otro documento existente o a sí mismo. Las HU aún no escritas (HU-05…HU-17 y los vacíos no planificados) **no** son incongruencias: figuran como brechas.

## 1. Conclusión de la auditoría

- **Veredicto:** NO VIABLE EN SU ESTADO ACTUAL.
- **Confianza:** Media. Alta en las incongruencias (cada una tiene cita textual verificable en el corpus vigente). Baja en la viabilidad económica y de mercado: 4 de 8 dimensiones no son evaluables por falta de datos.
- **Alcance del veredicto:** El veredicto califica el conjunto de especificaciones tal como está, no la idea de negocio. Con las 4 dimensiones sin datos, el mejor resultado posible tras corregir la especificación sería «INSUFICIENTE INFORMACIÓN», no «VIABLE».
- **Puntaje global:** 2,25 / 5, calculado solo sobre 4 de 8 dimensiones evaluables.
- **Limitación de la fuente:** El repositorio contiene solo documentación y definiciones de agentes de proceso (0 archivos de código de producto). La evaluación es de la especificación, no de una implementación.

Razones principales:

- Hay 1 incongruencia crítica abierta: el modelo de asignación que define el MVP (directorio, invitación dirigida y negociación en vision.md) contradice la asignación atómica por orden de llegada y la máquina de estados de Requirements-Context (INC-03). Hay además 7 altas.
- Solo 4 de 8 dimensiones son evaluables y 3 de esas 4 quedan por debajo de 3: la regla exige que ninguna evaluada sea menor a 3 para poder ser «VIABLE».
- Las incongruencias se miden solo contra lo que existe; las HU pendientes no se cuentan como defecto. Aun así, no hay presupuesto, cronograma, equipo ni datos de mercado en ningún documento.

### Documentos revisados

- Requirements-Context.md (normativo)
- HU-01, HU-02, HU-03, HU-04 (normativo, «Definida para MVP»)
- specs/context/vision.md (status: «aprobado»; define el alcance del MVP)
- specs/context/domain-model.md, business-rules-index.md, glossary.md, personas.md (contexto derivado)

### Conteo de incongruencias

| Severidad | Cantidad |
| --- | --- |
| Crítica | 1 |
| Alta | 7 |
| Media | 11 |
| Baja | 2 |
| **Total** | **21** |

## 2. Evaluación por dimensión

| Dimensión | Puntaje | Confianza | Principal riesgo |
| --- | --- | --- | --- |
| Técnica | 3 / 5 | Media | Definición de MVP que la máquina de estados vigente no soporta, y una regla de seguridad (PIN) que no se puede cumplir tal como está escrita. |
| Financiera | N/E | — | No evaluable: no se puede saber si el proyecto es financiable ni rentable. |
| Operativa | 2 / 5 | Baja | Cuello de botella humano en el camino crítico de la oferta y ausencia de soporte para incidentes en tiempo real. |
| Mercado / demanda | N/E | — | No evaluable: la demanda y la disposición a pagar no están demostradas. |
| Legal / regulatoria | 2 / 5 | Media | Operación que debe demostrar que es intermediación y no prestación de salud, con datos biométricos, de ubicación y de terceros sin consentimiento definido. |
| Cronograma | N/E | — | No evaluable. |
| Riesgos externos | 2 / 5 | Media | Cinco o más dependencias externas críticas sin seleccionar, presupuestar ni con plan de contingencia. |
| Sostenibilidad | N/E | — | No evaluable. |

### Técnica — 3 / 5

- `[VERIFICADA]` Los principios declarados son correctos y varias decisiones ya se aplicaron: asignación única y atómica (Req §10.2), validación en backend (§21), contraseña de 12 a 64 caracteres (HU01-RN-06) y código de correo con TTL de 15 min, 5 intentos y 3 reenvíos por hora (HU01-RN-09).
- `[VERIFICADA]` Contradicción interna en el PIN: se exige guardarlo solo como hash y, a la vez, mostrárselo al usuario (Req §14.1 vs §14.2); «único por servicio» admite dos lecturas (INC-28, INC-31).
- `[VERIFICADA]` La máquina de estados del servicio tiene 5 estados (Req §8.1) y no soporta negociación, invitación dirigida, expiración ni cancelación con reglas (INC-03, INC-12). 0 coincidencias de MFA, zona horaria, latencia, RPO/RTO o volumetría en Requirements-Context, HU-01…04 y vision.md.
- `[SUPUESTO]` Que el alcance aprobado en vision.md (pagos, biometría, geolocalización, tiempo real, negociación, directorio) sea construible: no hay arquitectura, proveedores ni requisitos no funcionales.
- `[VERIFICADA]` No existe código de producto en el repositorio; no se evaluó implementación.

### Financiera — No evaluable (N/E)

- `[VERIFICADA]` 0 menciones de presupuesto total, costos, precios reales o ingresos en Requirements-Context y HU. Única cifra económica: la ausencia de presupuesto para biometría (vision §7). La comisión figura como «A definir».
- **Motivo de no evaluación:** Sin presupuesto, costos por transacción (biometría, pasarela, mensajería, nube) ni modelo de ingresos.
- **Brechas que la bloquean:** GAP-01, GAP-02, GAP-11, GAP-12

### Operativa — 2 / 5

- `[VERIFICADA]` Toda aprobación de enfermeros es manual y la hace un único rol que además configura y audita (HU-03 RN-09; Req §22). Tras unificar los estados no existe acción terminal ni suspensión (INC-29).
- `[VERIFICADA]` El «soporte» que recibe el evento de bloqueo por PIN (Req §15) y al que se puede acudir con una calificación pendiente (§19.1) no existe como rol (Req §2 fija tres roles), proceso ni historia; el desbloqueo sigue «pendiente de definición».
- `[SUPUESTO]` Que 24–36 h sea alcanzable: vision §8 admite que el SLA «se degrada rápidamente» y no hay revisores, reloj ni métricas.
- `[VERIFICADA]` 0 menciones al tamaño del equipo: la confianza de esta calificación es baja.

### Mercado / demanda — No evaluable (N/E)

- `[VERIFICADA]` vision §1 admite: «no se dispone de datos de mercado verificados»; §8 marca la baja liquidez como «No cubierto en esta fase». Todos los objetivos de métricas están en «A definir».
- **Motivo de no evaluación:** Sin datos de demanda, oferta de enfermeros, tamaño de mercado ni disposición a pagar.
- **Brechas que la bloquean:** GAP-06, GAP-09, GAP-17

### Legal / regulatoria — 2 / 5

- `[VERIFICADA]` Avance real: HU-01 y Req §4.2 ya exigen aviso de privacidad y consentimiento explícito antes de capturar datos médicos propios (HU01-RN-08; Ley 1581 de 2012).
- `[VERIFICADA]` Decisión declarada: la plataforma opera como intermediario tecnológico y no como prestador de salud. vision §7 todavía la registra como «restricción activa, no resuelta» y admite que el diseño (verificación de credenciales, agendamiento, control de inicio y fin del servicio) «tiene rasgos de prestador» bajo la Res. 3100/2019 (INC-34).
- `[VERIFICADA]` Ninguna HU incorpora consentimiento para biometría facial (que la visión exige «explícito y separado»), ubicación en tiempo real, datos del tercero titular ni datos y documentos del enfermero (HU-02). 0 menciones de política de retención o supresión de datos.
- `[SUPUESTO]` Marco legal citado por vision.md (Ley 1581/2012, Res. 3100/2019, ReTHUS): coincide con mi conocimiento general, pero no se re-verificó contra fuente primaria. Requiere concepto jurídico.
- `[INFERENCIA]` Se califica 2 y no 1: la visión ya reconoce los riesgos y hubo un avance concreto; falta que el resto llegue a las HU.
- `[INFERENCIA]` La intención de operar como intermediario no fija por sí sola la calificación jurídica: la determina el diseño real de la operación. Por eso el concepto jurídico sigue siendo necesario, ahora para confirmar que el diseño encaja con el modelo elegido.

### Cronograma — No evaluable (N/E)

- `[VERIFICADA]` 0 menciones de cronograma, hitos, fechas objetivo ni capacidad del equipo en todo el corpus.
- **Motivo de no evaluación:** Sin fechas, hitos, equipo ni estimaciones.
- **Brechas que la bloquean:** GAP-03, GAP-04, GAP-10

### Riesgos externos — 2 / 5

- `[VERIFICADA]` Dependencias externas críticas sin proveedor ni costo: pasarela de pagos, verificación facial («Proveedor de verificación facial y su costo aún no definidos», vision §7), correo/SMS y geolocalización. 0 menciones de proveedor en Requirements-Context y HU.
- `[VERIFICADA]` Conectividad obligatoria sin modo offline y sin tolerancia definida a cortes durante un servicio (vision §7, pregunta abierta 10).
- `[SUPUESTO]` Consulta del ReTHUS como fuente externa disponible y gratuita: vision §8 solo la nombra como mitigación; su disponibilidad y costo no están verificados.

### Sostenibilidad — No evaluable (N/E)

- `[VERIFICADA]` Sin unidad económica (ingreso por servicio, comisión, costo de adquisición y de verificación). La métrica de comisión está en «A definir».
- **Motivo de no evaluación:** Sin datos económicos ni de retención que permitan juzgar si el modelo se sostiene.
- **Brechas que la bloquean:** GAP-02, GAP-09, GAP-17

## 3. Incongruencias

### Severidad Crítica

#### INC-03 — Metas incompatibles

- **Severidad:** Crítica
- **Naturaleza:** Hecho
- **Ubicación:** Requirements-Context §8.2, §10.2 (nota INC-03) y §27 reglas 3–5; vision.md §5
- **Cita exacta:** §10.2: «El primer enfermero cuya confirmación sea procesada correctamente obtendrá la asignación.» · nota INC-03: reconciliar ambos modelos «quedan pendientes de diseño» · vision §5: «el enfermero puede contraofertar… Requiere una historia de negociación de precio antes de la asignación» y «dirigir/invitar la solicitud a uno específico».
- **Descripción del hallazgo:** Vision.md (aprobado) define tres modelos de asignación que conviven en el MVP: feed atómico por orden de llegada, negociación de precio previa y invitación dirigida. Requirements-Context mantiene solo Publicado → Asignado → En curso → Finalizado / Cancelado y reglas críticas (§27, 3–5) que exigen que gane el primero en confirmar. Ofertado, invitado o contraofertado no existen en esa máquina y cambian qué significa «atómico» y quién gana. La propia nota INC-03 lo reconoce sin resolverlo. Es una contradicción entre documentos existentes, no una HU ausente.

### Severidad Alta

#### INC-02 — Objetivos vs. alcance

- **Severidad:** Alta
- **Naturaleza:** Hecho
- **Ubicación:** vision.md §5 (precio híbrido, tarifa sugerida por tipo de servicio); HU-04 «Fuera de alcance»; domain-model (TipoServicio, Configuracion); Requirements-Context §9 y §17.1
- **Cita exacta:** vision §5: «el superadministrador define una tarifa sugerida por tipo de servicio» · HU-04: «Definición de tarifas.» y «Cálculo del precio del servicio.» fuera de alcance · domain-model: TipoServicio sin atributo de tarifa; Configuracion solo con minutos de ventanas · Req §17.1: «El sistema calcula el valor adicional correspondiente.»
- **Descripción del hallazgo:** El MVP define que el superadministrador fija una tarifa por tipo de servicio, pero HU-04 (la historia que gestiona los tipos) la excluye y ni TipoServicio ni Configuracion la modelan. Req §17.1 calcula un valor adicional sin base de cálculo definida, y con pasarela en el MVP una extensión implica un cobro adicional que ningún documento contempla. El mecanismo concreto de negociación y pago sí es trabajo de HU futuras (GAP-12); aquí se señala solo que las piezas existentes no encajan con lo definido.

#### INC-04 — Objetivos vs. alcance

- **Severidad:** Alta
- **Naturaleza:** Hecho
- **Ubicación:** HU-02 RN-02 y RN-04; HU-03; Req §14; vision.md §5, §7 y §8
- **Cita exacta:** HU-02 RN-02: cuatro documentos obligatorios (identidad, foto, título, tarjeta) y RN-04 «La aprobación… debe realizarse manualmente». vision §5: «una foto en vivo se contrasta contra el documento de identidad» (máx. 2 intentos) y verificación al iniciar el servicio. vision §7: «Proveedor de verificación facial y su costo aún no definidos».
- **Descripción del hallazgo:** La visión aprobada incluye verificación facial en el registro y al iniciar el servicio, pero HU-02 y HU-03 —que siguen «Definida para MVP»— verifican solo imágenes autoportadas y Req §14 solo prevé el PIN. La visión identifica la suplantación como riesgo principal y fija esa mitigación; las HU existentes la ignoran y quedaron desactualizadas frente al MVP.

#### INC-05 — Supuestos contradictorios

- **Severidad:** Alta
- **Naturaleza:** Hecho
- **Ubicación:** HU-01 (RN-08, criterios); HU-02; Req §4.2 y §21; vision.md §5 y §7
- **Cita exacta:** Resuelto solo para datos médicos propios (HU01-RN-08). Sigue: Req §4.2: «junto con el consentimiento correspondiente del titular o de quien lo represente» (sin mecanismo) · vision §7: biometría con «consentimiento explícito y separado» (0 menciones en HU-02) · Req §21: «Privacidad» solo trata control de acceso.
- **Descripción del hallazgo:** El MVP y Req §4.2 exigen consentimiento del tercero titular, y la visión exige consentimiento biométrico «explícito y separado», pero las HU existentes no lo recogen: HU-01 solo cubre datos médicos propios, sin mecanismo de representación, y HU-02 no tiene aviso ni autorización para los datos, documentos y foto del enfermero. Falta también qué registra el consentimiento (versión, fecha, revocación). Req §21 sigue tratando solo control de acceso.

#### INC-06 — Dependencias sin precedente

- **Severidad:** Alta
- **Naturaleza:** Hecho
- **Ubicación:** HU-02 (flujo, pasos 1–14); HU-01 «Fuera de alcance»; Req §4.3–4.4 y §22
- **Cita exacta:** HU-02 no contiene contraseña ni verificación de correo. HU-01 excluye «Registro y aprobación profesional de enfermeros.» Req §4.3 y §4.4 están bajo «4. Usuario / Cliente».
- **Descripción del hallazgo:** HU-02 no define contraseña, verificación de correo ni inicio de sesión del enfermero, y Req §4.3 y §4.4 están bajo «Usuario / Cliente». La cuenta del superadministrador que aprueba perfiles con datos sensibles tampoco tiene definición de creación ni protección (MFA: 0 menciones). HU-03 depende de ambas. Es un vacío dentro de una HU ya declarada «Definida para MVP».

#### INC-09 — Metas incompatibles

- **Severidad:** Alta
- **Naturaleza:** Hecho + inferencia
- **Ubicación:** Requirements-Context §9 (nota INC-09) y §12.1; vision.md (pregunta abierta 5)
- **Cita exacta:** §9: «Punto de origen.», «Descripción general del servicio.» y «Los datos personales adicionales y los datos médicos sensibles del paciente no estarán disponibles en esta etapa.» · nota INC-09: la precisión «queda sin definir de forma deliberada».
- **Descripción del hallazgo:** El origen (domicilio de una persona vulnerable) y un texto libre se muestran a todos los enfermeros aprobados sin precisión ni filtro definidos. Una dirección exacta o «paciente con demencia» filtra identidad o salud antes de la asignación, contra la regla que la misma sección enuncia. Ahora es una omisión declarada, no resuelta.

#### INC-28 — Metas incompatibles

- **Severidad:** Alta
- **Naturaleza:** Hecho + inferencia
- **Ubicación:** Requirements-Context §14.1, §14.2, §14.3 y RN-18; glossary «PIN»
- **Cita exacta:** §14.1: «El PIN no deberá almacenarse en texto plano en ningún momento.» (solo hash) · §14.2: «El usuario podrá visualizar el PIN únicamente durante la ventana de tiempo configurada».
- **Descripción del hallazgo:** Un hash es irreversible: si solo se guarda el hash, el sistema no puede mostrar el PIN al usuario. Cumplir §14.2 exige guardarlo reversible (cifrado o en claro), lo que contradice §14.1 y RN-18, o derivarlo de forma recomputable (p. ej. HMAC con un secreto), que el texto no menciona. Además, un hash simple sobre solo 10^6 valores no protege ante una fuga: reconstruí la tabla completa de SHA-256 de los 1.000.000 de PIN en 2,9 s (medición propia, SUP-14). El control real son la ventana y los 5 intentos.

#### INC-29 — Dependencias sin precedente

- **Severidad:** Alta
- **Naturaleza:** Hecho + inferencia
- **Ubicación:** Requirements-Context §5.2 y §6.1; HU-03 (estados y flujos); personas.md (Enfermero, Superadministrador)
- **Cita exacta:** §6.1: «El superadministrador podrá: Aprobar. Solicitar corrección.» · §5.2: «Rechazado» y «Corrección solicitada» se unificaron (INC-16) · personas.md: «No existe estado ni flujo para retirar la aprobación a un enfermero ya habilitado».
- **Descripción del hallazgo:** Al unificar los estados no quedó ninguna acción terminal: ante un documento falsificado o una tarjeta profesional inexistente, lo único posible es «Solicitar corrección», y el perfil puede reenviarse sin límite de ciclos ni bloqueo del documento. Tampoco existe suspensión o revocación de un perfil ya aprobado. La visión declara la suplantación de enfermeros como riesgo principal.

### Severidad Media

#### INC-27 — Consistencia documental

- **Severidad:** Media
- **Naturaleza:** Hecho
- **Ubicación:** vision.md (front-matter «status: aprobado», nota INC-01, §7 y §9)
- **Cita exacta:** Nota: «Las preguntas abiertas de la sección 9 siguen sin resolver y no quedan cubiertas por esta aprobación de alcance» · §7: la clasificación legal es «restricción activa, no resuelta» · §9: 10 preguntas abiertas (pasarela y custodia, negociación, proveedor biométrico, geolocalización, contacto de emergencia, corte de conexión, umbrales…).
- **Descripción del hallazgo:** El documento está «aprobado» y a la vez declara que las condiciones que determinan si su alcance es viable (clasificación legal, proveedor y presupuesto biométrico, pasarela, precisión de geolocalización) siguen abiertas. La aprobación no dice qué ocurre con el alcance si alguna se resuelve en contra. No se cuentan aquí las HU pendientes.

#### INC-11 — Dependencias sin precedente

- **Severidad:** Media
- **Naturaleza:** Hecho
- **Ubicación:** Requirements-Context §2, §15 y §19.1
- **Cita exacta:** §15: «notificar o generar el evento correspondiente para soporte» · §19.1: «solicitar ayuda/soporte» · §2: «Los roles serán fijos» (Usuario, Enfermero, Superadministrador).
- **Descripción del hallazgo:** Dos reglas vigentes dependen de un «soporte» que no existe entre los tres roles fijos ni en las funciones del superadministrador (§22). La regla se puede escribir, pero no cumplir: no hay a quién dirigirla. El mecanismo de desbloqueo del PIN es trabajo pendiente (GAP-10) y no se cuenta aquí.

#### INC-12 — Dependencias sin precedente

- **Severidad:** Media
- **Naturaleza:** Hecho
- **Ubicación:** Requirements-Context §16 y §26.1
- **Cita exacta:** §26.1: «Publicado / Asignado / En curso → Cancelación válida → Cancelado» · §16: «solamente podrá ser finalizado cuando se haya cumplido como mínimo la duración contratada.»
- **Descripción del hallazgo:** El texto vigente permite pasar de «En curso» a «Cancelado», mientras §16 solo prevé cerrar un servicio en curso finalizándolo tras la duración contratada; no dice qué ocurre con el cobro ni con la calificación obligatoria (§19) si se cancela en curso. Las condiciones de cancelación en sí son una HU pendiente y no se cuentan.

#### INC-13 — Alcance vs. recursos

- **Severidad:** Media
- **Naturaleza:** Hecho + inferencia
- **Ubicación:** HU-03 RN-09; Req §22; vision.md §8
- **Cita exacta:** HU-03 RN-09: «El objetivo de revisión… será de 24 a 36 horas.» · vision §8: «El SLA de revisión se degrada rápidamente al crecer el volumen», mitigación «más de un revisor + métricas de cola desde el diseño».
- **Descripción del hallazgo:** La visión reconoce el cuello de botella de la aprobación manual y su mitigación («más de un revisor + métricas de cola desde el diseño»), pero HU-03 mantiene un SLA de 24–36 h sin revisores, sin reloj (¿se reinicia al corregir?) y sin métricas. Además, la verificación facial y el contraste externo que exige la visión (INC-04) aumentan el trabajo por perfil.

#### INC-14 — Modelo de datos

- **Severidad:** Media
- **Naturaleza:** Hecho
- **Ubicación:** domain-model.md §1 y §4; vision.md §5
- **Cita exacta:** domain-model §4: «no existe ContactoEmergencia, ni un atributo de geolocalización en Enfermero, ni un estado de verificación biométrica» · vision §5: directorio con «servicios prestados, distancia, costo promedio por hora» y ubicación en tiempo real con contacto de emergencia.
- **Descripción del hallazgo:** El modelo de dominio no cubre entidades ni atributos que el MVP define (ubicación, contacto de emergencia, verificación biométrica, costo promedio por hora, servicios prestados). El modelo reconoce el vacío; la incongruencia es que el MVP se declara aprobado y su modelo de negocio no lo representa. Las HU respectivas son trabajo pendiente y no se cuentan.

#### INC-10 — Consistencia documental

- **Severidad:** Media
- **Naturaleza:** Hecho
- **Ubicación:** Requirements-Context §19.1 vs. §27 (regla 16) y §27.1 RN-17; business-rules-index CTX-RN-17; glossary «Calificación obligatoria»
- **Cita exacta:** §19.1 (corregido): «No podrá crear nuevas solicitudes de servicio ni aceptar nuevos servicios… Sí podrá continuar con servicios ya en curso» · §27 regla 16: «Una calificación pendiente bloquea el resto de funcionalidades de la aplicación hasta completarla.» · RN-17: «impedirá realizar otras acciones en la aplicación hasta completarla.»
- **Descripción del hallazgo:** CN-17 acotó el bloqueo solo en §19.1. La lista «Reglas críticas del MVP» (§27), RN-17, CTX-RN-17 del índice y el glosario conservan el bloqueo total: dos reglas normativas contradictorias sobre el mismo evento. Se rebajó a Media porque la intención (CN-17) es clara y la corrección es editorial. §19.1 además presupone una función de «ayuda/soporte» que no existe (INC-11).

#### INC-15 — Dependencias sin precedente

- **Severidad:** Media
- **Naturaleza:** Hecho + inferencia
- **Ubicación:** Requirements-Context §11, §12.2 y §29 (HU-17); vision.md §5
- **Cita exacta:** §11: «Se habilitará la información necesaria para coordinar el servicio.» · §12.2: «Los datos de contacto se habilitarán de acuerdo con las mismas reglas de tiempo» · vision: chat fuera del MVP «más allá de lo estrictamente necesario para coordinar».
- **Descripción del hallazgo:** Con la ventana por defecto de 180 min, un servicio aceptado con días de antelación no permite contactar al paciente hasta 3 h antes, mientras §11 promete habilitar «los mecanismos de comunicación» y la visión limita el chat a lo «estrictamente necesario para coordinar». No se define qué canal cubre ese mínimo ni el servicio aceptado con menos de 180 min de margen.

#### INC-17 — Métricas de éxito

- **Severidad:** Media
- **Naturaleza:** Hecho
- **Ubicación:** vision.md §2 y §6; Req §8.1 y §26
- **Cita exacta:** vision §6: «% de solicitudes PUBLICADO que llegan a ASIGNADO antes de expirar» · todos los objetivos: «A definir». vision §2: conecta «de forma confiable».
- **Descripción del hallazgo:** Una métrica depende del estado «expirar», que no existe entre los cinco estados del servicio. Ninguna de las cinco métricas mide la promesa central (confianza y seguridad: incidentes, suplantaciones detectadas, cancelaciones) y ninguna tiene umbral.

#### INC-30 — Consistencia documental

- **Severidad:** Media
- **Naturaleza:** Hecho
- **Ubicación:** Requirements-Context §4.5; HU-01 (Recordatorio de verificación y RN-09)
- **Cita exacta:** §4.5: «Si el usuario no verifica el correo dentro de las primeras 24 horas, el sistema deberá enviar un recordatorio.» · RN-09: «vigencia (TTL) de 15 minutos desde su generación… un máximo de 3 reenvíos por hora».
- **Descripción del hallazgo:** A las 24 h el código original expiró hace 23 h 45 min, así que el recordatorio no puede llevar un código válido. No se define si el recordatorio incluye un código nuevo (¿cuenta contra los 3 reenvíos por hora?), un enlace o solo invita a pedir otro, ni qué pasa si se agotan los reenvíos. CN-20 introdujo el TTL sin revisar el recordatorio.

#### INC-31 — Terminología y definiciones

- **Severidad:** Media
- **Naturaleza:** Hecho + inferencia
- **Ubicación:** Requirements-Context §14.1 (punto 5), §27 (regla 9) y RN-08
- **Cita exacta:** §14.1: «Deberá ser único por servicio.» · §27 regla 9: «El PIN debe ser único por servicio.» · RN-08: «Cada servicio tendrá su propio PIN.»
- **Descripción del hallazgo:** «Único por servicio» admite dos lecturas: (a) cada servicio tiene su PIN, aunque el valor se repita entre servicios; (b) ningún valor se repite entre servicios. La lectura (b) se agota con 10^6 valores y colisiona pronto: con 1.000 servicios activos, la probabilidad de que dos coincidan es 39,3 % (cálculo propio). Además, con hash salado la unicidad no se puede comprobar (INC-28).

#### INC-33 — Metas incompatibles

- **Severidad:** Media
- **Naturaleza:** Hecho + inferencia
- **Ubicación:** vision.md §3, §4, §5 y §9 (pregunta 9)
- **Cita exacta:** §5: «el sistema traslada la confirmación al usuario/paciente, quien indica manualmente si la persona presente coincide con la foto de perfil» · §4: «la seguridad de poder confirmar en el momento que quien llega a su domicilio es realmente el enfermero asignado» · §9: «¿Qué pasa si el paciente… no puede compartir su ubicación (ej. persona sin smartphone, adulto mayor…, paciente inconsciente)?»
- **Descripción del hallazgo:** Si el rostro no valida, la decisión pasa a quien la visión reconoce que puede no estar en condiciones de tomarla (adulto mayor sin manejo de tecnología, paciente inconsciente, o un tercero que contrató y no está presente). Con ese fallback, la mitigación de suplantación depende de la misma persona vulnerable a la que protege.

### Severidad Baja

#### INC-32 — Consistencia documental

- **Severidad:** Baja
- **Naturaleza:** Hecho
- **Ubicación:** personas.md (Superadministrador); HU-03 RN-08; glossary «Radio de servicio»; domain-model §1 y §3
- **Cita exacta:** personas.md: «Gestionar el catálogo de tipos de servicio (HU-04): consultar y cambiar estado activo/inactivo» (HU04-RN-09 dice CRUD) · HU-03 RN-08: «aprobación, rechazo y solicitud de corrección» (el rechazo ya no existe) · glossary: «solo su zona de residencia» (la geolocalización real entró al MVP) · domain-model: «foto» en Persona y en Documento (tipo «foto»).
- **Descripción del hallazgo:** Cuatro restos de decisiones anteriores en documentos derivados y una duplicación de modelo (foto modelada como atributo de Persona y como Documento), más un diagrama cuyas especializaciones opcionales no imponen el rol único (CN-07). Ninguno cambia el alcance, pero un equipo puede construir sobre el texto obsoleto.

#### INC-34 — Consistencia documental

- **Severidad:** Baja
- **Naturaleza:** Hecho + inferencia
- **Ubicación:** vision.md §7 y §8; HU-04 (descripciones de tipos de servicio); vision.md §5 (tarifa sugerida)
- **Cita exacta:** vision §7: «Validar la clasificación legal del negocio (intermediario tecnológico vs. prestador de servicios de salud)… Se registra como restricción activa, no resuelta.» · vision §8: «Reclasificación legal como prestador de salud (no intermediario)» · HU-04: «Servicio de atención de enfermería realizado en el domicilio».
- **Descripción del hallazgo:** La decisión de operar como intermediario no está propagada: vision.md sigue tratando la clasificación como abierta. Además, algunas piezas del propio diseño pueden leerse como rasgos de prestador (la plataforma define el catálogo clínico, fija tarifas sugeridas, verifica credenciales y controla inicio y fin del servicio). Es una inferencia para revisión jurídica, no una conclusión legal.

## 4. Brechas de información

Las historias de usuario pendientes se registran aquí (GAP-10), no como incongruencias.

| ID | Dato faltante | Impacto | Dimensiones que bloquea | Prioridad |
| --- | --- | --- | --- | --- |
| GAP-01 | Presupuesto total y por rubro (biometría, pasarela, nube, mensajería, revisión). | No se puede saber si el proyecto es financiable. | Financiera, Riesgos externos | Alta |
| GAP-02 | Modelo de ingresos: porcentaje de comisión, custodia del dinero y liquidación. | Sin unidad económica no hay sostenibilidad evaluable. | Financiera, Sostenibilidad | Alta |
| GAP-03 | Cronograma, hitos, fecha de lanzamiento y responsables. | No se puede evaluar factibilidad de plazo. | Cronograma | Alta |
| GAP-04 | Equipo: tamaño, perfiles, capacidad de desarrollo, revisión y soporte. | No se puede contrastar alcance con recursos. | Cronograma, Operativa | Alta |
| GAP-05 | Volumetría objetivo y requisitos no funcionales (latencia, disponibilidad, RPO/RTO, retención). | «Escalar» no tiene objetivo medible. | Técnica | Media |
| GAP-06 | Datos de mercado: demanda, oferta de enfermeros, disposición a pagar, competencia. | La hipótesis de valor no está demostrada. | Mercado / demanda | Alta |
| GAP-07 | Concepto jurídico que confirme que el diseño (verificación de credenciales, agendamiento, PIN, control de inicio y fin, tarifa sugerida) es compatible con operar como intermediario y no como prestador de salud. | Si el diseño se califica como prestación, el modelo elegido no se sostiene. | Legal / regulatoria | Alta |
| GAP-08 | Base legal restante del tratamiento: consentimiento biométrico, de ubicación y del tercero titular; aviso para datos del enfermero; qué registra el consentimiento (versión, fecha, revocación); retención y supresión. | Riesgo de cierre de la operación con datos sensibles. Ya existe el consentimiento para datos médicos propios. | Legal / regulatoria | Alta |
| GAP-09 | Umbrales numéricos de las métricas de éxito. | No hay criterio para decidir continuar o parar. | Mercado / demanda, Sostenibilidad | Media |
| GAP-10 | HU-05…HU-17 (13 historias planificadas) y 9 vacíos no planificados según el índice: cancelación, recuperación de contraseña, desbloqueo de PIN, revocación de enfermeros, negociación/pago/liquidación, reconciliación asignación–directorio, verificación facial, ubicación en tiempo real y canal de coordinación. Se listan como brecha, no como incongruencia. | El ciclo de vida del servicio y el alcance aprobado no están especificados. | Técnica, Cronograma | Alta |
| GAP-11 | Selección de proveedores: pasarela, verificación facial, correo/SMS, almacenamiento, mapas. | Costos y riesgos externos sin cuantificar. | Riesgos externos, Financiera | Media |
| GAP-12 | Mecanismo concreto de precio y negociación (rondas, última palabra). | Sin él no se puede diseñar la asignación. | Financiera, Técnica | Alta |
| GAP-13 | Precisión y frecuencia de la geolocalización. | Afecta privacidad, costo y diseño. | Técnica, Legal / regulatoria | Media |
| GAP-14 | Especificación de idempotencia (clave, alcance, retención, respuesta ante repetición). | La aceptación atómica no es verificable. | Técnica | Media |
| GAP-16 | Contacto de emergencia: obligatoriedad, cantidad, excepciones (paciente sin dispositivo o inconsciente). | Funcionalidad de seguridad sin definir. | Operativa, Legal / regulatoria | Media |
| GAP-17 | Estrategia para lograr liquidez del marketplace (vision §8: «No cubierto»). | Riesgo principal de negocio sin plan. | Mercado / demanda, Sostenibilidad | Alta |
| GAP-18 | Reloj del sistema: zona horaria, evaluación de ventanas solo en servidor y qué configuración aplica a servicios ya asignados. | Riesgo de exponer o retirar datos sensibles por error. | Técnica | Media |
| GAP-19 | Nivel de competencia (auxiliar/profesional) y requisitos documentales y legales por tipo de servicio (p. ej. transporte, hospital). | Servicios aceptables por quien no puede prestarlos. | Legal / regulatoria, Operativa | Media |
| GAP-21 | Modelo de soporte: quién es «soporte», horario, SLA y facultades (desbloqueo de PIN, ayuda en servicio, disputas). | Bloqueos y emergencias en campo sin responsable. | Operativa | Alta |
| GAP-22 | Criterios y fuente del revisor para aprobar o pedir corrección (qué se verifica y contra qué fuente oficial), y límite de ciclos de corrección. | La aprobación manual no es reproducible ni auditable. | Operativa, Legal / regulatoria | Media |
| GAP-23 | Proceso de recaptura de la foto de referencia cada 6–12 meses: quién lo dispara y qué pasa si el enfermero no lo hace (vision §7: «debe diseñarse»). | La verificación facial pierde vigencia y el enfermero puede quedar activo con una referencia obsoleta. | Operativa | Media |
| GAP-24 | Regla de solapamiento de horarios del enfermero: nada impide que acepte dos servicios simultáneos (domain-model §2 lo reconoce como vacío). | Un enfermero puede quedar asignado a dos domicilios a la vez. | Técnica, Operativa | Media |
| GAP-25 | Marco contractual del intermediario: términos y condiciones, relación con los enfermeros como profesionales independientes, límites de responsabilidad de la plataforma y seguros. | Sin él, el modelo de intermediación no está formalizado y la responsabilidad ante un daño al paciente no está delimitada. | Legal / regulatoria, Operativa | Alta |

## 5. Supuestos críticos

| ID | Supuesto | Estado | Fuente | Dimensión | Consecuencia si es falso |
| --- | --- | --- | --- | --- | --- |
| SUP-01 | La operación como intermediario tecnológico (decisión de negocio) será reconocida como tal y no como prestación de servicios de salud. | SUPUESTO | Decisión de negocio 2026-09-30; vision.md §5 y §7 | Legal / regulatoria | Habría que habilitarse como prestador (REPS), con otro modelo operativo, costos y responsabilidad clínica; el diseño actual (verificación de credenciales, control de inicio y fin) estaría en el centro del análisis. |
| SUP-02 | Hay demanda suficiente de personas dispuestas a contratar enfermería por plataforma en lugar de canales informales. | SUPUESTO | vision.md §1 (declarado hipótesis) | Mercado / demanda | El lado de la demanda no llega y el marketplace no funciona. |
| SUP-03 | Habrá suficientes enfermeros dispuestos a operar por la plataforma con la comisión que se defina. | SIN SUSTENTO | vision.md §4 (propuesta de valor) | Mercado / demanda | La oferta no cubre la demanda y las solicitudes expiran sin aceptar. |
| SUP-04 | La aprobación manual por un solo rol puede mantener un SLA de 24–36 h. | SIN SUSTENTO | HU-03 RN-09 | Operativa | La oferta se retrasa o abandona y el MVP no gana enfermeros. |
| SUP-05 | Cuatro imágenes autoportadas revisadas por una persona bastan para acreditar identidad, vigencia y ausencia de sanciones. | SIN SUSTENTO | HU-02 RN-02 y RN-04 (vigentes mientras no se incorpore la verificación facial de vision §5) | Técnica | Un impostor o un profesional sancionado presta servicios en domicilios. |
| SUP-06 | El PIN de 6 dígitos prueba que el enfermero y el paciente están juntos. | SIN SUSTENTO | Req §14; vision §5 (CN-18 aceptó el riesgo residual) | Operativa | Se cobra tiempo no prestado y se pierde el control antifraude principal. |
| SUP-07 | Existe un proveedor de verificación facial con costo asumible y cumplimiento normativo. | SUPUESTO | vision.md §7 (proveedor y costo sin definir) | Riesgos externos | La mitigación central de suplantación no se puede implementar o excede el presupuesto. |
| SUP-08 | Exigir internet sin modo offline es aceptable para la operación. | SUPUESTO | vision.md §7 y §8 | Riesgos externos | Servicios que no pueden iniciar o seguirse en zonas de baja cobertura. |
| SUP-09 | Una persona solo necesita un rol (usuario, enfermero o superadministrador). | SUPUESTO | business-rules-index CN-07 (decisión del developer) | Técnica | Un enfermero no puede contratar para su familia y migrar después es costoso. |
| SUP-10 | Con 5 intentos sobre 10^6 combinaciones, adivinar el PIN tiene probabilidad de 0,0005 %. | VERIFICADA | Req §15 (recalculado) | Técnica | El límite de intentos sería insuficiente frente a fuerza bruta. |
| SUP-11 | Las resoluciones CN-10 a CN-21 quedaron reflejadas en todos los documentos fuente y derivados. | SIN SUSTENTO | business-rules-index.md (registro de cambios y resoluciones CN-10 a CN-21) | Técnica | El equipo construye sobre reglas contradictorias y el registro de cambios deja de ser confiable. |
| SUP-12 | Una ventana de 180 minutos antes del inicio es suficiente para coordinar y a la vez proteger datos sensibles. | SUPUESTO | Req §12 y §13 | Operativa | Se puede impedir la coordinación previa o exponer datos innecesariamente. |
| SUP-13 | El correo electrónico es un canal fiable para códigos y notificaciones críticas. | SUPUESTO | Req §4.4 y HU-03 | Riesgos externos | Cuentas sin verificar y enfermeros sin enterarse de correcciones o aprobaciones. Con TTL de 15 min, un correo lento invalida el código. |
| SUP-14 | Un PIN de 6 dígitos guardado como hash sigue protegido si se filtra la base de datos. | SIN SUSTENTO | Req §14.1 y RN-18 | Técnica | Quien obtenga la base recupera los PIN de los servicios activos en segundos y puede iniciar servicios o suplantar. |
| SUP-15 | Un enfermero puede prestar «Acompañamiento y transporte» y «Atención hospitalaria» con los mismos requisitos que un servicio domiciliario. | SUPUESTO | HU-04 (tipos de servicio) y HU-02 (requisitos únicos) | Legal / regulatoria | Se acepta un servicio de transporte sin licencia o seguro, o una atención hospitalaria sin autorización del centro. |

Resumen por estado: VERIFICADA 1 · SUPUESTO 8 · SIN SUSTENTO 6 (total 15).

## 6. Pre-mortem (causas probables de fracaso a 12 meses)

Probabilidad e impacto son juicios ordinales del auditor (`[INFERENCIA]`), en escala 1 a 5.

| ID | Causa | Prob. | Imp. | Puntaje | Señal temprana | Hallazgos relacionados |
| --- | --- | --- | --- | --- | --- | --- |
| PM-6 | Falta de liquidez: pocos enfermeros o pocos usuarios activos y baja tasa de aceptación, sin mitigación declarada. | 4 | 5 | 20 | Solicitudes publicadas sin aceptar; retención de enfermeros a 30 días baja; ninguna hipótesis de demanda validada. | SUP-02, SUP-03, GAP-06, GAP-17 |
| PM-4 | El MVP no sale: el alcance aprobado (pagos, biometría, geolocalización, tiempo real, negociación, directorio) no está especificado ni presupuestado. | 4 | 4 | 16 | HU-05…HU-17 sin redactar cerca del arranque; preguntas abiertas de vision §9 sin respuesta; sin presupuesto ni fechas. | INC-27, INC-02, GAP-03 |
| PM-5 | La cola de aprobación de enfermeros crece, la oferta no se activa y el marketplace arranca sin liquidez. | 4 | 4 | 16 | Edad del perfil más antiguo mayor a 24 h; un solo revisor; sin métricas de cola. | INC-13, SUP-04 |
| PM-1 | Incidente grave en un domicilio por un enfermero suplantado o no habilitado, con daño reputacional y legal para la plataforma. | 3 | 5 | 15 | Perfiles aprobados sin contraste con ReTHUS; un perfil dudoso que solo puede devolverse a corrección; tarjeta profesional duplicada; primera queja de identidad. | INC-04, INC-29, INC-33, SUP-05 |
| PM-2 | Requerimiento o sanción de la autoridad de protección de datos por tratar biometría, ubicación y datos de terceros sin consentimiento implementado. | 3 | 5 | 15 | El piloto sale sin consentimiento separado para biometría ni mecanismo para el tercero titular; primer reclamo de un titular. | INC-05, GAP-08 |
| PM-3 | Pese a la decisión de operar como intermediario, la autoridad o un tercero califica la operación como prestación de servicios de salud y exige habilitación. | 3 | 5 | 15 | Se empieza HU-05 sin concepto jurídico escrito sobre el diseño; consulta de una aseguradora o EPS sobre habilitación; contratos con enfermeros ausentes. | SUP-01, GAP-07, GAP-25, INC-34 |
| PM-7 | Servicios bloqueados en campo (PIN agotado, sin conectividad) con un paciente esperando y sin rol ni ruta de soporte. | 4 | 3 | 12 | Primeros tickets de PIN bloqueado; servicios que no pasan de «Asignado». | INC-11, GAP-21, SUP-08 |
| PM-10 | Compromiso de la cuenta del superadministrador (sin MFA definido) con acceso a documentos y configuración de ventanas de datos sensibles. | 2 | 5 | 10 | Accesos administrativos desde ubicaciones inusuales; cambios de configuración sin ticket. | INC-06 |
| PM-8 | Fraude de facturación: inicio del servicio con el PIN dictado a distancia, cobrando tiempo no prestado. | 3 | 3 | 9 | Servicios iniciados sin coincidencia de ubicación; extensiones sistemáticas. | SUP-06 |
| PM-11 | Para poder mostrar el PIN se guarda reversible o en claro y una fuga de la base expone los PIN de servicios activos. | 3 | 3 | 9 | HU del PIN escrita sin resolver la contradicción hash/visualización; PIN legible en revisión de código; sin secreto fuera de la base. | INC-28, SUP-14 |

## 7. Cifras recalculadas por el auditor

| Cálculo | Resultado | Estado | Nota |
| --- | --- | --- | --- |
| 180 min = 3 h; 15:00 − 180 min | 12:00 | Correcto | Req §12 y §13 |
| PIN visible: 15:00 − 5 min | 14:55 | Correcto | Req §14.2 |
| Inicio 10:00 + duración 2 h | 12:00 | Correcto | Req §16 |
| 5 intentos / 10^6 combinaciones | 0,0005 % | Correcto | Req §15; SUP-10 |
| Tabla de 1.000.000 SHA-256 sin sal (000000–999999) | 2,87 s; inversión del hash de prueba inmediata | Correcto | Medición propia en esta auditoría; SUP-14 e INC-28 |
| Colisión de PIN con 1.000 servicios activos: 1 − e^(−n(n−1)/2N), N = 10^6 | 39,3 % | Correcto | Cálculo propio; INC-31 (lectura «único entre todos los servicios») |
| Recordatorio 24 h − TTL 15 min | 23 h 45 min con el código vencido | Correcto | Req §4.5 y HU01-RN-09; INC-30 |

## 8. Ajustes realizados durante el análisis

- Incorporé la decisión de negocio del 2026-09-30 (intermediario, no prestador de salud): SUP-01 pasa de «hipótesis» a «decisión que debe confirmarse jurídicamente sobre el diseño»; añadí INC-34 (decisión no propagada a vision.md), GAP-25 y COND-21. No cambia el veredicto: la decisión no resuelve las incongruencias ni las dimensiones sin datos.
- Reencuadré como incongruencias solo lo que contradice a otro documento existente: INC-02 (tarifa del MVP vs. HU-04), INC-11 («soporte» vs. roles fijos), INC-12 (§26.1 vs. §16) e INC-14 (modelo vs. MVP).
- Retiré INC-20 (solapamiento de horarios) porque es una regla que falta, no una contradicción: pasó a GAP-24. Retiré INC-22 porque audita un informe y no el contexto.
- Bajé INC-02 y INC-04 a Alta y INC-27 a Media: su parte de «HU sin escribir» ya no cuenta; queda solo la contradicción con documentos existentes.
- Añadí INC-33 (fallback de la verificación facial), detectado al revisar la coherencia interna de la visión.
- Mantuve el veredicto NO VIABLE EN SU ESTADO ACTUAL: baja a 1 la cifra de críticas, pero hay 7 altas y 3 de las 4 dimensiones evaluadas quedan bajo 3.
- Dejé Financiera, Mercado, Cronograma y Sostenibilidad como N/E en lugar de estimarlas.

## 9. Limitaciones del análisis

- Evalúo documentación, no un producto: el repositorio no tiene código de producto.
- No hubo entrevistas ni acceso a stakeholders; la intención real puede diferir del texto.
- Las referencias legales (Ley 1581 de 2012, Res. 3100/2019, ReTHUS) provienen de vision.md; no se re-verificaron contra fuente primaria y no constituyen concepto jurídico.
- Las probabilidades e impactos del pre-mortem son juicios ordinales del auditor (inferencia), no estadísticas.
- La medición del hash se hizo con SHA-256 sin sal en una sola máquina; no dice qué tan rápido lo harían atacantes con hardware dedicado ni evalúa diseños alternativos (HMAC, hash lento).
- Cuatro de ocho dimensiones no son evaluables; el puntaje global solo resume las evaluadas.
- La decisión de operar como intermediario es una intención de negocio; no constituye ni sustituye un concepto jurídico sobre la calificación de la operación.

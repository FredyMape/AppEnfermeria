# Informe de hallazgos de auditoría — Plataforma de Servicios de Enfermería (AppEnfermeria)

**Fecha del informe:** 2026-09-29
**Destinatario:** agente responsable de atender los hallazgos.
**Naturaleza del documento:** solo describe hallazgos, evidencia y aspectos a mejorar. No prescribe soluciones. Las sugerencias del auditor están en `Sugerencias-Auditoria-AppEnfermeria.md`.
**Fuente de los datos:** `Diagnostico-Viabilidad-AppEnfermeria.html` (objeto `DIAGNOSTICO`).

## Convenciones

- **Etiquetas de evidencia:** `[VERIFICADA]` comprobada en el corpus o por cálculo; `[SUPUESTO]` declarado o asumido sin evidencia; `[SIN SUSTENTO]` afirmado sin respaldo o contradicho; `[INFERENCIA]` juicio del auditor.
- **Naturaleza del hallazgo:** «Hecho» es verificable en el texto citado; «Inferencia» es interpretación del auditor.
- **Severidad:** Crítica, Alta, Media o Baja.
- **«Aspecto a mejorar»:** describe qué está deficiente, no cómo resolverlo.

## 1. Conclusión de la auditoría

- **Veredicto:** NO VIABLE EN SU ESTADO ACTUAL.
- **Confianza:** Media. Alta en las incongruencias (cada una tiene cita textual verificable). Baja en la viabilidad económica y de mercado: 4 de 8 dimensiones no son evaluables por falta de datos.
- **Alcance del veredicto:** El veredicto califica el conjunto de especificaciones tal como está, no la idea de negocio. Con las 4 dimensiones sin datos, el mejor resultado posible tras corregir la especificación sería «INSUFICIENTE INFORMACIÓN», no «VIABLE».
- **Puntaje global:** 2,25 / 5, calculado solo sobre 4 de 8 dimensiones evaluables.
- **Limitación de la fuente:** El repositorio contiene solo documentación (0 archivos de código). La evaluación es de la especificación, no de una implementación.

Razones principales:

- Hay 5 incongruencias críticas abiertas: el MVP está definido de dos maneras incompatibles (vision.md borrador vs. Requirements-Context y HU-01…04) en pagos, modelo de asignación, verificación de identidad y consentimiento.
- Solo 4 de 8 dimensiones son evaluables y 3 de esas 4 quedan por debajo de 3: la regla exige que ninguna evaluada sea menor a 3 para poder ser «VIABLE».
- No existe presupuesto, cronograma, equipo ni datos de mercado en ningún documento, y 13 historias (HU-05…HU-17) más 4 vacíos no planificados siguen sin escribirse.

### Documentos revisados

- Requirements-Context.md (normativo)
- HU-01, HU-02, HU-03, HU-04 (normativo, «Definida para MVP»)
- specs/context/vision.md (status: «borrador»)
- specs/context/domain-model.md, business-rules-index.md, glossary.md, personas.md (derivados)
- Auditoria-Especificacion.md (auditoría previa: tratada como fuente a verificar, no como verdad)

### Conteo de incongruencias

| Severidad | Cantidad |
| --- | --- |
| Crítica | 5 |
| Alta | 9 |
| Media | 9 |
| Baja | 3 |
| **Total** | **26** |

## 2. Evaluación por dimensión

| Dimensión | Puntaje | Confianza | Principal riesgo |
| --- | --- | --- | --- |
| Técnica | 3 / 5 | Media | Alcance técnico ampliado en vision.md sin arquitectura, proveedores ni requisitos no funcionales que lo respalden. |
| Financiera | N/E | — | No evaluable: no se puede saber si el proyecto es financiable ni rentable. |
| Operativa | 2 / 5 | Baja | Cuello de botella humano en el camino crítico de la oferta y ausencia de soporte para incidentes en tiempo real. |
| Mercado / demanda | N/E | — | No evaluable: la demanda y la disposición a pagar no están demostradas. |
| Legal / regulatoria | 2 / 5 | Media | Tratamiento de datos de salud, biométricos y de ubicación sin base legal implementada, sobre una clasificación regulatoria sin resolver. |
| Cronograma | N/E | — | No evaluable. |
| Riesgos externos | 2 / 5 | Media | Cinco o más dependencias externas críticas sin seleccionar, presupuestar ni con plan de contingencia. |
| Sostenibilidad | N/E | — | No evaluable. |

### Técnica — 3 / 5

- `[VERIFICADA]` Los principios declarados son correctos: asignación única y atómica (Req §10.2, CTX-RN-01/02), validación en backend (§21), PIN con hash (CTX-RN-18) y contraseña de 12 a 64 caracteres (HU01-RN-06).
- `[VERIFICADA]` Faltan decisiones sin las cuales no se puede implementar ni probar: idempotencia solo enunciada (§10.3, sin clave ni retención), TTL/intentos del código de correo pendientes (HU-01), y 0 coincidencias de zona horaria, MFA, latencia o volumetría en los documentos normativos.
- `[SUPUESTO]` Que el alcance de vision.md (pagos, biometría, geolocalización, tiempo real, negociación) sea construible: no hay arquitectura, proveedores ni requisitos no funcionales.
- `[VERIFICADA]` No existe código en el repositorio; no se evaluó implementación.

### Financiera — No evaluable (N/E)

- `[VERIFICADA]` 0 menciones de presupuesto total, costos, precios reales o ingresos. Única cifra económica: la ausencia de presupuesto para biometría (vision §7). La comisión figura como «A definir».
- **Motivo de no evaluación:** Sin presupuesto, costos por transacción (biometría, pasarela, mensajería, nube) ni modelo de ingresos.
- **Brechas que la bloquean:** GAP-01, GAP-02, GAP-11, GAP-12

### Operativa — 2 / 5

- `[VERIFICADA]` Toda aprobación de enfermeros es manual y la hace un rol (superadministrador) que además configura, audita y —según CN/§15— debería atender bloqueos (HU-03 RN-09; Req §22).
- `[VERIFICADA]` No existe proceso de soporte: el desbloqueo tras 5 intentos de PIN está «pendiente de definición» (Req §15) y sin HU; la recaptura periódica de foto «debe diseñarse» (vision §7).
- `[SUPUESTO]` Que 24–36 h sea alcanzable: la propia visión reconoce que el SLA «se degrada rápidamente» (vision §8) y no define revisores ni reloj.
- `[VERIFICADA]` 0 menciones al tamaño del equipo: la confianza de esta calificación es baja.

### Mercado / demanda — No evaluable (N/E)

- `[VERIFICADA]` vision §1 admite: «no se dispone de datos de mercado verificados»; §8 marca la baja liquidez como «No cubierto en esta fase». Todos los objetivos de métricas están en «A definir».
- **Motivo de no evaluación:** Sin datos de demanda, oferta de enfermeros, tamaño de mercado ni disposición a pagar.
- **Brechas que la bloquean:** GAP-06, GAP-09, GAP-17

### Legal / regulatoria — 2 / 5

- `[VERIFICADA]` La clasificación intermediario vs. prestador de salud está declarada «restricción activa, no resuelta» (vision §7) y diseño actual «tiene rasgos de prestador».
- `[VERIFICADA]` Los flujos normativos recogen datos de salud (HU-01) sin autorización, aviso ni finalidad, aunque la visión exige «base legal, consentimiento y aviso de privacidad antes de recolectar» (vision §7). Sin consentimiento biométrico ni del tercero titular en ninguna HU.
- `[SUPUESTO]` Marco citado por la auditoría previa (Ley 1581/2012 art. 23, Res. 3100/2019, Ley 1164/2007): coincide con mi conocimiento general, pero no fue re-verificado contra fuente primaria en esta sesión. Requiere concepto jurídico.
- `[INFERENCIA]` Se califica 2 y no 1 porque la visión ya reconoce los riesgos y propone mitigaciones; el problema es que no llegaron a las HU.

### Cronograma — No evaluable (N/E)

- `[VERIFICADA]` 0 menciones de cronograma, hitos, fechas objetivo ni capacidad del equipo en todo el corpus.
- **Motivo de no evaluación:** Sin fechas, hitos, equipo ni estimaciones.
- **Brechas que la bloquean:** GAP-03, GAP-04, GAP-10, GAP-20

### Riesgos externos — 2 / 5

- `[VERIFICADA]` Dependencias externas críticas sin proveedor ni costo: pasarela de pagos, verificación facial (vision §7 y preguntas abiertas), correo/SMS y geolocalización. 0 menciones de proveedor en los documentos normativos.
- `[VERIFICADA]` Conectividad obligatoria sin modo offline y sin tolerancia definida a cortes durante un servicio (vision §7, pregunta abierta 10).
- `[SUPUESTO]` Consulta del ReTHUS como fuente externa disponible y gratuita (afirmación de la auditoría previa; no verificada aquí).

### Sostenibilidad — No evaluable (N/E)

- `[VERIFICADA]` Sin unidad económica (ingreso por servicio, comisión, costo de adquisición y de verificación). La métrica de comisión está en «A definir».
- **Motivo de no evaluación:** Sin datos económicos ni de retención que permitan juzgar si el modelo se sostiene.
- **Brechas que la bloquean:** GAP-02, GAP-09, GAP-17

## 3. Incongruencias

### Severidad Crítica

#### INC-01 — Dos definiciones de MVP sin regla de precedencia

- **Severidad:** Crítica
- **Tipo:** Consistencia documental
- **Naturaleza:** Hecho
- **Ubicación:** vision.md (front-matter y §5) vs. HU-01…HU-04 y Requirements-Context.md
- **Cita exacta:** vision.md: status: «borrador» y «Pasarela de pagos integrada en el MVP». HU-01…04: «Estado: Definida para MVP».
- **Descripción del hallazgo:** Hay dos definiciones de MVP. El documento en borrador amplía el alcance (pagos, biometría, geolocalización, directorio, negociación, tiempo real) y declara que «reabre y reemplaza» una restricción de HU-02, pero las HU «definidas» y Requirements-Context no se actualizaron y no hay regla de precedencia. Cada equipo construiría un producto distinto según el documento que lea.
- **Aspecto a mejorar:** Falta un documento normativo único y una regla de precedencia entre documentos; el estado de aprobación de vision.md no está definido.

#### INC-02 — Precio y pagos exigidos pero no definidos en ninguna historia

- **Severidad:** Crítica
- **Tipo:** Objetivos vs. alcance
- **Naturaleza:** Hecho
- **Ubicación:** Requirements-Context §9 y §17.1; HU-01/03/04 «Fuera de alcance»; vision.md §5; Req §29
- **Cita exacta:** §9: «Precio ofrecido.» · §17.1: «El sistema calcula el valor adicional correspondiente.» · HU-04: «Definición de tarifas.» fuera de alcance · vision §5: «Pasarela de pagos integrada en el MVP (no se pospone a una fase posterior)».
- **Descripción del hallazgo:** El sistema muestra y recalcula precios y la visión exige cobrar en el MVP, pero ninguna HU definida ni pendiente (HU-05…HU-17) cubre tarifa, negociación, pago, comisión ni liquidación. CN-06 confirmó el modelo híbrido pero admite que «no crea todavía el mecanismo concreto».
- **Aspecto a mejorar:** Ausencia de definición de tarifa, negociación, pago, comisión y liquidación, frente a un MVP que los exige.

#### INC-03 — Asignación por orden de llegada incompatible con negociación y directorio

- **Severidad:** Crítica
- **Tipo:** Metas incompatibles
- **Naturaleza:** Hecho
- **Ubicación:** Requirements-Context §8.2 y §10.2; vision.md §5
- **Cita exacta:** §10.2: «El primer enfermero cuya confirmación sea procesada correctamente obtendrá la asignación.» · vision §5: «el enfermero puede contraofertar… Requiere una historia de negociación de precio antes de la asignación» y «dirigir/invitar la solicitud a uno específico».
- **Descripción del hallazgo:** La asignación atómica por orden de llegada es incompatible con contraoferta previa y con invitación dirigida. Ambas exigen estados y reglas (ofertada, invitada, contraofertada) que la máquina de estados (Publicado → Asignado) no tiene, y cambian qué significa «atómico» y quién gana.
- **Aspecto a mejorar:** El mecanismo de asignación del MVP no está definido de forma única; la máquina de estados no cubre negociación ni invitación dirigida.

#### INC-04 — Verificación de identidad del enfermero desalineada con la visión

- **Severidad:** Crítica
- **Tipo:** Objetivos vs. alcance
- **Naturaleza:** Hecho
- **Ubicación:** HU-02 RN-02 y RN-04; HU-03; Req §14; vision.md §5 y §8
- **Cita exacta:** HU-02 RN-02: cuatro documentos obligatorios (identidad, foto, título, tarjeta) y RN-04 «La aprobación… debe realizarse manualmente». vision §5: «una foto en vivo se contrasta contra el documento de identidad» (máx. 2 intentos). vision §8: mitigación «biometría/prueba de vida y contraste… contra el ReTHUS antes de aprobar».
- **Descripción del hallazgo:** La visión identifica como riesgo la suplantación de un enfermero en el domicilio y fija su mitigación, pero HU-02/03 verifican solo imágenes autoportadas y Req §14 solo prevé el PIN. Los antecedentes disciplinarios siguen como documento opcional (HU-02). La auditoría previa propone 3 intentos biométricos (CA-E-06) frente a los 2 de la visión.
- **Aspecto a mejorar:** La verificación de identidad y de credenciales no está alineada con el riesgo y la mitigación declarados en la visión.

#### INC-05 — Datos sensibles recogidos sin autorización ni consentimiento

- **Severidad:** Crítica
- **Tipo:** Supuestos contradictorios
- **Naturaleza:** Hecho
- **Ubicación:** HU-01 (criterios y flujo); Req §4.2 y §21; vision.md §5 y §7
- **Cita exacta:** HU-01: «Los datos médicos propios pueden almacenarse durante el registro.» (ningún paso de autorización) · vision §7: el tratamiento de salud «requiere base legal, consentimiento y aviso de privacidad antes de recolectar datos médicos»; biometría con «consentimiento explícito y separado».
- **Descripción del hallazgo:** El flujo normativo recoge datos de salud (y, según la visión, biométricos y de ubicación) sin autorización, aviso ni finalidad, y sin contemplar al tercero titular (HU01-RN-05). Req §21 se titula «Privacidad» pero solo trata control de acceso. La visión declara la obligación y las HU la ignoran.
- **Aspecto a mejorar:** Los flujos que recogen datos de salud, biométricos y de ubicación no incorporan autorización, aviso ni consentimiento; el tercero titular no está contemplado.

### Severidad Alta

#### INC-06 — Enfermero y superadministrador sin definición de credenciales

- **Severidad:** Alta
- **Tipo:** Dependencias sin precedente
- **Naturaleza:** Hecho
- **Ubicación:** HU-02 (flujo, pasos 1–14); HU-01 «Fuera de alcance»; Req §4.3–4.4 y §22
- **Cita exacta:** HU-02 no contiene contraseña ni verificación de correo. HU-01 excluye «Registro y aprobación profesional de enfermeros.» Req §4.3 y §4.4 están bajo «4. Usuario / Cliente».
- **Descripción del hallazgo:** Ninguna historia define cómo el enfermero crea credenciales, verifica su correo o inicia sesión, ni cómo se crea y protege (MFA: 0 menciones) la cuenta del superadministrador que aprueba perfiles con datos sensibles. HU-03 depende de ambas.
- **Aspecto a mejorar:** No hay definición de credenciales, verificación de correo ni protección de cuenta para enfermeros y superadministrador.

#### INC-07 — Atención hospitalaria dentro y fuera del MVP

- **Severidad:** Alta
- **Tipo:** Objetivos vs. alcance
- **Naturaleza:** Hecho
- **Ubicación:** Requirements-Context §7; HU-04 RN-01 y criterios; vision.md §5
- **Cita exacta:** Req §7: «Para el MVP se definirán… 4. Atención hospitalaria.» · vision §5 «Explícitamente fuera del MVP»: «Atención hospitalaria dentro de una IPS mediante convenio formal».
- **Descripción del hallazgo:** El catálogo sembrado del MVP incluye un tipo que la visión excluye del MVP. HU-04 exige que el sistema «cuenta con los cuatro tipos», así que se ofrecería a los usuarios algo que el negocio dice que no presta.
- **Aspecto a mejorar:** El estado del tipo «Atención hospitalaria» en el MVP es contradictorio entre documentos.

#### INC-08 — Restricción de radio de servicio contradicha por la visión

- **Severidad:** Alta
- **Tipo:** Supuestos contradictorios
- **Naturaleza:** Hecho
- **Ubicación:** Req §5.1 y §9; HU-02 RN-05; glossary.md; vision.md §5
- **Cita exacta:** Req §5.1: «No se almacenará ni solicitará un radio de servicio. Solamente se almacenará la zona de residencia.» · Req §9: «Distancia aproximada.» · vision §5: «Geolocalización real del enfermero (no solo zona de residencia como texto libre)».
- **Descripción del hallazgo:** Para mostrar distancia hace falta una referencia geográfica del enfermero; la restricción vigente lo impide y la visión la deroga sin actualizar Req, HU-02 ni el glosario. Además la zona no tiene tipo de dato (texto libre, código DANE o coordenadas).
- **Aspecto a mejorar:** La restricción sobre zona/radio de servicio está vigente en unos documentos y derogada en otro; el tipo de dato de la zona no está definido.

#### INC-09 — Origen y texto libre expuestos antes de la aceptación

- **Severidad:** Alta
- **Tipo:** Metas incompatibles
- **Naturaleza:** Hecho + inferencia
- **Ubicación:** Requirements-Context §9 y §12.1; vision.md (pregunta abierta 5)
- **Cita exacta:** §9: «Punto de origen.», «Descripción general del servicio.» y «Los datos personales adicionales y los datos médicos sensibles del paciente no estarán disponibles en esta etapa.» · vision: «¿Qué nivel de precisión de geolocalización se requiere (ciudad/barrio vs. coordenadas exactas)…?»
- **Descripción del hallazgo:** La lista previa a la aceptación muestra el origen (el domicilio de una persona vulnerable) y un texto libre a todos los enfermeros aprobados, sin precisión definida ni filtro. Una dirección exacta o una descripción como «paciente con demencia» filtra identidad o salud antes de la asignación, contradiciendo la regla que la misma sección enuncia.
- **Aspecto a mejorar:** La precisión del origen y el contenido del texto libre visibles antes de la aceptación no están definidos; hay riesgo de exposición de identidad o salud.

#### INC-10 — Bloqueo total por calificación pendiente

- **Severidad:** Alta
- **Tipo:** Metas incompatibles
- **Naturaleza:** Hecho + inferencia
- **Ubicación:** Requirements-Context §19.1, RN-17 (§27.1), §14.2 y §17; vision.md §3
- **Cita exacta:** §19.1: «No podrá realizar otras acciones dentro de la aplicación.» · vision §3: «ambos lados del marketplace deben tener fricción mínima para lograr liquidez».
- **Descripción del hallazgo:** El bloqueo total por calificación pendiente impediría consultar el PIN, aprobar una extensión de un servicio en curso, cancelar o pedir ayuda mientras el usuario debe calificar otro servicio. Contradice el objetivo de fricción mínima y las reglas de PIN y extensión.
- **Aspecto a mejorar:** El alcance del bloqueo por calificación pendiente es amplio respecto de otras reglas y del objetivo de fricción mínima.

#### INC-11 — Bloqueo de PIN sin mecanismo de desbloqueo

- **Severidad:** Alta
- **Tipo:** Dependencias sin precedente
- **Naturaleza:** Hecho
- **Ubicación:** Requirements-Context §15 y §29; business-rules-index.md (Vacíos)
- **Cita exacta:** §15: «El mecanismo exacto para desbloquear el servicio después de alcanzar el límite de intentos queda pendiente de definición.»
- **Descripción del hallazgo:** Tras 5 intentos fallidos el servicio queda bloqueado sin salida, con el paciente esperando, y ninguna HU (ni de la lista de §29) lo asume. La visión sí define un fallback para la verificación facial pero no para el PIN.
- **Aspecto a mejorar:** El mecanismo de desbloqueo tras el límite de intentos de PIN no está definido ni tiene historia asignada.

#### INC-12 — Cancelación sin reglas ni historia

- **Severidad:** Alta
- **Tipo:** Dependencias sin precedente
- **Naturaleza:** Hecho
- **Ubicación:** Requirements-Context §8.2, §16, §26.1 y §29
- **Cita exacta:** §26.1: «Publicado / Asignado / En curso → Cancelación válida → Cancelado… Las condiciones específicas de cancelación deberán definirse en la historia de usuario correspondiente.» · §16: «solamente podrá ser finalizado cuando se haya cumplido como mínimo la duración contratada.»
- **Descripción del hallazgo:** La historia de cancelación no existe ni está en la lista de pendientes. Se permite pasar de «En curso» a «Cancelado» sin reglas, mientras §16 solo prevé finalizar tras la duración contratada: no hay terminación anticipada, no-show ni expiración de solicitudes sin aceptar.
- **Aspecto a mejorar:** Las reglas de cancelación, terminación anticipada, no-show y expiración no están definidas; la historia no existe.

#### INC-13 — SLA de revisión sin capacidad, reloj ni métricas

- **Severidad:** Alta
- **Tipo:** Alcance vs. recursos
- **Naturaleza:** Hecho + inferencia
- **Ubicación:** HU-03 RN-09; Req §22; vision.md §8
- **Cita exacta:** HU-03 RN-09: «El objetivo de revisión… será de 24 a 36 horas.» · vision §8: «El SLA de revisión se degrada rápidamente al crecer el volumen», mitigación «más de un revisor + métricas de cola desde el diseño».
- **Descripción del hallazgo:** La visión reconoce el cuello de botella y su mitigación, pero HU-03 mantiene un SLA sin revisores, sin reloj (¿se reinicia al corregir?) y sin métricas. Con las premisas de la auditoría previa (no medidas) el SLA se rompe con pocas decenas de registros al día; ver cálculo en Autoverificación.
- **Aspecto a mejorar:** El SLA de revisión no tiene capacidad, reloj ni métricas definidos, y la mitigación reconocida en la visión no está incorporada.

#### INC-14 — Ubicación en tiempo real y contacto de emergencia sin modelo

- **Severidad:** Alta
- **Tipo:** Modelo de datos
- **Naturaleza:** Hecho
- **Ubicación:** vision.md §5 y preguntas abiertas 8–10; domain-model.md §1; HU-01
- **Cita exacta:** vision §5: «el usuario/paciente debe tener su ubicación activa y compartida en tiempo real con un contacto de emergencia designado».
- **Descripción del hallazgo:** Ubicación en tiempo real y contacto de emergencia están en el MVP, pero el modelo de dominio no tiene entidades para ellos, HU-01 no captura contacto de emergencia y las preguntas de la visión (obligatoriedad, paciente sin GPS, cortes de conexión) siguen abiertas. Es dato sensible continuo de una persona vulnerable.
- **Aspecto a mejorar:** Estas funcionalidades no tienen modelo de datos, historia ni respuesta a las preguntas abiertas de la visión.

### Severidad Media

#### INC-15 — Canal y momento de coordinación sin definir

- **Severidad:** Media
- **Tipo:** Dependencias sin precedente
- **Naturaleza:** Hecho + inferencia
- **Ubicación:** Requirements-Context §11, §12.2 y §29 (HU-17); vision.md §5
- **Cita exacta:** §11: «Se habilitará la información necesaria para coordinar el servicio.» · §12.2: «Los datos de contacto se habilitarán de acuerdo con las mismas reglas de tiempo» · vision: chat fuera del MVP «más allá de lo estrictamente necesario para coordinar».
- **Descripción del hallazgo:** Con la ventana por defecto de 180 min, un servicio aceptado con días de antelación no permite contactar al paciente hasta 3 h antes, y no está definido qué canal cubre «lo estrictamente necesario». Tampoco se define el caso de un servicio que se acepta con menos de 180 min de margen ni el «servicio no programado».
- **Aspecto a mejorar:** El canal de coordinación previo al servicio y el tratamiento de servicios con margen menor a la ventana no están definidos.

#### INC-16 — Rechazado y Corrección solicitada con comportamiento idéntico

- **Severidad:** Media
- **Tipo:** Terminología y definiciones
- **Naturaleza:** Hecho
- **Ubicación:** Requirements-Context §5.2 y §6.1; HU-03 (flujos de rechazo y corrección, RN-05)
- **Cita exacta:** §5.2: «Rechazado y Corrección solicitada son estados independientes: el primero indica que la información no fue aprobada… el segundo que se requieren ajustes puntuales». HU-03: ambos flujos terminan igual (corrige, solicita nueva revisión, vuelve a «Pendiente de revisión»). §6.1: «Rechazar / solicitar corrección» como una sola acción.
- **Descripción del hallazgo:** Dos estados con significados distintos y comportamiento idéntico: la distinción no tiene efecto. No hay estado terminal de rechazo ni apelación, y solo el rechazo exige comentario por regla (RN-05).
- **Aspecto a mejorar:** Los dos estados se definen distintos pero se comportan igual; falta estado terminal o apelación.

#### INC-17 — Métricas de éxito sin umbral ni medición de confianza

- **Severidad:** Media
- **Tipo:** Métricas de éxito
- **Naturaleza:** Hecho
- **Ubicación:** vision.md §2 y §6; Req §8.1 y §26
- **Cita exacta:** vision §6: «% de solicitudes PUBLICADO que llegan a ASIGNADO antes de expirar» · todos los objetivos: «A definir». vision §2: conecta «de forma confiable».
- **Descripción del hallazgo:** Una métrica depende del estado «expirar», que no existe en los cinco estados del servicio. Además ninguna de las cinco métricas mide la promesa central (confianza y seguridad: incidentes, suplantaciones detectadas, cancelaciones) y ninguna tiene umbral.
- **Aspecto a mejorar:** Las métricas no tienen umbral, no miden confianza/seguridad y una depende de un estado inexistente.

#### INC-18 — Usuario y paciente usados como sinónimos frente al PIN

- **Severidad:** Media
- **Tipo:** Terminología y definiciones
- **Naturaleza:** Hecho + inferencia
- **Ubicación:** Requirements-Context §14.3 y §4.2; vision.md §5; HU01-RN-05
- **Cita exacta:** §14.3: «El usuario consulta el PIN… El usuario proporciona el PIN al enfermero.» · vision §5: «el PIN sigue validando la presencia del paciente/usuario».
- **Descripción del hallazgo:** «Usuario» y «paciente» se usan como sinónimos, pero cuando el servicio es para un tercero son personas distintas: el PIN lo ve quien contrata, que puede estar ausente. Un PIN dictable por teléfono no prueba presencia, que es lo que la visión le atribuye.
- **Aspecto a mejorar:** El destinatario del PIN en servicios para terceros y el valor probatorio del PIN no están definidos.

#### INC-19 — Diagrama de dominio contradice la cardinalidad 1:1

- **Severidad:** Media
- **Tipo:** Modelo de datos
- **Naturaleza:** Hecho
- **Ubicación:** specs/context/domain-model.md §3 y §4
- **Cita exacta:** §4: «una misma Persona no puede registrarse bajo más de un Rol». Diagrama: «PERSONA ||--o{ ROL : asociado a (1:1, resuelto 2026-09-22)» y tres especializaciones opcionales «||--o|».
- **Descripción del hallazgo:** El diagrama expresa que una persona tiene cero o muchos roles y permite tener a la vez perfil de Usuario y de Enfermero, contra la decisión 1:1 (CN-07) que el propio texto cita.
- **Aspecto a mejorar:** El diagrama del modelo de dominio no refleja la cardinalidad 1:1 decidida.

#### INC-20 — Regla inexistente atribuida a la asignación única

- **Severidad:** Media
- **Tipo:** Modelo de datos
- **Naturaleza:** Hecho
- **Ubicación:** specs/context/domain-model.md §2 (Enfermero → Servicio)
- **Cita exacta:** «…un enfermero puede aceptar múltiples servicios… pero solo uno activo a la vez por regla de asignación única.»
- **Descripción del hallazgo:** CTX-RN-01 dice que un servicio tiene un solo enfermero, no que un enfermero tenga un solo servicio activo. La regla atribuida no existe y, al mismo tiempo, nada regula que un enfermero acepte servicios con horarios solapados.
- **Aspecto a mejorar:** La regla atribuida no existe en la fuente y el solapamiento de servicios del enfermero no tiene regla.

#### INC-21 — Experiencia profesional obligatoria en el requisito y opcional en la historia

- **Severidad:** Media
- **Tipo:** Consistencia documental
- **Naturaleza:** Hecho
- **Ubicación:** Requirements-Context §5.1 vs. HU-02 («Experiencia profesional», criterios de aceptación)
- **Cita exacta:** Req §5.1: «El registro deberá incluir información personal y profesional» (años, experiencia, lugares…). HU-02: «El enfermero podrá registrar: Años de experiencia. Experiencia profesional…»; ningún criterio de aceptación la exige.
- **Descripción del hallazgo:** El requisito dice «deberá incluir» y la historia lo hace opcional. Lo que el revisor debe poder evaluar no está definido.
- **Aspecto a mejorar:** La obligatoriedad de los datos de experiencia difiere entre el requisito y la historia.

#### INC-22 — Auditoría previa desactualizada y con errores de cálculo

- **Severidad:** Media
- **Tipo:** Cifras
- **Naturaleza:** Hecho
- **Ubicación:** Auditoria-Especificacion.md §1.2, §3.4.5, §4.4.1, §6.2.1, Anexo A.2
- **Cita exacta:** «La cadena a1!… cumple los tres requisitos literales y el backend la aceptaría.» · tabla M/M/1: «15 | 0,50 | 1,9 h».
- **Descripción del hallazgo:** La auditoría previa (22-sep) describe como abiertos defectos ya corregidos (contraseña sin longitud: HU01-RN-06 exige 12–64; PIN en claro: CTX-RN-18; §9/§12.1 y numeración duplicada; lista de datos sensibles vacía). Contiene errores de cálculo: con μ=30/día y λ=15, W=24/(30−15)=1,6 h, no 1,9 h; y «≈7/día» mezcla multitarea con reprocesos (sin ellos son 9/día). La cifra «14 veces pendiente de definición» no se reproduce: hay 3 coincidencias exactas en Requirements+HU. Sus recomendaciones N:M y «probablemente no CRUD» fueron decididas al revés (CN-07, CN-05) sin dejar razón registrada.
- **Aspecto a mejorar:** El documento de auditoría previa está desactualizado, contiene errores de cálculo y no registra la razón de las recomendaciones descartadas.

#### INC-23 — Flujo de código expirado sin TTL definido

- **Severidad:** Media
- **Tipo:** Dependencias sin precedente
- **Naturaleza:** Hecho
- **Ubicación:** HU-01 (Flujos alternativos: código incorrecto y código expirado)
- **Cita exacta:** «Las reglas de expiración y cantidad máxima de intentos quedan pendientes de definición.» y, a continuación, «Si el código ha expirado: El sistema deberá informar al usuario.»
- **Descripción del hallazgo:** El flujo de código expirado presupone un tiempo de vida que la misma historia declara no definido. Sin TTL ni límite de intentos no se puede escribir el criterio de aceptación ni la prueba de seguridad.
- **Aspecto a mejorar:** El flujo depende de un TTL declarado como no definido; los límites de intentos y reenvíos tampoco están definidos.

### Severidad Baja

#### INC-24 — Foto y zona de residencia ubicadas en entidades distintas

- **Severidad:** Baja
- **Tipo:** Modelo de datos
- **Naturaleza:** Hecho
- **Ubicación:** HU-02 «Información base de Persona» vs. Req §3.1 y domain-model.md §1
- **Cita exacta:** HU-02 ubica «Foto» y «Zona de residencia» en Persona; Req §3.1 y domain-model las ubican en Enfermero.
- **Descripción del hallazgo:** Dónde vive cada dato cambia el modelo de datos: Persona se comparte entre roles y el dato es solo del enfermero.
- **Aspecto a mejorar:** La ubicación conceptual de «Foto» y «Zona de residencia» difiere entre documentos.

#### INC-25 — Ventana del PIN sin valor por defecto

- **Severidad:** Baja
- **Tipo:** Consistencia documental
- **Naturaleza:** Hecho
- **Ubicación:** Requirements-Context §12 y §14.2; domain-model.md (Configuracion)
- **Cita exacta:** §12: «El valor por defecto es de 180 minutos» (datos sensibles) · §14.2: «Configuración: 5 minutos antes» (solo ejemplo).
- **Descripción del hallazgo:** La ventana de datos sensibles tiene valor por defecto y la del PIN no. Si nadie la configura, el comportamiento es indefinido.
- **Aspecto a mejorar:** La ventana del PIN no tiene valor por defecto, a diferencia de la ventana de datos sensibles.

#### INC-26 — Registro de cambios no coincide con el documento

- **Severidad:** Baja
- **Tipo:** Consistencia documental
- **Naturaleza:** Hecho
- **Ubicación:** business-rules-index.md (CN-01) vs. Requirements-Context §12.1
- **Cita exacta:** Índice: «§12.1 reescrita como referencia a §9; eliminada la lista duplicada.» El archivo aún contiene bajo §12.1 la lista completa de 11 elementos.
- **Descripción del hallazgo:** El registro de cambios afirma algo que el documento no cumple. Hoy las dos listas coinciden, pero siguen siendo dos fuentes que pueden divergir.
- **Aspecto a mejorar:** El registro de cambios declara una corrección que el documento fuente no refleja.

## 4. Brechas de información

| ID | Dato faltante | Impacto | Dimensiones que bloquea | Prioridad |
| --- | --- | --- | --- | --- |
| GAP-01 | Presupuesto total y por rubro (biometría, pasarela, nube, mensajería, revisión). | No se puede saber si el proyecto es financiable. | Financiera, Riesgos externos | Alta |
| GAP-02 | Modelo de ingresos: porcentaje de comisión, custodia del dinero y liquidación. | Sin unidad económica no hay sostenibilidad evaluable. | Financiera, Sostenibilidad | Alta |
| GAP-03 | Cronograma, hitos, fecha de lanzamiento y responsables. | No se puede evaluar factibilidad de plazo. | Cronograma | Alta |
| GAP-04 | Equipo: tamaño, perfiles, capacidad de desarrollo, revisión y soporte. | No se puede contrastar alcance con recursos. | Cronograma, Operativa | Alta |
| GAP-05 | Volumetría objetivo y requisitos no funcionales (latencia, disponibilidad, RPO/RTO, retención). | «Escalar» no tiene objetivo medible. | Técnica | Media |
| GAP-06 | Datos de mercado: demanda, oferta de enfermeros, disposición a pagar, competencia. | La hipótesis de valor no está demostrada. | Mercado / demanda | Alta |
| GAP-07 | Concepto jurídico sobre intermediario vs. prestador de salud. | Puede invalidar el modelo operativo. | Legal / regulatoria | Alta |
| GAP-08 | Base legal del tratamiento: autorizaciones, aviso de privacidad, consentimiento biométrico y del tercero, retención y supresión. | Riesgo de cierre de la operación con datos sensibles. | Legal / regulatoria | Alta |
| GAP-09 | Umbrales numéricos de las métricas de éxito. | No hay criterio para decidir continuar o parar. | Mercado / demanda, Sostenibilidad | Media |
| GAP-10 | HU-05…HU-17 (13 historias) y 4 vacíos no planificados: cancelación, recuperación de contraseña, desbloqueo de PIN, revocación de enfermeros. | El ciclo de vida del servicio no está especificado. | Técnica, Cronograma | Alta |
| GAP-11 | Selección de proveedores: pasarela, verificación facial, correo/SMS, almacenamiento, mapas. | Costos y riesgos externos sin cuantificar. | Riesgos externos, Financiera | Media |
| GAP-12 | Mecanismo concreto de precio y negociación (rondas, última palabra). | Sin él no se puede diseñar la asignación. | Financiera, Técnica | Alta |
| GAP-13 | Precisión y frecuencia de la geolocalización. | Afecta privacidad, costo y diseño. | Técnica, Legal / regulatoria | Media |
| GAP-14 | Especificación de idempotencia (clave, alcance, retención, respuesta ante repetición). | La aceptación atómica no es verificable. | Técnica | Media |
| GAP-15 | Reglas del código de correo: TTL, intentos, uso único, reenvíos. | Cuentas atacables por fuerza bruta. | Técnica | Media |
| GAP-16 | Contacto de emergencia: obligatoriedad, cantidad, excepciones (paciente sin dispositivo o inconsciente). | Funcionalidad de seguridad sin definir. | Operativa, Legal / regulatoria | Media |
| GAP-17 | Estrategia para lograr liquidez del marketplace (vision §8: «No cubierto»). | Riesgo principal de negocio sin plan. | Mercado / demanda, Sostenibilidad | Alta |
| GAP-18 | Reloj del sistema: zona horaria, evaluación de ventanas solo en servidor y qué configuración aplica a servicios ya asignados. | Riesgo de exponer o retirar datos sensibles por error. | Técnica | Media |
| GAP-19 | Nivel de competencia (auxiliar/profesional) y requisitos documentales por tipo de servicio (p. ej. transporte). | Servicios aceptables por quien no puede prestarlos. | Legal / regulatoria, Operativa | Media |
| GAP-20 | Regla de precedencia entre documentos y estado de aprobación de vision.md. | El equipo no sabe qué construir. | Técnica, Cronograma | Alta |

## 5. Supuestos críticos

| ID | Supuesto | Estado | Fuente | Dimensión | Consecuencia si es falso |
| --- | --- | --- | --- | --- | --- |
| SUP-01 | La plataforma es un intermediario tecnológico y no un prestador de servicios de salud. | SUPUESTO | vision.md §5 y §7 | Legal / regulatoria | Habría que habilitarse como prestador, con otro modelo operativo, costos y responsabilidad clínica. |
| SUP-02 | Hay demanda suficiente de personas dispuestas a contratar enfermería por plataforma en lugar de canales informales. | SUPUESTO | vision.md §1 (declarado hipótesis) | Mercado / demanda | El lado de la demanda no llega y el marketplace no funciona. |
| SUP-03 | Habrá suficientes enfermeros dispuestos a operar por la plataforma con la comisión que se defina. | SIN SUSTENTO | vision.md §4 (propuesta de valor) | Mercado / demanda | La oferta no cubre la demanda y las solicitudes expiran sin aceptar. |
| SUP-04 | La aprobación manual por un solo rol puede mantener un SLA de 24–36 h. | SIN SUSTENTO | HU-03 RN-09 | Operativa | La oferta se retrasa o abandona y el MVP no gana enfermeros. |
| SUP-05 | Cuatro imágenes autoportadas revisadas por una persona bastan para acreditar identidad, vigencia y ausencia de sanciones. | SIN SUSTENTO | HU-02 RN-02 y RN-04 | Técnica | Un impostor o un profesional sancionado presta servicios en domicilios. |
| SUP-06 | El PIN de 6 dígitos prueba que el enfermero y el paciente están juntos. | SIN SUSTENTO | Req §14; vision §5 | Operativa | Se cobra tiempo no prestado y se pierde el control antifraude principal. |
| SUP-07 | Existe un proveedor de verificación facial con costo asumible y cumplimiento normativo. | SUPUESTO | vision.md §7 (proveedor y costo sin definir) | Riesgos externos | La mitigación central de suplantación no se puede implementar o excede el presupuesto. |
| SUP-08 | Exigir internet sin modo offline es aceptable para la operación. | SUPUESTO | vision.md §7 y §8 | Riesgos externos | Servicios que no pueden iniciar o seguirse en zonas de baja cobertura. |
| SUP-09 | Una persona solo necesita un rol (usuario, enfermero o superadministrador). | SUPUESTO | business-rules-index CN-07 | Técnica | Un enfermero no puede contratar para su familia y migrar después es costoso. |
| SUP-10 | Con 5 intentos sobre 10^6 combinaciones, adivinar el PIN tiene probabilidad de 0,0005 %. | VERIFICADA | Req §15 (recalculado) | Técnica | El límite de intentos sería insuficiente frente a fuerza bruta. |
| SUP-11 | Las decisiones CN-02 a CN-09 se aplicaron a los documentos fuente. | VERIFICADA | business-rules-index.md | Técnica | El registro de cambios sería poco fiable. |
| SUP-12 | Una ventana de 180 minutos antes del inicio es suficiente para coordinar y a la vez proteger datos sensibles. | SUPUESTO | Req §12 y §13 | Operativa | Se puede impedir la coordinación previa o exponer datos innecesariamente. |
| SUP-13 | El correo electrónico es un canal fiable para códigos y notificaciones críticas. | SUPUESTO | Req §4.4 y HU-03 | Riesgos externos | Cuentas sin verificar y enfermeros sin enterarse de correcciones o aprobaciones. |

Resumen por estado: VERIFICADA 2 · SUPUESTO 7 · SIN SUSTENTO 4 (total 13).

## 6. Pre-mortem (causas probables de fracaso a 12 meses)

Probabilidad e impacto son juicios ordinales del auditor (`[INFERENCIA]`), en escala 1 a 5.

| ID | Causa | Prob. | Imp. | Puntaje | Señal temprana | Hallazgos relacionados |
| --- | --- | --- | --- | --- | --- | --- |
| PM-6 | Falta de liquidez: pocos enfermeros o pocos usuarios activos y baja tasa de aceptación, sin mitigación declarada. | 4 | 5 | 20 | Solicitudes publicadas sin aceptar; retención de enfermeros a 30 días baja; ninguna hipótesis de demanda validada. | SUP-02, SUP-03, GAP-06 |
| PM-4 | El MVP no sale: el alcance (pagos, biometría, geolocalización, tiempo real, negociación, directorio) excede lo definido y lo presupuestado. | 4 | 4 | 16 | HU-05…HU-17 sin redactar cerca del arranque; contradicciones vision/Req sin resolver; sin presupuesto ni fechas. | INC-01, INC-02, GAP-03 |
| PM-5 | La cola de aprobación de enfermeros crece, la oferta no se activa y el marketplace arranca sin liquidez. | 4 | 4 | 16 | Edad del perfil más antiguo mayor a 24 h; un solo revisor; sin métricas de cola. | INC-13, SUP-04 |
| PM-1 | Incidente grave en un domicilio por un enfermero suplantado o no habilitado, con daño reputacional y legal para la plataforma. | 3 | 5 | 15 | Perfiles aprobados sin contraste con ReTHUS; tarjeta profesional duplicada; primera queja de identidad. | INC-04, SUP-05 |
| PM-2 | Requerimiento o sanción de la autoridad de protección de datos por tratar salud, biometría y ubicación sin base legal implementada. | 3 | 5 | 15 | El piloto sale sin aviso de privacidad ni autorización en el flujo; primer reclamo de un titular. | INC-05, GAP-08 |
| PM-3 | Se descubre tarde que la plataforma requiere habilitación como prestador de salud y el modelo operativo no es viable tal como está. | 3 | 5 | 15 | Se empieza HU-05 sin concepto jurídico escrito; consulta de una aseguradora o EPS sobre habilitación. | SUP-01, GAP-07 |
| PM-7 | Servicios bloqueados en campo (PIN agotado, sin conectividad) con un paciente esperando y sin ruta de soporte. | 4 | 3 | 12 | Primeros tickets de PIN bloqueado; servicios que no pasan de «Asignado». | INC-11, SUP-08 |
| PM-10 | Compromiso de la cuenta del superadministrador (sin MFA definido) con acceso a documentos y configuración de ventanas de datos sensibles. | 2 | 5 | 10 | Accesos administrativos desde ubicaciones inusuales; cambios de configuración sin ticket. | INC-06 |
| PM-8 | Fraude de facturación: inicio del servicio con el PIN dictado a distancia, cobrando tiempo no prestado. | 3 | 3 | 9 | Servicios iniciados sin coincidencia de ubicación; extensiones sistemáticas. | INC-18, SUP-06 |
| PM-9 | Abandono por bloqueo total ante calificación pendiente, incluso en momentos de urgencia. | 3 | 2 | 6 | Quejas por no poder usar la app; calificaciones puestas al azar para desbloquear. | INC-10 |

## 7. Cifras recalculadas por el auditor

| Cálculo | Resultado | Estado | Nota |
| --- | --- | --- | --- |
| 180 min = 3 h; 15:00 − 180 min | 12:00 | Correcto | Req §12 y §13 |
| PIN visible: 15:00 − 5 min | 14:55 | Correcto | Req §14.2 |
| Inicio 10:00 + duración 2 h | 12:00 | Correcto | Req §16 |
| 5 intentos / 10^6 combinaciones | 0,0005 % | Correcto | Req §15; SUP-10 |
| W = 24 h / (30 − λ), λ = 10 · 20 · 25 · 28 · 29 | 1,2 · 2,4 · 4,8 · 12 · 24 h | Correcto | Coincide con la auditoría previa (modelo M/M/1, premisas no medidas) |
| W con λ = 15 | 24 / 15 = 1,6 h (la auditoría dice 1,9 h) | Discrepancia | Error de la auditoría previa; ver INC-22 |
| Ruptura con 35 % de reprocesos: 1,35·λ = 29 | λ ≈ 21,5 / día | Correcto | Coincide con la auditoría previa |
| Ruptura con μ = 10/día: sin reprocesos / con reprocesos | λ = 9 / λ ≈ 6,7 | Correcto | La auditoría dice «≈7» sin separar ambos casos |
| [Inferencia] p95 del tiempo de revisión ≤ 24 h exige μ − λ_ef ≥ 3/día (p95 ≈ 3 × media en M/M/1) | λ ≤ 27 · ≤ 20 con reprocesos · ≤ 5,2 con reprocesos y μ = 10 | Correcto | Mi cálculo; la media de 24 h de la auditoría previa dejaría ~37 % de perfiles fuera del SLA |

## 8. Ajustes realizados durante el análisis

- Descarté como defectos abiertos varios hallazgos de la auditoría previa que ya están corregidos: contraseña sin longitud (ahora 12–64), PIN en claro (CTX-RN-18), lista de datos sensibles vacía, secciones duplicadas y contradicción de HU-04. Los registré como INC-22.
- Detecté que la afirmación del índice sobre §12.1 (CN-01) no se cumple en el archivo: la lista duplicada sigue ahí (INC-26).
- Detecté un error de cálculo en la auditoría previa (W=1,6 h para λ=15, no 1,9 h) y una mezcla de supuestos en «≈7/día» (INC-22).
- Bajé de «Crítica» a «Alta» la ausencia de credenciales del enfermero (INC-06) y la contradicción de zona/geolocalización (INC-08), porque la visión ya reconoce la segunda y la primera no bloquea la definición del modelo.
- Califiqué Legal con 2 y no con 1 para no afirmar inviabilidad legal que no puedo demostrar; queda como riesgo alto sujeto a concepto jurídico.
- Dejé Financiera, Mercado, Cronograma y Sostenibilidad como N/E en lugar de estimarlas.

## 9. Limitaciones del análisis

- Evalúo documentación, no un producto: el repositorio no tiene código.
- No hubo entrevistas ni acceso a stakeholders; la intención real puede diferir del texto.
- Las referencias legales de la auditoría previa (Ley 1581, Res. 3100/2019, Ley 1164, ReTHUS) no se re-verificaron contra fuente primaria y no constituyen concepto jurídico.
- Las probabilidades e impactos del pre-mortem son juicios ordinales del auditor (inferencia), no estadísticas.
- El cálculo de capacidad de revisión usa premisas de la auditoría previa (12 min/perfil, 6 h/día, 35 % de correcciones) que no están medidas en el proyecto; modela llegadas y servicio con supuestos simplificadores.
- Cuatro de ocho dimensiones no son evaluables; el puntaje global solo resume las evaluadas.

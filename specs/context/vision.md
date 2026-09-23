---
status: "borrador"
---

# Visión de Producto — Plataforma de Servicios de Enfermería

## 1. Problema

Hoy, una persona que necesita un servicio de enfermería (para sí misma o para un familiar) recurre a canales informales y fragmentados: contactos de conocidos, referidos boca a boca, llamadas directas a EPS/IPS de atención domiciliaria, o búsqueda ad-hoc de enfermeros independientes. Estos canales comparten los mismos problemas:

- **Sin verificación centralizada:** no hay forma sencilla de confirmar credenciales, experiencia o antecedentes disciplinarios del profesional antes de dejarlo entrar al domicilio.
- **Sin comparación:** no existe un lugar donde comparar calificación, experiencia, distancia y costo entre varios enfermeros disponibles.
- **Fricción de coordinación:** acordar precio, horario y condiciones ocurre por fuera de cualquier sistema, sin trazabilidad ni respaldo si algo sale mal.
- **Oferta desaprovechada:** enfermeros con tiempo disponible entre turnos u otros trabajos no tienen un canal ágil para encontrar servicios adicionales.

> Nota: no se dispone de datos de mercado verificados sobre la frecuencia relativa de cada canal informal en Colombia (agencias vs. contactos personales vs. EPS/IPS). Se registra como hipótesis a validar con investigación de usuarios, no como hecho confirmado.

## 2. Visión

Una plataforma que conecta de forma confiable, ágil y centralizada a personas que necesitan servicios de enfermería con enfermeros verificados y aprobados, permitiendo tanto publicar una solicitud abierta a la oferta disponible como elegir directamente a un enfermero específico por su perfil, calificación, distancia y costo.

## 3. Usuarios objetivo (resumen)

- **Usuario / Cliente** — persona que contrata el servicio, para sí misma o para un tercero (familiar, conocido). Rol con mayor prioridad de experiencia junto con Enfermero: ambos lados del marketplace deben tener fricción mínima para lograr liquidez.
- **Enfermero** — profesional o auxiliar de enfermería verificado que ofrece sus servicios, ya sea aceptando solicitudes publicadas o siendo contactado directamente desde su perfil.
- **Superadministrador** — rol operativo que verifica y aprueba enfermeros, configura el catálogo de servicios y las ventanas de datos sensibles, y audita la operación.

Detalle completo en [personas.md](personas.md).

## 4. Propuesta de valor por rol

| Rol                | Qué gana al usar el producto                                                                                                                                                                                                                                                                                                                                                                                      |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Usuario / Cliente  | Contratar un servicio de enfermería de forma sencilla, rápida, ágil y centralizada — para sí mismo o para cualquier persona que desee —, con la confianza de que el enfermero fue verificado y aprobado, con visibilidad de calificación, experiencia, distancia y costo antes de decidir, y con la seguridad de poder confirmar en el momento que quien llega a su domicilio es realmente el enfermero asignado. |
| Enfermero          | Mayor flujo de trabajo: acceso a más clientes sin depender de intermediarios tradicionales, generando ingresos adicionales y reduciendo el tiempo muerto entre turnos u otros trabajos.                                                                                                                                                                                                                           |
| Superadministrador | Control centralizado sobre quién puede prestar servicios en la plataforma y sobre las reglas de exposición de información sensible, con trazabilidad de sus decisiones.                                                                                                                                                                                                                                           |

## 5. Alcance del MVP

### Incluido en el MVP

- Registro y verificación de Usuario y Enfermero (`HU-01`, `HU-02`, `HU-03`).
- Catálogo de tipos de servicio (`HU-04`).
- **Dos modelos de descubrimiento conviviendo:**
  - **Feed:** el usuario publica una solicitud y los enfermeros elegibles la ven y aceptan (modelo ya descrito en `Requirements-Context.md`).
  - **Directorio:** el usuario navega perfiles de enfermeros (descripción de servicios que puede tomar según su experiencia, calificación, servicios prestados, distancia, costo promedio por hora) y puede dirigir/invitar la solicitud a uno específico.
- **Geolocalización real del enfermero** (no solo zona de residencia como texto libre) para poder calcular y mostrar distancia — esto reabre y reemplaza la restricción actual de "solo zona de residencia, sin radio de servicio" (`Requirements-Context.md` §5.1, RN implícita).
- **Modelo de precio híbrido:** el superadministrador define una tarifa sugerida por tipo de servicio; el usuario puede ofertar un valor distinto al publicar; el enfermero puede contraofertar según su propio criterio. Requiere una historia de negociación de precio antes de la asignación.
- **Pasarela de pagos integrada** en el MVP (no se pospone a una fase posterior).
- Aceptación atómica, PIN de inicio, finalización, extensión y calificación de servicios (`HU-05` a `HU-13`, hoy pendientes de definición).
- El negocio se concibe como **intermediario tecnológico** (marketplace), no como prestador de servicios de salud — esta clasificación debe validarse formalmente (ver Restricciones) antes de operar en producción.
- **Verificación de identidad facial del enfermero, reutilizable en dos momentos:**
  - **En el registro:** una foto en vivo se contrasta contra el documento de identidad cargado. Máximo 2 intentos; si no coincide, el perfil pasa a revisión manual del superadministrador, quien puede habilitar intentos adicionales según el caso.
  - **Al iniciar el servicio:** valida que el enfermero presente es el mismo del perfil aprobado. Es un mecanismo **independiente del PIN**: el PIN sigue validando la presencia del paciente/usuario, el rostro valida la identidad del enfermero.
  - **Fallback si la validación automática falla en campo:** el sistema traslada la confirmación al usuario/paciente, quien indica manualmente si la persona presente coincide con la foto de perfil del enfermero.
  - **Vigencia de la foto de referencia:** se recaptura periódicamente (cada 6-12 meses) para evitar desactualización por cambios de apariencia.
  - El dato biométrico (rostro) requiere **consentimiento explícito y separado**, distinto del consentimiento general de tratamiento de datos.
- **Conectividad y ubicación en tiempo real durante el servicio:** la conexión a internet es obligatoria para iniciar y mantener un servicio (sin modo offline). El enfermero mantiene su ubicación activa durante todo el servicio, y el usuario/paciente debe tener su ubicación activa y compartida en tiempo real con un contacto de emergencia designado.

### Explícitamente fuera del MVP (por ahora)

- Atención hospitalaria dentro de una IPS mediante convenio formal (requiere due diligence legal aparte).
- Chat/comunicación enriquecida entre usuario y enfermero más allá de lo estrictamente necesario para coordinar el servicio.
- Gestión avanzada de disputas con mediación estructurada (se define una vez exista el flujo base de servicio).

## 6. Métricas de éxito

| Métrica                                         | Objetivo                                                             | Cómo se mide                                                               |
| ----------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| Servicios completados por mes                   | A definir con negocio tras el lanzamiento (sin línea base histórica) | Conteo de servicios en estado `FINALIZADO` por período                     |
| Tasa de aceptación de solicitudes publicadas    | A definir                                                            | % de solicitudes `PUBLICADO` que llegan a `ASIGNADO` antes de expirar      |
| Retención de enfermeros activos (30/60/90 días) | A definir                                                            | % de enfermeros aprobados que aceptan al menos un servicio en cada ventana |
| Tiempo promedio entre publicación y asignación  | A definir                                                            | Promedio de `asignado_en − publicado_en`                                   |
| Ingresos / comisión generada                    | A definir                                                            | Suma de comisión retenida por la plataforma sobre servicios finalizados    |

> Los objetivos numéricos quedan pendientes de definición con negocio — se registran las métricas y su fórmula de cálculo, no los umbrales, para no inventar cifras sin respaldo.

## 7. Restricciones y supuestos conocidos

- **Validar la clasificación legal del negocio (intermediario tecnológico vs. prestador de servicios de salud)** antes de operar en producción. La intención de negocio es operar como intermediario, pero la auditoría advierte que el diseño actual (verificación de credenciales, agendamiento, control de inicio/fin del servicio) tiene rasgos de prestador de servicios de salud bajo la Resolución 3100 de 2019. Se registra como restricción activa, no resuelta.
- El modelo de precio híbrido (tarifa sugerida + oferta del usuario + contraoferta del enfermero) implica una historia de negociación que debe diseñarse antes de HU-05.
- La pasarela de pagos integrada implica selección de proveedor, cumplimiento de normativa de medios de pago en Colombia y modelo de comisión/liquidación — pendiente de definición técnica y de negocio.
- La geolocalización real del enfermero cambia el modelo de datos de `Persona`/`Enfermero` respecto a lo descrito hoy en `Requirements-Context.md` §5.1 — requiere actualizar esa sección o documentarlo como decisión posterior explícita.
- Tratamiento de datos sensibles (salud) bajo Ley 1581 de 2012 — requiere base legal, consentimiento y aviso de privacidad antes de recolectar datos médicos.
- El dato biométrico facial es un dato sensible bajo la Ley 1581 de 2012 (nivel de protección igual o mayor al de los datos de salud) — requiere consentimiento explícito y separado, y almacenamiento seguro.
- **Conectividad obligatoria durante el servicio (sin modo offline):** ni el inicio ni el seguimiento del servicio contemplan hoy un escenario sin internet. Es una restricción de negocio aceptada, no un vacío pendiente de resolver — pero condiciona la operación en zonas de baja cobertura.
- **Proveedor de verificación facial y su costo aún no definidos.** No hay presupuesto asignado todavía para un servicio externo de verificación biométrica (usualmente con costo por transacción); queda como decisión pendiente antes de comprometer la arquitectura de esta funcionalidad.
- La recaptura periódica de la foto de referencia (cada 6-12 meses) es un proceso operativo nuevo que debe diseñarse (quién lo dispara, qué pasa si el enfermero no recaptura a tiempo).

## 8. Riesgos identificados

| Riesgo                                                                                                  | Impacto                                                                                                                                       | Mitigación propuesta                                                                                                                |
| ------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| Responsabilidad legal por suplantación de identidad de enfermeros                                       | Un enfermero suplantado presta un servicio clínico en el domicilio de un paciente; exposición legal y reputacional directa para la plataforma | Verificación de identidad (biometría/prueba de vida) y contraste de credenciales contra el ReTHUS antes de aprobar un perfil        |
| Cuello de botella: la aprobación manual no escala con el crecimiento                                    | El SLA de revisión se degrada rápidamente al crecer el volumen de registros, frenando el crecimiento de la oferta                             | Verificación asistida (ReTHUS automático) + más de un revisor + métricas de cola desde el diseño                                    |
| Sanciones por manejo indebido de datos de salud (Ley 1581)                                              | Multas, suspensión o cierre definitivo de la operación que trate datos sensibles sin base legal                                               | Consentimiento explícito, aviso de privacidad, minimización de datos y auditoría de acceso a información sensible                   |
| Reclasificación legal como prestador de salud (no intermediario)                                        | Podría exigir habilitación REPS y cambiar el modelo operativo completo                                                                        | Validar con concepto legal/regulatorio antes de escalar el producto (ver Restricciones)                                             |
| Baja liquidez del marketplace                                                                           | Pocos enfermeros o pocos usuarios activos impiden que el modelo de dos lados funcione                                                         | No cubierto en esta fase — a monitorear con las métricas de éxito definidas arriba                                                  |
| Dependencia total de conectividad e-tiempo real para iniciar/mantener un servicio                       | Sin internet, el enfermero no puede iniciar el servicio ni compartir ubicación; bloquea la operación en zonas de baja cobertura               | Definir política de excepción/soporte para estos casos (fuera de alcance de esta visión; a resolver en la especificación del flujo) |
| Tratamiento continuo de ubicación en tiempo real del paciente (dato sensible de una persona vulnerable) | Rastreo permanente de una persona en situación de necesidad de cuidado; exposición ante fuga o mal uso del dato                               | Consentimiento explícito, minimización (solo durante el servicio activo) y acceso restringido al contacto de emergencia designado   |

## 9. Preguntas abiertas

- [ ] ¿Cuál es el mecanismo exacto de negociación de precio (oferta del usuario → contraoferta del enfermero → ¿cuántas rondas? ¿quién tiene la última palabra?)?
- [ ] ¿Qué proveedor de pasarela de pagos se usará y quién retiene el dinero hasta la finalización del servicio (custodia/escrow)?
- [ ] ¿Quién y cuándo se encargará de validar formalmente la clasificación legal (intermediario vs. prestador de salud)?
- [ ] ¿Cuáles son los umbrales numéricos objetivo para las métricas de éxito (servicios/mes, tasa de aceptación, retención)?
- [ ] ¿Qué nivel de precisión de geolocalización se requiere (ciudad/barrio vs. coordenadas exactas) y con qué frecuencia se actualiza la ubicación del enfermero?
- [ ] ¿Qué proveedor de verificación facial/biométrica se usará y cuál es el presupuesto disponible por verificación?
- [ ] ¿Cuántos intentos adicionales puede habilitar el superadministrador tras los 2 iniciales en el registro, y bajo qué criterio decide cuántos?
- [ ] ¿Cómo se registra el "contacto de emergencia" del paciente/usuario? ¿Es obligatorio, se permite más de uno, y qué pasa si el paciente no tiene uno disponible?
- [ ] ¿Qué pasa si el paciente no tiene un dispositivo con GPS o no puede compartir su ubicación (ej. persona sin smartphone, adulto mayor sin manejo de tecnología, paciente inconsciente)?
- [ ] ¿Existe alguna tolerancia (ventana de reintento, modo degradado) ante la pérdida momentánea de conexión durante un servicio ya en curso, o se corta de inmediato?

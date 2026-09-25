# 2. Planteamiento de la problemática

## 2.1. Situación problemática

La London Fire Brigade responde a una demanda compuesta por incendios, servicios especiales y falsas alarmas, distribuida entre los distintos boroughs de Londres. Cada atención presenta características que influyen en la respuesta requerida y en los recursos utilizados. Por ello, evaluar el servicio exige considerar conjuntamente qué tipo de incidente se atiende, cuánto tardan los recursos en llegar y qué despliegue queda asociado a esa atención.

El problema de gestión consiste en determinar qué zonas y tipos de atención conviene revisar primero. Una meta promedio para todo Londres no describe las diferencias entre territorios, horarios y clases de incidente, ni muestra qué movilizaciones, minutos de autobomba y costo nocional corresponden a cada grupo de atenciones. El proyecto analizará conjuntamente estas diferencias para apoyar la identificación de zonas y tipos de atención que requieren una revisión prioritaria.

En la respuesta operativa, el análisis examinará los tiempos de salida, viaje y llegada, los motivos de demora reportados, como el tráfico o las obras viales, y la estación desde la que salió cada recurso. En los recursos, examinará las movilizaciones, las autobombas asistentes, los minutos de autobomba publicados con redondeo y el costo nocional de esas mismas atenciones. Para que las diferencias sean interpretables, se compararán incidentes de características semejantes, ya que un mayor tiempo o costo en un incendio complejo puede responder a las exigencias de la emergencia.

Dentro de esta comparación, las falsas alarmas constituyen un tipo de atención específico. Identificar en qué zonas y tipos de inmueble generan más atenciones y movilizaciones permitirá orientar la investigación de sus posibles causas y, según los resultados, evaluar acciones preventivas. Así, el análisis apoyará dos decisiones dentro del mismo proceso: dónde examinar dificultades de respuesta y dónde investigar concentraciones de falsas alarmas.

## 2.2. Pregunta central de negocio

> ¿Qué zonas y tipos de atención debería priorizar la London Fire Brigade para revisar su respuesta operativa y orientar acciones preventivas, considerando los tiempos de llegada, las demoras reportadas y los recursos utilizados durante 2025?

## 2.3. Objetivos

**Objetivo general del proyecto:** analizar la atención de incidentes de LFB durante 2025, considerando los tiempos de llegada, las demoras reportadas y los recursos utilizados, para fundamentar qué zonas y tipos de atención deberían priorizarse para revisión operativa y orientación de acciones preventivas.

**Objetivos específicos:**

1. Caracterizar las atenciones por zona, tipo de incidente, horario y tipo de inmueble.

2. Comparar los tiempos de respuesta y las demoras reportadas entre atenciones de características semejantes.

3. Examinar las movilizaciones, las autobombas asistentes, los minutos publicados con redondeo y el costo nocional asociados a esas atenciones.

4. Identificar concentraciones de falsas alarmas por zona y tipo de inmueble, distinguiendo sus categorías, para orientar la investigación de sus posibles causas.

5. Integrar esas comparaciones para sustentar la selección de zonas y tipos de atención que requieren revisión.

## 2.4. Alcance de la primera entrega

Esta primera entrega presenta la institución, desarrolla el marco teórico y plantea el problema que orientará el proyecto. También documenta las variables disponibles y propone el modelo multidimensional para estudiar las atenciones realizadas durante 2025. La determinación de zonas prioritarias y las propuestas sustentadas en resultados corresponden al análisis posterior.

| Aspecto | Delimitación |
|---|---|
| Institución | London Fire Brigade. |
| Periodo | Del 1 de enero al 31 de diciembre de 2025, mediante CalYear=2025 en ambas fuentes. |
| Proceso | Atención operativa de incidentes; tiempos desde movilización hasta llegada y recursos asociados a la intervención. |
| Tipos de atención | Incendios, falsas alarmas y servicios especiales. |
| Territorio | Borough como nivel principal; ward y distrito postal para detalle cuando corresponda. |
| Contexto | Hora de llamada y categoría y tipo de propiedad. |
| Nivel de detalle | Un incidente y una movilización, conservados en hechos distintos. |
| Decisión | Dónde concentrar la revisión operativa y dónde investigar posibles causas de falsas alarmas. |

La ubicación y el tipo de atención organizan la comparación. Los tiempos describen la respuesta; los recursos describen el esfuerzo asociado. Un mayor tiempo o costo no demuestra por sí solo ineficiencia. Los registros tampoco demuestran que una falsa alarma haya retrasado otra emergencia, que Other Station implique falta de unidades en la estación más cercana, ni que toda falsa alarma sea evitable. El costo nocional es una estimación y no un ahorro automáticamente recuperable.

## 2.5. Correspondencia entre el problema y la información

| Aspecto que se estudiará | Campos principales | Ubicación en el modelo |
|---|---|---|
| Zona y tipo de atención | IncGeo_BoroughName, IncGeo_WardName, IncidentGroup, StopCodeDescription | DimGeografia y DimTipoIncidente, compartidas por ambos hechos. |
| Contexto temporal y del inmueble | DateOfCall, HourOfCall, PropertyCategory, PropertyType | DimFecha, DimHora y DimTipoPropiedad. |
| Primera llegada al incidente | FirstPumpArriving_AttendanceTime | FactIncidente.PrimerArriboSeg. |
| Tiempos de cada unidad | TurnoutTimeSeconds, TravelTimeSeconds, AttendanceTimeSeconds | FactMovilizacion.SalidaSeg, ViajeSeg y LlegadaSeg. |
| Demoras reportadas y contexto de salida | DelayCodeId, DelayCode_Description, DeployedFromStation_Code, DeployedFromLocation | DimMotivoDemora, DimEstacion y DimOrigenDespliegue. |
| Recursos de la atención | ResourceMobilisationId, NumPumpsAttending, PumpMinutesRounded, Notional Cost (£) | Conteo de movilizaciones y medidas de FactIncidente. |
| Falsas alarmas | IncidentGroup=False Alarm y detalle en StopCodeDescription | Filtro por DimTipoIncidente; comparación por zona, propiedad y hora. |

IncidentNumber permite vincular el incidente con sus movilizaciones durante la preparación. Las medidas del incidente permanecen una sola vez, aunque participen varias unidades. Se compararán resultados agregados por las dimensiones comunes, conservando el significado de ambos niveles de detalle.

[Volver al informe principal](../../README.md) · [Índice de componentes](../README.md).

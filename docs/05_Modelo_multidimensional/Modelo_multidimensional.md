# 6. Modelo multidimensional

## 6.1. Decisiones de diseño desde la pregunta de negocio

La pregunta requiere estudiar la atención de un incidente y el despliegue de las unidades que participan. Son dos niveles de detalle del mismo proceso. Se proponen dos tablas de hechos porque una atención puede generar varias movilizaciones y cada nivel tiene medidas propias.

| Tabla de hechos | Qué representa una fila | Qué permite analizar |
|---|---|---|
| FactIncidente | Un incidente publicado con CalYear=2025, identificado por IncidentNumber. | Demanda atendida, primera y segunda llegada publicadas, autobombas, estaciones participantes, minutos redondeados, llamadas y costo nocional de la atención. |
| FactMovilizacion | Una movilización válida de un recurso con CalYear=2025, identificada por ResourceMobilisationId. | Tiempo de salida, viaje y llegada de cada unidad, estación de despliegue, contexto de salida y motivo de demora reportado. |

Las falsas alarmas se identifican por DimTipoIncidente.Grupo=False Alarm. No necesitan una tercera tabla de hechos: tienen el mismo grano que los incendios y servicios especiales. El costo nocional se mantiene solo en FactIncidente; repetirlo en cada movilización multiplicaría el costo del incidente.

IncidentNumber relaciona las fuentes durante la preparación y conserva la trazabilidad. En el modelo analítico los hechos se filtran por dimensiones compartidas y sus resultados se agregan por separado. Este diseño respeta el [grano de cada hecho](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/grain/) y evita [uniones entre hechos que multipliquen registros](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/multipass-sql/).

## 6.2. Ocho dimensiones y su utilidad

| Dimensión | Atributos principales | Utilidad para el problema | Hechos |
|---|---|---|---|
| DimFecha | Año, trimestre, mes, día, día de semana | Ubicar periodos de concentración de la demanda | Ambos |
| DimHora | Hora y franja | Distinguir el contexto horario de las atenciones | Ambos |
| DimTipoIncidente | Grupo, clasificación y servicio especial | Distinguir incendios, falsas alarmas y servicios especiales antes de comparar su atención | Ambos |
| DimTipoPropiedad | Categoría y tipo | Contextualizar atenciones e identificar concentraciones de falsas alarmas por propiedad | Ambos |
| DimGeografia | Borough, ward y distrito postal | Delimitar segmentos territoriales | Ambos |
| DimEstacion | Código y nombre | Reconocer estaciones que participan en el despliegue | Movilización |
| DimOrigenDespliegue | Home Station, Other Station, desconocido | Distinguir el contexto de salida del recurso | Movilización |
| DimMotivoDemora | Código y descripción | Contextualizar el desplazamiento; no explicar la activación de la alarma | Movilización |

La dimensión fecha se cuenta una vez; año, mes y día son atributos. El origen del despliegue conserva las categorías Home Station y Other Station, distintas del código y nombre de estación; no identifica la disponibilidad de la estación más cercana. El motivo de demora no se interpreta como causa de falsa alarma.

## 6.3. Relaciones y claves

Cada dimensión tiene una clave primaria (PK), y el hecho correspondiente almacena una clave foránea (FK). La cardinalidad es **1:N**: una combinación dimensional puede aparecer en muchos hechos. FactIncidente tiene cinco relaciones dimensionales; FactMovilizacion tiene ocho. Las primeras cinco son conformadas: comparten la misma definición en ambos hechos.

Las claves de fecha y hora se generan de forma determinista. Las demás dimensiones usan claves sustitutas que identifican sus combinaciones descriptivas. El miembro 0 representa contexto desconocido; no es un evento inventado. Una movilización sin incidente enlazado conserva su registro y sus dimensiones propias, pero no se le atribuye una falsa alarma ni un territorio a partir de suposiciones.

## 6.4. Medidas del diseño

En FactIncidente se conservan la cantidad de bombas asistentes, estaciones participantes, llamadas, minutos de bomba redondeados, costo nocional y tiempos publicados de primer y segundo arribo. La cantidad de incidentes se obtiene contando sus filas; TieneMovilizacion identifica si existe al menos una movilización válida enlazada. En FactMovilizacion se conservan los tiempos de salida, viaje y llegada, y la cantidad de movilizaciones se obtiene contando filas válidas.

Estas son medidas del modelo, no resultados ni metas de desempeño. Los tiempos de arribo y desplazamiento permiten estudiar la respuesta, y los recursos y el costo nocional describen el esfuerzo asociado. Su coincidencia no demostrará que las falsas alarmas causaron retrasos en otras emergencias.

El costo y las llamadas se agregan únicamente desde FactIncidente. Sumar NumBombas representa participaciones en incidentes, no vehículos únicos. Los tiempos de recursos no equivalen a duración total del incidente. Los filtros de estación, origen y demora corresponden a movilizaciones; no deben asignar artificialmente costos de incidentes a estaciones.

## 6.5. Jerarquías

- Fecha: año → trimestre → mes → día. Día de semana es un atributo independiente.

- Propiedad: categoría → tipo.

- Geografía: borough → ward, validando correspondencia. Distrito postal es un filtro alternativo, no un nivel necesariamente contenido en ward.

- Incidente: grupo → clasificación. Servicio especial solo aporta detalle cuando corresponde.

- Hora: franja → hora. Las franjas son agrupaciones académicas y no turnos oficiales de LFB.

## 6.6. Condiciones para una interpretación válida

Las copias idénticas de movilizaciones se deduplicarán y los identificadores con versiones contradictorias se revisarán antes de cargar. Los valores desconocidos permanecerán distinguibles de las categorías reales. Las dimensiones comunes de la movilización procederán del incidente enlazado. No se duplicarán medidas del incidente al integrar recursos.

Estas reglas resguardan la lectura del problema; su ejecución técnica detallada permanece en el soporte interno. La primera entrega presenta el modelo y sus definiciones, sin incluir una implementación de base de datos.

## 6.7. Agregación y comparación entre hechos

| Medida | Agregación e interpretación |
|---|---|
| Incidentes | Conteo de filas únicas de FactIncidente. |
| Movilizaciones | Conteo de filas válidas de FactMovilizacion; no es el número de incidentes ni necesariamente el de autobombas que llegaron. |
| NumBombas y NumEstaciones | Su suma representa participaciones en atenciones; no unidades o estaciones únicas en todo el periodo. |
| MinutosBombaRedondeados y CostoNocionalGBP | Suma exclusivamente desde FactIncidente, conservando el redondeo y la estimación del proveedor. |
| PrimerArriboSeg y SegundoArriboSeg | Promedio, mediana o distribución sobre incidentes con valor informado; no sumar como duración del servicio. |
| SalidaSeg, ViajeSeg y LlegadaSeg | Promedio, mediana o distribución sobre movilizaciones con valor informado. No sumar los tres campos como si fueran etapas independientes. |
| NumLlamadas | Suma a nivel incidente; no equivale al número de incidentes. |

Para combinar información por borough y tipo de incidente se agregará cada hecho por separado y luego se alinearán sus resultados por las dimensiones compartidas. No se promediarán promedios de subgrupos sin ponderar por su número de valores válidos. La cantidad de observaciones y de datos ausentes acompañará las comparaciones de tiempos.

FechaKey y HoraKey de FactMovilizacion corresponden a la fecha y hora de llamada del incidente enlazado, para mantener coherencia con FactIncidente; FechaHoraMovilizada conserva la marca de movilización como atributo independiente. Cuando el incidente no pueda enlazarse, las cinco claves compartidas tendrán valor 0 y TieneIncidente=0. La estación, el origen y el motivo de demora se conservarán desde la movilización.

Los tiempos publicados pueden estar sujetos a reglas de reporte. PerformanceReporting se conserva para documentar el universo seleccionado; una comparación con las metas oficiales requeriría reproducir sus criterios de inclusión. No se declarará incumplimiento individual por superar seis minutos ni se interpretará una demora ausente como ausencia de tráfico.

## 6.8. Diagramas y especificación completa

![Incidentes y cinco dimensiones compartidas](../05_Modelo_multidimensional/FactIncidente.png)

![Movilizaciones y ocho dimensiones](../05_Modelo_multidimensional/FactMovilizacion.png)

Los diagramas muestran las claves, relaciones 1:N y grupos de medidas. La especificación de todos los campos está en el [diccionario del modelo](../04_Diccionario_datos/Diccionario_modelo.md). El [modelo Mermaid editable](../05_Modelo_multidimensional/Modelo_completo.mmd) conserva todos los atributos.

## 6.9. Cómo se integran las comparaciones

| Uso del análisis | Universo y regla |
|---|---|
| Describir toda la demanda de 2025 | FactIncidente completo, incluidos incidentes sin movilización enlazada en la descarga. |
| Comparar tiempos y recursos de las mismas atenciones | FactIncidente con TieneMovilizacion=1 y FactMovilizacion con TieneIncidente=1. Ambos controles se calculan después de depurar las movilizaciones. |
| Describir movilizaciones sin incidente enlazado | Mantenerlas identificadas para revisión de cobertura. No atribuirles zona, propiedad ni tipo de incidente sin respaldo. |

Cada hecho se agrega por las mismas claves de fecha, hora, geografía, tipo de incidente y propiedad, y luego se alinean los resultados. La comparación conjunta usa el mismo conjunto de IncidentNumber; si se decide mostrar la demanda total, se identifica expresamente la diferencia de cobertura. Un incidente sin movilización enlazada no equivale a un incidente sin atención.

Los filtros de estación, origen y motivo de demora corresponden a FactMovilizacion. Si se filtra una estación, los costos de FactIncidente no se convierten en costos de esa estación. El modelo no distribuye costos entre unidades ni promete recuperar el presupuesto estimado de falsas alarmas.

## 6.10. Qué se conserva en la fuente y qué se utiliza en el modelo

LlegadaSeg representa AttendanceTimeSeconds y describe el tiempo hasta la llegada de cada unidad. TieneMovilizacion es un control derivado en FactIncidente para delimitar comparaciones enlazadas, junto con TieneIncidente en FactMovilizacion.

La clasificación de falsa alarma permanece en DimTipoIncidente. PumpOrder y los dos campos de tipo de movilización permanecen en las fuentes: el primero tiene una definición insuficiente para inferir el orden de llegada y los otros dos no distinguen grupos en el corte de 2025. El retorno a estación no se modela como medida de duración por su falta de cobertura.

El número de incidentes y el de movilizaciones se obtienen contando filas únicas de sus respectivos hechos. Los tiempos se resumen sobre valores válidos, conservando el número de observaciones. LlegadaSeg ya incluye el intervalo de salida y viaje; no se suman los tres tiempos como si fueran etapas distintas.


[Volver al informe principal](../../README.md) · [Índice de componentes](../README.md).

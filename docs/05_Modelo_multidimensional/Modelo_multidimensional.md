# 6. Modelo multidimensional

## 6.1. Tablas de hechos

Diseñamos el modelo con dos tablas de hechos: una para cada incidente y otra para cada movilización. Un incidente puede requerir varias unidades, por lo que sus recursos y costos deben distinguirse de los tiempos de cada unidad movilizada.

| Tabla de hechos | Qué representa una fila | Qué permite analizar |
|---|---|---|
| FactIncidente | Un incidente publicado con CalYear=2025, identificado por IncidentNumber. | Demanda atendida, primera y segunda llegada publicadas, autobombas, estaciones participantes, minutos redondeados, llamadas y costo nocional de la atención. |
| FactMovilizacion | Una movilización válida de un recurso con CalYear=2025, identificada por ResourceMobilisationId. | Tiempo de salida, viaje y llegada de cada unidad, estación de despliegue, contexto de salida y motivo de demora reportado. |

Las falsas alarmas se identifican mediante DimTipoIncidente.Grupo=False Alarm, al mismo nivel de detalle que los incendios y servicios especiales. El costo nocional se mantiene solo en FactIncidente; repetirlo en cada movilización multiplicaría el costo del incidente.

IncidentNumber relaciona las fuentes durante la preparación. Las tablas de hechos no se conectan directamente entre sí; comparten dimensiones.

## 6.2. Dimensiones

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

El origen del despliegue conserva las categorías Home Station y Other Station, distintas del código y nombre de estación; no identifica la disponibilidad de la estación más cercana.

## 6.3. Relaciones y claves

Cada dimensión tiene una clave primaria (PK), y el hecho correspondiente almacena una clave foránea (FK). La cardinalidad es **1:N**: una combinación dimensional puede aparecer en muchos hechos. FactIncidente tiene cinco relaciones dimensionales; FactMovilizacion tiene ocho. Las primeras cinco son conformadas: comparten la misma definición en ambos hechos.

FechaKey y HoraKey se calculan a partir de la fecha y la hora de llamada. Para las claves primarias de las otras seis dimensiones usaremos IDENTITY(1,1). Reservamos el valor 0 para desconocidos y lo cargaremos explícitamente mediante IDENTITY_INSERT. Las claves foráneas reutilizan el identificador de su dimensión; no generan uno nuevo. FactIncidente y FactMovilizacion conservan los identificadores originales de la fuente, sin IDENTITY.

## 6.4. Jerarquías

- Fecha: año → trimestre → mes → día. Día de semana es un atributo independiente.

- Propiedad: categoría → tipo.

- Geografía: borough → ward, validando correspondencia. Distrito postal es un filtro alternativo, no un nivel necesariamente contenido en ward.

- Incidente: grupo → clasificación. Servicio especial solo aporta detalle cuando corresponde.

- Hora: franja → hora. Definimos las franjas para el análisis; no representan turnos oficiales de LFB.

## 6.5. Preparación de los datos

Eliminaremos las copias idénticas de movilizaciones y separaremos los identificadores con versiones contradictorias para revisarlos antes de la carga. Mantendremos los valores desconocidos separados de las categorías registradas. Obtendremos las dimensiones compartidas del incidente enlazado, sin repetir sus medidas por cada recurso movilizado.

## 6.6. Agregación y comparación entre hechos

| Medida | Agregación e interpretación |
|---|---|
| Incidentes | Conteo de filas únicas de FactIncidente. |
| Movilizaciones | Conteo de filas válidas de FactMovilizacion; no es el número de incidentes ni necesariamente el de autobombas que llegaron. |
| NumBombas y NumEstaciones | Su suma representa participaciones en atenciones; no unidades o estaciones únicas en todo el periodo. |
| MinutosBombaRedondeados y CostoNocionalGBP | Suma exclusivamente desde FactIncidente, conservando el redondeo y la estimación del proveedor. |
| PrimerArriboSeg y SegundoArriboSeg | Promedio, mediana o distribución sobre incidentes con valor informado; no sumar como duración del servicio. |
| SalidaSeg, ViajeSeg y LlegadaSeg | Promedio, mediana o distribución sobre movilizaciones con valor informado. No sumar los tres campos como si fueran etapas independientes. |
| NumLlamadas | Suma a nivel incidente; no equivale al número de incidentes. |

Agregaremos cada tabla de hechos por separado y compararemos sus resultados por las dimensiones compartidas. Si combinamos promedios de subgrupos, los ponderaremos por su cantidad de valores válidos. En las comparaciones de tiempos indicaremos también cuántas observaciones tienen datos y cuántas presentan valores ausentes.

FechaKey y HoraKey de FactMovilizacion corresponden a la fecha y hora de llamada del incidente enlazado, para mantener coherencia con FactIncidente; FechaHoraMovilizada conserva la marca de movilización como atributo independiente. Cuando el incidente no pueda enlazarse, las cinco claves compartidas tendrán valor 0 y TieneIncidente=0. La estación, el origen y el motivo de demora se conservarán desde la movilización.

Los tiempos publicados pueden estar sujetos a reglas de reporte. PerformanceReporting se conserva para documentar el universo seleccionado; una comparación con las metas oficiales requeriría reproducir sus criterios de inclusión. No se declarará incumplimiento individual por superar seis minutos ni se interpretará una demora ausente como ausencia de tráfico.

## 6.7. Diagramas

![Incidentes y cinco dimensiones compartidas](../05_Modelo_multidimensional/FactIncidente.png)

![Movilizaciones y ocho dimensiones](../05_Modelo_multidimensional/FactMovilizacion.png)

Los diagramas muestran los campos, las claves y las relaciones 1:N. La especificación de todos los campos está en el [diccionario del modelo](../04_Diccionario_datos/Diccionario_modelo.md).

## 6.8. Población de análisis

| Uso del análisis | Universo y regla |
|---|---|
| Describir toda la demanda de 2025 | FactIncidente completo, incluidos incidentes sin movilización enlazada en la descarga. |
| Comparar tiempos y recursos de las mismas atenciones | FactIncidente con TieneMovilizacion=1 y FactMovilizacion con TieneIncidente=1. Ambos controles se calculan después de depurar las movilizaciones. |
| Describir movilizaciones sin incidente enlazado | Mantenerlas identificadas para revisión de cobertura. No atribuirles zona, propiedad ni tipo de incidente sin respaldo. |

La comparación conjunta usa el mismo conjunto de IncidentNumber. Un incidente sin movilización enlazada en la descarga no equivale a un incidente sin atención.

Los filtros de estación, origen y demora se aplican a FactMovilizacion. El costo nocional pertenece al incidente y no se reparte entre estaciones ni unidades. Es una estimación, no una pérdida presupuestaria ni un ahorro recuperable. Los registros tampoco permiten demostrar que una falsa alarma haya retrasado otra emergencia.

## 6.9. Selección de campos

Conservamos PumpOrder, PlusCode_Code y PlusCode_Description en las fuentes, sin incorporarlos al modelo. El primero no define con precisión el orden de llegada; los otros dos solo distinguen Initial / Initial Mobilisation en 2025. PumpCount queda fuera de las medidas por falta de definición suficiente. El tiempo de retorno tampoco se usa por su escasa cobertura.

[Volver al informe principal](../../README.md) · [Documentos](../README.md).

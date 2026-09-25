# Tiempos de respuesta y uso de recursos en la atención de incidentes de London Fire Brigade

**Integrantes:**

- Tania Lisset Chavez Alvitez
- Brayan Anderson Rua Pomahuacre
- Alyssa Antuanette Trujillo Cruzado
- Thiago Cesar Ormeño Freundt

## 1. Introducción

Los servicios públicos de respuesta inmediata deben organizar sus recursos para atender situaciones de distinta naturaleza y gravedad. En los servicios de bomberos, esto exige considerar tanto la rapidez de la respuesta como las unidades y el tiempo necesarios para intervenir.

En el Reino Unido, estos servicios atienden incendios y otros tipos de incidentes. Las estadísticas oficiales de Inglaterra muestran que las falsas alarmas representaron más de un tercio de las atenciones en el año terminado en marzo de 2025 (MHCLG, 2025).

En Londres, la London Fire Brigade (LFB) atiende incendios, servicios especiales y falsas alarmas. Su proceso de atención comienza con la llamada de emergencia y continúa con la movilización de unidades, el desplazamiento, la intervención y el retorno a la estación. Estudiaremos los tiempos desde la movilización hasta la llegada y los recursos empleados en cada atención.

LFB tiene como meta que la primera autobomba llegue en un promedio de seis minutos y la segunda en ocho, contados desde la movilización. Son promedios para todo Londres, no plazos garantizados para cada incidente (Greater London Authority, 2025; LFB, 2024). Entre 2017 y 2025, las atenciones aumentaron cerca de un tercio y el tiempo promedio de primera llegada alcanzó 329 segundos en 2025 (London Assembly Research Unit, 2026). Aunque este valor se mantiene dentro de la meta, no muestra las diferencias entre zonas y tipos de atención.

Cada incidente requiere un despliegue distinto. LFB registra las unidades participantes, los minutos de autobomba y un costo nocional que estima el esfuerzo de la atención mediante una tarifa estándar (LFB, 2022). Las falsas alarmas también utilizan estos recursos, por lo que interesa conocer dónde se concentran y qué tipos de inmueble están asociados a ellas.

Analizaremos las atenciones de 2025 para identificar qué zonas y tipos de incidentes requieren una revisión operativa y dónde conviene investigar las causas de las falsas alarmas. Utilizaremos los registros de incidentes y movilizaciones publicados por LFB en London Datastore (LFB, 2026a, 2026b).

## 2. Planteamiento de la problemática

### 2.1. Situación problemática

El promedio de llegada de LFB para todo Londres no permite distinguir qué zonas y tipos de atención presentan mayores tiempos ni qué recursos se utilizan en cada caso. El problema que abordamos es cómo priorizar su revisión considerando los tiempos de respuesta, las demoras reportadas y el despliegue asociado a las mismas atenciones.

Compararemos los tiempos de salida, viaje y llegada, los motivos de demora y los recursos empleados entre incidentes de características semejantes. Así evitaremos interpretar el mayor despliegue de una emergencia compleja como ineficiencia. En el caso de las falsas alarmas, examinaremos su concentración por zona y tipo de inmueble para identificar dónde conviene investigar sus causas y orientar acciones preventivas.

### 2.2. Pregunta central de negocio

¿Qué zonas y tipos de atención debería priorizar la London Fire Brigade para revisar su respuesta operativa y orientar acciones preventivas, considerando los tiempos de llegada, las demoras reportadas y los recursos utilizados durante 2025?

### 2.3. Objetivos

**Objetivo general:** analizar la atención de incidentes de LFB durante 2025, considerando los tiempos de llegada, las demoras reportadas y los recursos utilizados, para fundamentar qué zonas y tipos de atención deberían priorizarse para revisión operativa y orientación de acciones preventivas.

**Objetivos específicos:**

1. Caracterizar las atenciones por zona, tipo de incidente, horario y tipo de inmueble.

2. Comparar los tiempos de respuesta y las demoras reportadas entre atenciones de características semejantes.

3. Examinar las movilizaciones, las autobombas asistentes, los minutos publicados con redondeo y el costo nocional asociados a esas atenciones.

4. Identificar concentraciones de falsas alarmas por zona y tipo de inmueble, distinguiendo sus categorías, para orientar la investigación de sus posibles causas.

### 2.4. Especificaciones generales

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

## 3. Descripción de la institución

### 3.1. Institución y finalidad

London Fire Brigade (LFB) es el servicio de bomberos y rescate de Londres. Combina la respuesta a incidentes con actividades de prevención y protección de la comunidad. Su plan 2023–2029 plantea reducir y responder al riesgo en Londres. [Plan institucional de LFB](https://www.london-fire.gov.uk/about-us/your-london-fire-brigade-our-plan-for-2023-29/).

### 3.2. Proceso de atención

| Etapa del proceso general | Relación con el proyecto |
|---|---|
| Recepción del aviso | Fecha, hora, lugar y clasificación publicada permiten describir la demanda. |
| Movilización de unidades | Identificaremos cada recurso movilizado, la estación y el origen de su despliegue. |
| Salida, desplazamiento y llegada | Compararemos los tiempos en segundos y los motivos de demora reportados. |
| Intervención | Examinaremos las autobombas asistentes, los minutos publicados con redondeo y el costo nocional del incidente. |
| Retorno de unidades | Forma parte del proceso general, pero queda fuera del análisis de duración y disponibilidad por insuficiencia del campo de retorno. |

### 3.3. Fuentes de datos

Utilizaremos los registros abiertos oficiales de [incidentes](https://data.london.gov.uk/dataset/london-fire-brigade-incident-records-em8xy) y [movilizaciones](https://data.london.gov.uk/dataset/london-fire-brigade-mobilisation-records-24r65), publicados por LFB en London Datastore. Trabajaremos con la descarga del 24 de septiembre de 2026 y seleccionaremos el año 2025, que está completo en ambos archivos.

La fuente de incidentes describe cada atención; la de movilizaciones, cada recurso desplegado. Las relacionaremos mediante IncidentNumber.

## 4. Marco teórico

### 4.1. Business Intelligence como apoyo a la gestión

Business Intelligence reúne y organiza datos para apoyar la toma de decisiones. Aplicaremos este enfoque para comparar los tiempos de respuesta y los recursos utilizados por LFB según la zona y el tipo de incidente.

### 4.2. Hechos y nivel de detalle

Un hecho representa un evento medible de un proceso. El nivel de detalle o grano define qué representa cada fila de una tabla de hechos. En nuestro caso distinguimos el incidente atendido y la movilización de un recurso: un mismo incidente puede generar varias movilizaciones, cada una con sus propios tiempos.

### 4.3. Dimensiones y modelo multidimensional

Las dimensiones describen las características de un hecho, como su fecha, ubicación o tipo de incidente. Un esquema estrella relaciona una tabla de hechos con sus dimensiones. Cuando varias tablas de hechos comparten dimensiones, forman una constelación de estrellas.

Las dimensiones compartidas usan las mismas claves y definiciones. Esto permite comparar resultados de incidentes y movilizaciones por fecha, hora, zona, tipo de incidente y tipo de propiedad.

### 4.4. Datamart y análisis multidimensional

Un datamart reúne datos de un ámbito de la institución; el nuestro se centra en la atención de incidentes. OLTP se orienta al registro de transacciones y OLAP a su análisis. Las jerarquías permiten pasar de un nivel general a uno más detallado, por ejemplo, de año a mes o de borough a ward.

### 4.5. Integración y significado de los datos

El proceso ETL —extracción, transformación y carga— prepara los datos para el análisis. En este caso requiere relacionar incidentes y movilizaciones, uniformizar formatos, revisar duplicados y distinguir los valores ausentes de los valores registrados.

### 4.6. Conceptos operativos

| Concepto | Significado en el proyecto |
|---|---|
| Tiempo de llegada | Intervalo desde la movilización hasta la llegada del recurso; no incluye por sí solo todo el tiempo desde la llamada. |
| Primer arribo | Llegada de la primera autobomba al incidente; se distingue del tiempo de cada recurso que participa. |
| Demora reportada | Motivo registrado para contextualizar la movilización; no expresa cuántos segundos se perdieron exclusivamente por ese motivo. |
| Falsa alarma | Clasificación final de un aviso atendido como posible incendio; se identifica por IncidentGroup, sin inferirla a partir del costo o el tiempo. |
| Costo nocional | Valoración estimada del tiempo de autobombas según una tarifa estándar; no corresponde a una pérdida presupuestaria demostrada. |

Las definiciones de tiempos y costo se apoyan en los metadatos y en las aclaraciones de LFB de [2024](https://www.london-fire.gov.uk/media/8863/foia84201-response-times-of-fire-brigades-and-data-collation-response.pdf) y [2022](https://www.london-fire.gov.uk/media/6796/foi-response-66361.pdf). La clasificación de falsas alarmas se interpreta con las [definiciones oficiales de Inglaterra](https://www.gov.uk/government/statistics/fire-and-rescue-incident-statistics-year-ending-march-2025/fire-and-rescue-incident-statistics-year-ending-march-2025).

## 5. Diccionario de datos

La fuente de incidentes contiene 39 campos y la de movilizaciones, 24. Los diccionarios conservan los nombres originales y describen su uso en el modelo.

| Documento | Contenido |
|---|---|
| [Diccionario de fuentes](docs/04_Diccionario_datos/Diccionario_fuentes.md) | Los 63 campos: significado, unidad, uso en el modelo o conservación como respaldo. |
| [Diccionario de fuentes en CSV](docs/04_Diccionario_datos/Diccionario_fuentes.csv) | Versión tabular con las definiciones y notas originales del proveedor. |
| [Diccionario del modelo](docs/04_Diccionario_datos/Diccionario_modelo.md) | Todos los campos de las ocho dimensiones y los dos hechos: tipo de dato, rol, definición y transformación. |

Los [metadatos oficiales de incidentes](docs/04_Diccionario_datos/metadatos/diccionario_incidentes.xlsx) y [movilizaciones](docs/04_Diccionario_datos/metadatos/diccionario_movilizaciones.xlsx) se incluyen sin modificaciones.

## 6. Modelo multidimensional

### 6.1. Tablas de hechos

Diseñamos el modelo con dos tablas de hechos: una para cada incidente y otra para cada movilización. Un incidente puede requerir varias unidades, por lo que sus recursos y costos deben distinguirse de los tiempos de cada unidad movilizada.

| Tabla de hechos | Qué representa una fila | Qué permite analizar |
|---|---|---|
| FactIncidente | Un incidente publicado con CalYear=2025, identificado por IncidentNumber. | Demanda atendida, primera y segunda llegada publicadas, autobombas, estaciones participantes, minutos redondeados, llamadas y costo nocional de la atención. |
| FactMovilizacion | Una movilización válida de un recurso con CalYear=2025, identificada por ResourceMobilisationId. | Tiempo de salida, viaje y llegada de cada unidad, estación de despliegue, contexto de salida y motivo de demora reportado. |

Las falsas alarmas se identifican mediante DimTipoIncidente.Grupo=False Alarm, al mismo nivel de detalle que los incendios y servicios especiales. El costo nocional se mantiene solo en FactIncidente; repetirlo en cada movilización multiplicaría el costo del incidente.

IncidentNumber relaciona las fuentes durante la preparación. Las tablas de hechos no se conectan directamente entre sí; comparten dimensiones.

### 6.2. Dimensiones

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

### 6.3. Relaciones y claves

Cada dimensión tiene una clave primaria (PK), y el hecho correspondiente almacena una clave foránea (FK). La cardinalidad es **1:N**: una combinación dimensional puede aparecer en muchos hechos. FactIncidente tiene cinco relaciones dimensionales; FactMovilizacion tiene ocho. Las primeras cinco son conformadas: comparten la misma definición en ambos hechos.

FechaKey y HoraKey se calculan a partir de la fecha y la hora de llamada. Para las claves primarias de las otras seis dimensiones usaremos IDENTITY(1,1). Reservamos el valor 0 para desconocidos y lo cargaremos explícitamente mediante IDENTITY_INSERT. Las claves foráneas reutilizan el identificador de su dimensión; no generan uno nuevo. FactIncidente y FactMovilizacion conservan los identificadores originales de la fuente, sin IDENTITY.

### 6.4. Jerarquías

- Fecha: año → trimestre → mes → día. Día de semana es un atributo independiente.

- Propiedad: categoría → tipo.

- Geografía: borough → ward, validando correspondencia. Distrito postal es un filtro alternativo, no un nivel necesariamente contenido en ward.

- Incidente: grupo → clasificación. Servicio especial solo aporta detalle cuando corresponde.

- Hora: franja → hora. Definimos las franjas para el análisis; no representan turnos oficiales de LFB.

### 6.5. Preparación de los datos

Eliminaremos las copias idénticas de movilizaciones y separaremos los identificadores con versiones contradictorias para revisarlos antes de la carga. Mantendremos los valores desconocidos separados de las categorías registradas. Obtendremos las dimensiones compartidas del incidente enlazado, sin repetir sus medidas por cada recurso movilizado.

### 6.6. Agregación y comparación entre hechos

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

### 6.7. Diagramas

![Incidentes y cinco dimensiones compartidas](docs/05_Modelo_multidimensional/FactIncidente.png)

![Movilizaciones y ocho dimensiones](docs/05_Modelo_multidimensional/FactMovilizacion.png)

Los diagramas muestran los campos, las claves y las relaciones 1:N. La especificación de todos los campos está en el [diccionario del modelo](docs/04_Diccionario_datos/Diccionario_modelo.md).

### 6.8. Población de análisis

| Uso del análisis | Universo y regla |
|---|---|
| Describir toda la demanda de 2025 | FactIncidente completo, incluidos incidentes sin movilización enlazada en la descarga. |
| Comparar tiempos y recursos de las mismas atenciones | FactIncidente con TieneMovilizacion=1 y FactMovilizacion con TieneIncidente=1. Ambos controles se calculan después de depurar las movilizaciones. |
| Describir movilizaciones sin incidente enlazado | Mantenerlas identificadas para revisión de cobertura. No atribuirles zona, propiedad ni tipo de incidente sin respaldo. |

La comparación conjunta usa el mismo conjunto de IncidentNumber. Un incidente sin movilización enlazada en la descarga no equivale a un incidente sin atención.

Los filtros de estación, origen y demora se aplican a FactMovilizacion. El costo nocional pertenece al incidente y no se reparte entre estaciones ni unidades. Es una estimación, no una pérdida presupuestaria ni un ahorro recuperable. Los registros tampoco permiten demostrar que una falsa alarma haya retrasado otra emergencia.

### 6.9. Selección de campos

Conservamos PumpOrder, PlusCode_Code y PlusCode_Description en las fuentes, sin incorporarlos al modelo. El primero no define con precisión el orden de llegada; los otros dos solo distinguen Initial / Initial Mobilisation en 2025. PumpCount queda fuera de las medidas por falta de definición suficiente. El tiempo de retorno tampoco se usa por su escasa cobertura.

### 6.10. Diccionario del modelo

<details>
<summary>Campos de las dimensiones y tablas de hechos</summary>

#### DimFecha

Un día calendario; 365 días de 2025 y miembro desconocido.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| FechaKey | int | PK | YYYYMMDD desde DateOfCall; 0 desconocido. |
| Fecha | date | Atributo | DateOfCall sin componente horario; NULL solo para miembro 0. |
| Anio | smallint | Atributo | Año calendario de Fecha. |
| Trimestre | tinyint | Atributo | Trimestre calendario 1–4. |
| Mes | tinyint | Atributo | Mes 1–12. |
| Dia | tinyint | Atributo | Día del mes 1–31. |
| DiaSemana | tinyint | Atributo | Lunes=1 ... domingo=7; cálculo independiente del idioma de SQL Server. |

#### DimHora

Hora de llamada publicada; no equivale a la hora de movilización.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| HoraKey | int | PK | HourOfCall + 1; 1–24 representan 0–23; 0 desconocido. |
| Hora | tinyint | Atributo | HourOfCall, 0–23. |
| Franja | nvarchar(30) | Atributo | Agrupación horaria: madrugada 0–5, mañana 6–11, tarde 12–17, noche 18–23; no son turnos oficiales. |

#### DimTipoIncidente

Una combinación de grupo, clasificación y servicio especial.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| TipoIncidenteKey | int | PK | Clave sustituta generada mediante IDENTITY(1,1); fila 0 reservada para desconocido. No procede de la fuente. |
| Grupo | nvarchar(100) | Atributo | IncidentGroup: Fire, False Alarm o Special Service; desconocido si vacío. |
| Clasificacion | nvarchar(200) | Atributo | StopCodeDescription; categoría detallada publicada. |
| ServicioEspecial | nvarchar(200) | Atributo | SpecialServiceType; No aplica fuera de Special Service, Desconocido cuando falta dentro de ese grupo. |

#### DimTipoPropiedad

Una combinación de categoría y tipo de inmueble.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| TipoPropiedadKey | int | PK | Clave sustituta generada mediante IDENTITY(1,1); fila 0 reservada para desconocido. No procede de la fuente. |
| Categoria | nvarchar(100) | Atributo | PropertyCategory. |
| Tipo | nvarchar(200) | Atributo | PropertyType. No identifica una dirección individual. |

#### DimGeografia

Una combinación de borough, ward y distrito postal observados en incidentes.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| GeografiaKey | int | PK | Clave sustituta generada mediante IDENTITY(1,1); fila 0 reservada para desconocido. No procede de la fuente. |
| BoroughCodigo | nvarchar(30) | Atributo | IncGeo_BoroughCode. |
| BoroughNombre | nvarchar(120) | Atributo | IncGeo_BoroughName. |
| WardCodigo | nvarchar(30) | Atributo | IncGeo_WardCode. |
| WardNombre | nvarchar(150) | Atributo | IncGeo_WardName. |
| DistritoPostal | nvarchar(30) | Atributo | Postcode_district; atributo de filtro, no nivel debajo de ward. |

#### DimEstacion

Una estación de despliegue identificada por código.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| EstacionKey | int | PK | Clave sustituta generada mediante IDENTITY(1,1); fila 0 reservada para desconocido. No procede de la fuente. |
| Codigo | nvarchar(30) | Atributo | DeployedFromStation_Code; clave de negocio. |
| Nombre | nvarchar(150) | Atributo | DeployedFromStation_Name. No equivale a IncidentStationGround. |

#### DimOrigenDespliegue

Situación del recurso al desplegarse.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| OrigenDespliegueKey | int | PK | Clave sustituta generada mediante IDENTITY(1,1); fila 0 reservada para desconocido. No procede de la fuente. |
| Origen | nvarchar(50) | Atributo | DeployedFromLocation: Home Station, Other Station o Desconocido. No es una coordenada ni una estación adicional. |

#### DimMotivoDemora

Un código de motivo reportado.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| MotivoDemoraKey | int | PK | Clave sustituta generada mediante IDENTITY(1,1); fila 0 reservada para desconocido. No procede de la fuente. |
| Codigo | nvarchar(30) | Atributo | DelayCodeId; conservar como texto. |
| Descripcion | nvarchar(200) | Atributo | DelayCode_Description. Not held up es ausencia explícita de demora; vacío es Desconocido. |

#### FactIncidente

Una fila por IncidentNumber en CalYear=2025.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| IncidentNumber | varchar(30) | PK | Identificador publicado, preservando ceros y guiones; no se normaliza su estructura. |
| FechaKey | int | FK DimFecha | Lookup en DimFecha; 0 cuando no puede resolverse. NOT NULL. |
| HoraKey | int | FK DimHora | Lookup en DimHora; 0 cuando no puede resolverse. NOT NULL. |
| TipoIncidenteKey | int | FK DimTipoIncidente | Lookup en DimTipoIncidente; 0 cuando no puede resolverse. NOT NULL. |
| TipoPropiedadKey | int | FK DimTipoPropiedad | Lookup en DimTipoPropiedad; 0 cuando no puede resolverse. NOT NULL. |
| GeografiaKey | int | FK DimGeografia | Lookup en DimGeografia; 0 cuando no puede resolverse. NOT NULL. |
| PrimerArriboSeg | int | Medida | FirstPumpArriving_AttendanceTime; tiempo publicado de la primera autobomba que llega, desde su movilización hasta su llegada. NULL si no informado. |
| SegundoArriboSeg | int | Medida | SecondPumpArriving_AttendanceTime; tiempo desde movilización hasta llegada de la segunda autobomba que llega. NULL si no informado. |
| NumEstaciones | int | Medida | NumStationsWithPumpsAttending; estaciones con bombas que asistieron. |
| NumBombas | int | Medida | NumPumpsAttending; bombas que asistieron. |
| MinutosBombaRedondeados | int | Medida | PumpMinutesRounded; tiempo de bombas en el incidente, redondeado a 60 minutos si es inferior a una hora según metadatos. No duración exacta ni horas-persona. |
| CostoNocionalGBP | decimal(18,2) | Medida | Notional Cost (£); estimación nocional en GBP, no gasto contable. |
| NumLlamadas | int | Medida | NumCalls; llamadas asociadas al incidente. |
| TieneMovilizacion | bit | Control de enlace | 1 si existe al menos una movilización válida de 2025 con el mismo IncidentNumber después de deduplicar y separar conflictos; 0 en otro caso. No mide si el incidente recibió atención real. |

#### FactMovilizacion

Una fila por ResourceMobilisationId de CalYear=2025 aceptado tras deduplicar y separar versiones contradictorias.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| ResourceMobilisationId | bigint | PK | Identificador de negocio; se exige unicidad después de reglas de calidad. |
| IncidentNumber | varchar(30) | Atributo / control | Identificador degenerado para trazabilidad. Sin FK física a FactIncidente; permite rastrear el incidente de origen, incluidos registros sin correspondencia en la descarga. |
| FechaKey | int | FK DimFecha | Lookup en DimFecha; 0 cuando no puede resolverse. NOT NULL. |
| HoraKey | int | FK DimHora | Lookup en DimHora; 0 cuando no puede resolverse. NOT NULL. |
| TipoIncidenteKey | int | FK DimTipoIncidente | Lookup en DimTipoIncidente; 0 cuando no puede resolverse. NOT NULL. |
| TipoPropiedadKey | int | FK DimTipoPropiedad | Lookup en DimTipoPropiedad; 0 cuando no puede resolverse. NOT NULL. |
| GeografiaKey | int | FK DimGeografia | Lookup en DimGeografia; 0 cuando no puede resolverse. NOT NULL. |
| EstacionKey | int | FK DimEstacion | Lookup en DimEstacion; 0 cuando no puede resolverse. NOT NULL. |
| OrigenDespliegueKey | int | FK DimOrigenDespliegue | Lookup en DimOrigenDespliegue; 0 cuando no puede resolverse. NOT NULL. |
| MotivoDemoraKey | int | FK DimMotivoDemora | Lookup en DimMotivoDemora; 0 cuando no puede resolverse. NOT NULL. |
| ResourceCodigo | nvarchar(30) | Atributo / control | Resource_Code; identificador publicado del recurso, dimensión degenerada. |
| PerformanceReporting | nvarchar(30) | Atributo / control | Valor original 1, 2 o Not Used. No tratarlo como cantidad. |
| FechaHoraMovilizada | datetime2(0) | Atributo / control | DateAndTimeMobilised interpretado dd/MM/yyyy HH:mm; conservar GMT según documentación. |
| SalidaSeg | int | Medida | TurnoutTimeSeconds; segundos desde movilización hasta salida. |
| ViajeSeg | int | Medida | TravelTimeSeconds; segundos desde salida hasta llegada. |
| LlegadaSeg | int | Medida | AttendanceTimeSeconds; segundos publicados desde movilización hasta llegada. No es duración de la intervención. |
| TieneIncidente | bit | Atributo / control | 1 si IncidentNumber encuentra fila en el universo 2025 de incidentes, 0 si no. |

</details>

## 7. Referencias

Greater London Authority. (2025, 16 de enero). Fire response times (3) (Pregunta n.º 2025/0167) [Respuesta a pregunta al alcalde]. London City Hall. [Enlace a la fuente](https://www.london.gov.uk/who-we-are/what-london-assembly-does/questions-mayor/find-an-answer/fire-response-times-3)

London Assembly Research Unit. (2026). London Fire Brigade: Data analysis, March 2026. Greater London Authority. [Enlace a la fuente](https://www.london.gov.uk/who-we-are/what-london-assembly-does/london-assembly-research-unit-publications/london-fire-brigade)

London Fire Brigade. (2022, 6 de julio). Freedom of Information request reference number 6636.1 [Respuesta a solicitud de información]. [Enlace a la fuente](https://www.london-fire.gov.uk/media/6796/foi-response-66361.pdf)

London Fire Brigade. (2024, 5 de febrero). Freedom of Information request reference number 8420.1 [Respuesta a solicitud de información]. [Enlace a la fuente](https://www.london-fire.gov.uk/media/8863/foia84201-response-times-of-fire-brigades-and-data-collation-response.pdf)

London Fire Brigade. (2026a). London Fire Brigade Incident Records [Conjunto de datos]. London Datastore. Recuperado el 24 de septiembre de 2026 de [Enlace a la fuente](https://data.london.gov.uk/dataset/london-fire-brigade-incident-records-em8xy)

London Fire Brigade. (2026b). London Fire Brigade Mobilisation Records [Conjunto de datos]. London Datastore. Recuperado el 24 de septiembre de 2026 de [Enlace a la fuente](https://data.london.gov.uk/dataset/london-fire-brigade-mobilisation-records-24r65)

Ministry of Housing, Communities and Local Government. (2025). Fire and rescue incident statistics, year ending March 2025. GOV.UK. [Enlace a la fuente](https://www.gov.uk/government/statistics/fire-and-rescue-incident-statistics-year-ending-march-2025/fire-and-rescue-incident-statistics-year-ending-march-2025)

London Fire Brigade. (2023). Your London Fire Brigade: Our plan for 2023–29. [Enlace a la fuente](https://www.london-fire.gov.uk/about-us/your-london-fire-brigade-our-plan-for-2023-29/)

Los enlaces de descarga y el registro de las copias utilizadas están en [Datos oficiales](data/README.md).

Source: London Fire Brigade / London Datastore. Contains public sector information licensed under the [Open Government Licence v2.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/2/).

## Documentos

- [Documentos por sección](docs/README.md).
- [Datos oficiales y descargas](data/README.md).

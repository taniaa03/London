# Diccionario del modelo dimensional

Especificación de primera entrega; tipos de datos de referencia para el diseño. No se incluye implementación SQL en este entregable. Cada campo utilizado queda definido a continuación.

## DimFecha

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

## DimHora

Hora de llamada publicada; no equivale a la hora de movilización.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| HoraKey | int | PK | HourOfCall + 1; 1–24 representan 0–23; 0 desconocido. |
| Hora | tinyint | Atributo | HourOfCall, 0–23. |
| Franja | nvarchar(30) | Atributo | Agrupación académica: madrugada 0–5, mañana 6–11, tarde 12–17, noche 18–23; no son turnos oficiales. |

## DimTipoIncidente

Una combinación de grupo, clasificación y servicio especial.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| TipoIncidenteKey | int | PK | Clave sustituta; fila 0 reservada para desconocido. No procede de la fuente. |
| Grupo | nvarchar(100) | Atributo | IncidentGroup: Fire, False Alarm o Special Service; desconocido si vacío. |
| Clasificacion | nvarchar(200) | Atributo | StopCodeDescription; categoría detallada publicada. |
| ServicioEspecial | nvarchar(200) | Atributo | SpecialServiceType; No aplica fuera de Special Service, Desconocido cuando falta dentro de ese grupo. |

## DimTipoPropiedad

Una combinación de categoría y tipo de inmueble.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| TipoPropiedadKey | int | PK | Clave sustituta; fila 0 reservada para desconocido. No procede de la fuente. |
| Categoria | nvarchar(100) | Atributo | PropertyCategory. |
| Tipo | nvarchar(200) | Atributo | PropertyType. No identifica una dirección individual. |

## DimGeografia

Una combinación de borough, ward y distrito postal observados en incidentes.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| GeografiaKey | int | PK | Clave sustituta; fila 0 reservada para desconocido. No procede de la fuente. |
| BoroughCodigo | nvarchar(30) | Atributo | IncGeo_BoroughCode. |
| BoroughNombre | nvarchar(120) | Atributo | IncGeo_BoroughName. |
| WardCodigo | nvarchar(30) | Atributo | IncGeo_WardCode. |
| WardNombre | nvarchar(150) | Atributo | IncGeo_WardName. |
| DistritoPostal | nvarchar(30) | Atributo | Postcode_district; atributo de filtro, no nivel debajo de ward. |

## DimEstacion

Una estación de despliegue identificada por código.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| EstacionKey | int | PK | Clave sustituta; fila 0 reservada para desconocido. No procede de la fuente. |
| Codigo | nvarchar(30) | Atributo | DeployedFromStation_Code; clave de negocio. |
| Nombre | nvarchar(150) | Atributo | DeployedFromStation_Name. No equivale a IncidentStationGround. |

## DimOrigenDespliegue

Situación del recurso al desplegarse.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| OrigenDespliegueKey | int | PK | Clave sustituta; fila 0 reservada para desconocido. No procede de la fuente. |
| Origen | nvarchar(50) | Atributo | DeployedFromLocation: Home Station, Other Station o Desconocido. No es una coordenada ni una estación adicional. |

## DimMotivoDemora

Un código de motivo reportado.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| MotivoDemoraKey | int | PK | Clave sustituta; fila 0 reservada para desconocido. No procede de la fuente. |
| Codigo | nvarchar(30) | Atributo | DelayCodeId; conservar como texto. |
| Descripcion | nvarchar(200) | Atributo | DelayCode_Description. Not held up es ausencia explícita de demora; vacío es Desconocido. |

## FactIncidente

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

## FactMovilizacion

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

## Reglas comunes

Todas las FK son obligatorias y usan la fila 0 cuando corresponde. Las medidas desconocidas permanecen NULL; nunca se convierten a 0 por conveniencia. Las dimensiones con claves sustitutas reservan explícitamente el miembro 0. Para Fecha y Hora se generan claves deterministas. Se mantiene un calendario completo para representar todos los días del periodo, incluso aquellos sin registros.

Las dimensiones descriptivas se desnormalizan. En este corte congelado se plantea actualización tipo 1 para correcciones de etiquetas, manteniendo los originales y bitácoras de carga. Una futura comparación histórica deberá evaluar historial de cambios tipo 2 y cambios de límites territoriales. No se cuenta dos veces una dimensión por asumir varios roles.

Las longitudes propuestas son conservadoras; validar cualquier nueva descarga antes de cargar y rechazar truncamientos. Usar búsquedas de combinación completa para dimensiones compuestas y código para estación/demora. Si un código presenta dos nombres en la misma extracción, resolver en staging antes del lookup, sin elegir arbitrariamente.

## Reglas de enlace y tiempo

En FactMovilizacion, las cinco claves compartidas se obtienen del incidente enlazado por IncidentNumber. FechaKey y HoraKey representan el contexto de llamada; FechaHoraMovilizada conserva la marca GMT de movilización. Si no existe enlace, las cinco claves compartidas son 0 y TieneIncidente=0. Las tres dimensiones propias se obtienen de la movilización. Una marca GMT no se combina con una hora local sin verificar antes las convenciones de la fuente.

PerformanceReporting se conserva como categoría publicada, no como cantidad. El ejemplo oficial 1 corresponde al primer recurso que llega; el campo no sustituye la documentación completa del universo usado en reportes de desempeño.

DimHora conserva las horas publicadas y agrupaciones descriptivas. No representa la política de alarmas automáticas de 07:00–20:30: el límite de media hora requiere información y validación adicionales. Ese análisis de cumplimiento no forma parte del alcance.

[Modelo y reglas de agregación](../05_Modelo_multidimensional/Modelo_multidimensional.md) · [Diccionario de fuentes](Diccionario_fuentes.md).


## Campos de origen conservados fuera del modelo analítico

PumpOrder permanece en el respaldo de movilizaciones: su definición no especifica despacho o llegada y no se necesita para identificar el primer arribo. PlusCode_Code y PlusCode_Description también se conservan en la fuente; en 2025 solo distinguen Initial / Initial Mobilisation y no aportan segmentación a la pregunta. No se agregan como dimensión ni como atributos de FactMovilizacion.

La clasificación de falsa alarma se obtiene de DimTipoIncidente.Grupo=False Alarm en ambos hechos. No se duplica mediante una bandera EsFalsaAlarma en FactIncidente. El conteo de incidentes se obtiene contando sus filas y el de movilizaciones contando sus filas válidas; no se incorpora una cantidad ficticia procedente de la fuente.

## Universo para combinar los dos hechos

Para estudiar incidentes y movilizaciones sobre el mismo conjunto de atenciones se aplican conjuntamente FactIncidente.TieneMovilizacion=1 y FactMovilizacion.TieneIncidente=1. Ambos controles se calculan después de resolver duplicados y separar conflictos. Los incidentes sin enlace no se eliminan de FactIncidente y las movilizaciones sin enlace permanecen con sus dimensiones compartidas desconocidas.

Este universo común se usa en comparaciones que combinan medidas de ambos hechos. Para describir toda la demanda publicada puede usarse FactIncidente completo, identificando que es un universo más amplio. Cada promedio debe mostrar su cantidad de valores válidos y no sustituir tiempos ausentes por cero. Estos controles de enlace no demuestran ausencia de asistencia ni falta de disponibilidad operativa.

# Diccionario del modelo dimensional

Tipos de datos, claves y reglas de transformación de los campos del modelo.

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
| Franja | nvarchar(30) | Atributo | Agrupación horaria: madrugada 0–5, mañana 6–11, tarde 12–17, noche 18–23; no son turnos oficiales. |

## DimTipoIncidente

Una combinación de grupo, clasificación y servicio especial.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| TipoIncidenteKey | int | PK | IDENTITY(1,1) para identificar cada combinación de Grupo, Clasificacion y ServicioEspecial, que no tiene un código único en la fuente. Clave 0 para desconocido. |
| Grupo | nvarchar(100) | Atributo | IncidentGroup: Fire, False Alarm o Special Service; desconocido si vacío. |
| Clasificacion | nvarchar(200) | Atributo | StopCodeDescription; categoría detallada publicada. |
| ServicioEspecial | nvarchar(200) | Atributo | SpecialServiceType; No aplica fuera de Special Service, Desconocido cuando falta dentro de ese grupo. |

## DimTipoPropiedad

Una combinación de categoría y tipo de inmueble.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| TipoPropiedadKey | int | PK | IDENTITY(1,1) para identificar cada combinación de Categoria y Tipo, que no tiene un código único en la fuente. Clave 0 para desconocido. |
| Categoria | nvarchar(100) | Atributo | PropertyCategory. |
| Tipo | nvarchar(200) | Atributo | PropertyType. No identifica una dirección individual. |

## DimGeografia

Una combinación de borough, ward y distrito postal observados en incidentes.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| GeografiaKey | int | PK | IDENTITY(1,1) para identificar la combinación territorial de borough, ward y distrito postal; ninguno de sus códigos identifica por sí solo esa combinación. Clave 0 para desconocido. |
| BoroughCodigo | nvarchar(30) | Atributo | IncGeo_BoroughCode. |
| BoroughNombre | nvarchar(120) | Atributo | IncGeo_BoroughName. |
| WardCodigo | nvarchar(30) | Atributo | IncGeo_WardCode. |
| WardNombre | nvarchar(150) | Atributo | IncGeo_WardName. |
| DistritoPostal | nvarchar(30) | Atributo | Postcode_district; atributo de filtro, no nivel debajo de ward. |

## DimEstacion

Una estación de despliegue identificada por código.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| EstacionKey | int | PK | IDENTITY(1,1) como clave interna entera. El código alfanumérico de la estación se conserva en Codigo y se usa para buscar o reutilizar la clave. Clave 0 para desconocido. |
| Codigo | nvarchar(30) | Atributo | DeployedFromStation_Code; clave de negocio. |
| Nombre | nvarchar(150) | Atributo | DeployedFromStation_Name. No equivale a IncidentStationGround. |

## DimOrigenDespliegue

Situación del recurso al desplegarse.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| OrigenDespliegueKey | int | PK | Sin IDENTITY. Catálogo fijo: 0=Desconocido, 1=Home Station, 2=Other Station. Los valores ausentes de la fuente se asignan a 0. |
| Origen | nvarchar(50) | Atributo | DeployedFromLocation: Home Station, Other Station o Desconocido. No es una coordenada ni una estación adicional. |

## DimMotivoDemora

Un código de motivo reportado.

| Campo | Tipo de dato | Rol | Definición y transformación |
|---|---|---|---|
| MotivoDemoraKey | int | PK | Sin IDENTITY. Valor entero de DelayCodeId, validado como código numérico positivo en 2025; 0 para desconocido. Codigo conserva el valor original como texto. |
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

Todas las FK son obligatorias y usan la fila 0 cuando corresponde. Las medidas desconocidas permanecen NULL; nunca se convierten a 0 por conveniencia. En las cuatro dimensiones con IDENTITY(1,1), la fila desconocida con clave 0 se cargará explícitamente mediante IDENTITY_INSERT. Para Fecha y Hora se generan claves deterministas. Se mantiene un calendario completo para representar todos los días del periodo, incluso aquellos sin registros.

Usaremos dimensiones desnormalizadas y actualizaciones tipo 1 para corregir etiquetas. Conservaremos los archivos originales y el registro de los cambios realizados durante la carga.

Antes de cargar los datos verificaremos tipos y longitudes. Buscaremos las combinaciones de incidente, propiedad y geografía y el código de estación antes de insertar: si ya existen, reutilizaremos su clave. En origen aplicaremos el catálogo fijo y en demora validaremos DelayCodeId y su descripción. Los códigos nuevos o contradictorios se revisarán antes de cargar. IDENTITY solo genera números; las restricciones PK y UNIQUE y la validación de las claves de negocio evitarán duplicados.

## Reglas de enlace y tiempo

En FactMovilizacion, las cinco claves compartidas se obtienen del incidente enlazado por IncidentNumber. FechaKey y HoraKey representan el contexto de llamada; FechaHoraMovilizada conserva la marca GMT de movilización. Si no existe enlace, las cinco claves compartidas son 0 y TieneIncidente=0. Las tres dimensiones propias se obtienen de la movilización. Una marca GMT no se combina con una hora local sin verificar antes las convenciones de la fuente.

PerformanceReporting se conserva como categoría publicada, no como cantidad. El ejemplo oficial 1 corresponde al primer recurso que llega; el campo no sustituye la documentación completa del universo usado en reportes de desempeño.

[Modelo y reglas de agregación](../05_Modelo_multidimensional/Modelo_multidimensional.md) · [Diccionario de fuentes](Diccionario_fuentes.md).

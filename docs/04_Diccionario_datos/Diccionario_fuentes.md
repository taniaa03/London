# Diccionario de las fuentes

Se documentan los 39 campos de incidentes y los 24 de movilizaciones presentes en los archivos recientes. El tipo de referencia expresa cómo interpretar el campo; no modifica los archivos oficiales. La columna de definición señala qué campos se usan en el modelo y cuáles se conservan solo como respaldo de origen.

El CSV adjunto conserva también las descripciones y notas originales del proveedor. El diccionario del modelo especifica las claves y los campos derivados.

## Incidentes

| Campo | Tipo de referencia | Unidad / formato | Definición y uso |
|---|---|---|---|
| IncidentNumber | Texto | No aplica | Identificador de incidente; unión entre fuentes; mantener como texto. |
| DateOfCall | Fecha | Día calendario | Fecha publicada de llamada/registro; base de DimFecha. |
| CalYear | Entero | Año | Año publicado; filtro común CalYear=2025 y control con DateOfCall. |
| TimeOfCall | Hora | hh:mm:ss publicado | Hora detallada publicada; conservar en respaldo de origen para control, no emplear para calcular duraciones. |
| HourOfCall | Entero | Hora 0–23 | Hora publicada 0–23; base de DimHora. |
| IncidentGroup | Texto | No aplica | Grupo del incidente; DimTipoIncidente. La categoría False Alarm permite filtrar falsas alarmas en ambos hechos. |
| StopCodeDescription | Texto | No aplica | Clasificación detallada; DimTipoIncidente. |
| SpecialServiceType | Texto | No aplica | Tipo de servicio especial; DimTipoIncidente. |
| PropertyCategory | Texto | No aplica | Categoría del inmueble; DimTipoPropiedad. |
| PropertyType | Texto | No aplica | Tipo de inmueble; DimTipoPropiedad. |
| AddressQualifier | Texto | No aplica | Calificador de ubicación; respaldo de origen, sin dimensión de dirección en el alcance. |
| Postcode_full | Texto | No aplica | Código postal completo; respaldo de origen, no publicar ubicación individual. |
| Postcode_district | Texto | No aplica | Distrito postal; DimGeografia. |
| UPRN | Texto | No aplica | Referencia única de inmueble; respaldo de origen, no usada en el modelo. |
| USRN | Texto | No aplica | Referencia de calle; respaldo de origen, no usada en el modelo. |
| IncGeo_BoroughCode | Texto | No aplica | Código de borough; DimGeografia. |
| IncGeo_BoroughName | Texto | No aplica | Nombre de borough; DimGeografia. |
| ProperCase | Texto | No aplica | Nombre territorial con formato de mayúsculas/minúsculas; respaldo de origen, etiqueta redundante. |
| IncGeo_WardCode | Texto | No aplica | Código de ward; DimGeografia. |
| IncGeo_WardName | Texto | No aplica | Nombre de ward; DimGeografia. |
| IncGeo_WardNameNew | Texto | No aplica | Nombre alternativo/actualizado de ward; respaldo de origen, no mezclar con la versión elegida. |
| Easting_m | Número | Metros en sistema de coordenadas de origen | Coordenada este en metros; respaldo de origen, no necesaria para agregación territorial. |
| Northing_m | Número | Metros en sistema de coordenadas de origen | Coordenada norte en metros; respaldo de origen. |
| Easting_rounded | Número | Metros en sistema de coordenadas de origen | Coordenada este redondeada; respaldo de origen. |
| Northing_rounded | Número | Metros en sistema de coordenadas de origen | Coordenada norte redondeada; respaldo de origen. |
| Latitude | Decimal | Grados | Latitud; respaldo de origen, puede omitirse intencionalmente en viviendas. |
| Longitude | Decimal | Grados | Longitud; respaldo de origen, puede omitirse intencionalmente en viviendas. |
| FRS | Texto | No aplica | Servicio de bomberos territorial publicado; respaldo de origen. |
| IncidentStationGround | Texto | No aplica | Área de estación del incidente; respaldo de origen. No es la estación que desplegó el recurso. |
| FirstPumpArriving_AttendanceTime | Entero | Segundos | Tiempo publicado del primer arribo, segundos; FactIncidente.PrimerArriboSeg. |
| FirstPumpArriving_DeployedFromStation | Texto | No aplica | Estación de la primera bomba; respaldo de origen, no atribuir todos los incidentes a DimEstacion. |
| SecondPumpArriving_AttendanceTime | Entero | Segundos | Tiempo publicado del segundo arribo, segundos; FactIncidente.SegundoArriboSeg. |
| SecondPumpArriving_DeployedFromStation | Texto | No aplica | Estación de la segunda bomba; respaldo de origen. |
| NumStationsWithPumpsAttending | Entero | Conteo / orden | Número de estaciones con bombas asistentes; FactIncidente.NumEstaciones. |
| NumPumpsAttending | Entero | Conteo / orden | Número de bombas asistentes; FactIncidente.NumBombas. |
| PumpCount | Texto | No aplica | Campo presente sin definición suficiente en diccionario; respaldo de origen, excluido de medidas para no asumir equivalencia. |
| PumpMinutesRounded | Entero | Minutos publicados con redondeo | Tiempo de bombas en el incidente, redondeado a 60 minutos si es inferior a una hora según metadatos; FactIncidente.MinutosBombaRedondeados. No es tiempo exacto ni horas-persona. |
| Notional Cost (£) | Decimal | GBP; costo nocional | Costo nocional en GBP; FactIncidente.CostoNocionalGBP. |
| NumCalls | Entero | Conteo / orden | Número de llamadas; FactIncidente.NumLlamadas. |

## Movilizaciones

| Campo | Tipo de referencia | Unidad / formato | Definición y uso |
|---|---|---|---|
| IncidentNumber | Texto | No aplica | Identificador de incidente; unión entre fuentes; mantener como texto. |
| CalYear | Entero | Año | Año publicado; filtro común CalYear=2025 y control con DateOfCall. |
| BoroughName | Texto | No aplica | Nombre de borough observado en CSV, ausente del diccionario de movilizaciones; control contra incidentes, sin enriquecer huérfanos con geografía parcial. |
| WardName | Texto | No aplica | Nombre de ward observado en CSV, ausente del diccionario de movilizaciones; control contra incidentes. |
| HourOfCall | Entero | Hora 0–23 | Hora publicada 0–23; base de DimHora. |
| ResourceMobilisationId | Entero largo | Identificador | Identificador publicado de movilización; PK solo tras resolver duplicados y conflictos. |
| Resource_Code | Texto | No aplica | Código de recurso; atributo degenerado en FactMovilizacion. |
| PerformanceReporting | Texto | No aplica | Clasificación para reporte de desempeño; atributo de filtro, no suma. |
| DateAndTimeMobilised | Fecha y hora | dd/MM/yyyy HH:mm, GMT según metadatos | Fecha/hora de movilización; FactMovilizacion.FechaHoraMovilizada. |
| DateAndTimeMobile | Fecha y hora | dd/MM/yyyy HH:mm, GMT según metadatos | Fecha/hora de salida; respaldo de origen para control, precisión de minutos. |
| DateAndTimeArrived | Fecha y hora | dd/MM/yyyy HH:mm, GMT según metadatos | Fecha/hora de llegada; respaldo de origen para control, precisión de minutos. |
| TurnoutTimeSeconds | Entero | Segundos | Tiempo desde movilización hasta salida, en segundos; FactMovilizacion.SalidaSeg. |
| TravelTimeSeconds | Entero | Segundos | Tiempo desde salida hasta llegada, en segundos; FactMovilizacion.ViajeSeg. |
| AttendanceTimeSeconds | Entero | Segundos | Tiempo desde movilización hasta llegada, en segundos; FactMovilizacion.LlegadaSeg. No es la duración de la intervención. |
| DateAndTimeLeft | Fecha y hora | dd/MM/yyyy HH:mm, GMT según metadatos | Fecha/hora de abandono de escena; respaldo de origen, no se utiliza para duración total. |
| DateAndTimeReturned | Fecha y hora | dd/MM/yyyy HH:mm, GMT según metadatos | Fecha/hora de regreso; casi siempre ausente, respaldo de origen; no apta para medir disponibilidad. |
| DeployedFromStation_Code | Texto | No aplica | Código de estación de despliegue; DimEstacion. |
| DeployedFromStation_Name | Texto | No aplica | Nombre de estación de despliegue; DimEstacion. |
| DeployedFromLocation | Texto | No aplica | Origen operativo Home Station / Other Station; DimOrigenDespliegue. |
| PumpOrder | Entero | Conteo / orden | Orden publicado como Pump order; el diccionario no precisa despacho o llegada. Respaldo de origen, fuera del modelo analítico; no se usa para identificar primer arribo. |
| PlusCode_Code | Texto | No aplica | Código del tipo de movilización; respaldo de origen. Constante Initial en 2025, fuera del modelo analítico. |
| PlusCode_Description | Texto | No aplica | Descripción del tipo de movilización; respaldo de origen. Constante Initial Mobilisation en 2025, fuera del modelo analítico. |
| DelayCodeId | Texto | No aplica | Código de demora; DimMotivoDemora. |
| DelayCode_Description | Texto | No aplica | Descripción de demora; DimMotivoDemora. |

## Reglas de interpretación

- Las claves de incidente conservan ceros y guiones. Los códigos de estaciones, demoras y territorios se tratan como texto.

- Una falsa alarma se identifica por la clasificación publicada. No se infiere a partir de costo, tiempo o tipo de propiedad.

- NULL y vacío representan ausencia. En medidas no se sustituyen por cero; en dimensiones se identifica la categoría desconocida.

- Se usan los tiempos numéricos publicados en segundos. Las marcas de movilización tienen precisión de minutos y no deben restarse para reemplazar esas duraciones.

- Motivo de demora se refiere al desplazamiento y no a la causa de la falsa alarma. Not held up es distinto de desconocido.

- Las coordenadas de viviendas pueden omitirse por las reglas de publicación. No se reconstruyen direcciones ni se imputan puntos.

- PumpCount carece de definición suficiente en el diccionario consultado; se conserva como texto de origen y no se usa como medida.

- BoroughName y WardName son columnas observadas en movilizaciones, ausentes de su diccionario oficial. Se señalan para trazabilidad; la geografía del modelo procede del incidente enlazado.

[Diccionario del modelo](Diccionario_modelo.md) · [Versión CSV](Diccionario_fuentes.csv) · [Fuentes originales](../06_Fuentes/Referencias.md).

## Categorías de falsas alarmas

Se filtra primero IncidentGroup=False Alarm y después se utiliza StopCodeDescription. En la descarga de 2025 se observaron estas etiquetas:

| Etiqueta publicada | Interpretación y límite |
|---|---|
| AFA | Alarma automática en un incidente clasificado como falsa alarma. No identifica por sí sola el motivo técnico de activación. |
| False alarm - Good intent | Aviso realizado de buena fe al creer que existía un incendio. |
| False alarm - Malicious | Aviso deliberado de un incidente inexistente. |
| Alleged Fire Risk | Etiqueta publicada que se conserva; el diccionario no aporta detalle suficiente para atribuirle una causa específica. |

Los campos documentan la clasificación final, no el protocolo de comprobación aplicado por los bomberos ni una descripción libre de la investigación. Las categorías generales de buena fe y aviso malicioso se interpretan según las [definiciones oficiales](https://www.gov.uk/government/statistics/fire-and-rescue-incident-statistics-year-ending-march-2025/fire-and-rescue-incident-statistics-year-ending-march-2025). No se deduce falta de mantenimiento, humo de cocina u otra causa concreta a partir de AFA.

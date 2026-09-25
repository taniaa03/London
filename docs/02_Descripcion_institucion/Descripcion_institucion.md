# 3. Descripción de la institución

## 3.1. Institución y finalidad

London Fire Brigade (LFB) es el servicio de bomberos y rescate de Londres. Combina la respuesta a incidentes con actividades de prevención y protección de la comunidad. Su plan 2023–2029 plantea reducir y responder al riesgo en Londres. [Plan institucional de LFB](https://www.london-fire.gov.uk/about-us/your-london-fire-brigade-our-plan-for-2023-29/).

## 3.2. Proceso de atención

| Etapa del proceso general | Relación con el proyecto |
|---|---|
| Recepción del aviso | Fecha, hora, lugar y clasificación publicada permiten describir la demanda. |
| Movilización de unidades | Identificaremos cada recurso movilizado, la estación y el origen de su despliegue. |
| Salida, desplazamiento y llegada | Compararemos los tiempos en segundos y los motivos de demora reportados. |
| Intervención | Examinaremos las autobombas asistentes, los minutos publicados con redondeo y el costo nocional del incidente. |
| Retorno de unidades | Forma parte del proceso general, pero queda fuera del análisis de duración y disponibilidad por insuficiencia del campo de retorno. |

## 3.3. Usuarios previstos

Orientamos el análisis a las decisiones de las áreas de operaciones, planificación y prevención de LFB.

| Área | Decisión |
|---|---|
| Operaciones y planificación | Seleccionar zonas y tipos de atención en los que conviene examinar tiempos de llegada y demoras reportadas. |
| Prevención | Identificar concentraciones de falsas alarmas por zona y tipo de inmueble para investigar sus posibles causas. |

## 3.4. Fuentes de datos

Utilizaremos los registros abiertos oficiales de [incidentes](https://data.london.gov.uk/dataset/london-fire-brigade-incident-records-em8xy) y [movilizaciones](https://data.london.gov.uk/dataset/london-fire-brigade-mobilisation-records-24r65), publicados por LFB en London Datastore. Trabajaremos con la descarga del 24 de septiembre de 2026 y seleccionaremos el año 2025, que está completo en ambos archivos.

La fuente de incidentes describe cada atención; la de movilizaciones, cada recurso desplegado. Las relacionaremos mediante IncidentNumber.

[Volver al informe principal](../../README.md) · [Documentos](../README.md).

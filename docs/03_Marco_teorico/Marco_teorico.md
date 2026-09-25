# 4. Marco teórico

## 4.1. Business Intelligence como apoyo a la gestión

Business Intelligence reúne y organiza datos para apoyar la toma de decisiones. Aplicaremos este enfoque para comparar los tiempos de respuesta y los recursos utilizados por LFB según la zona y el tipo de incidente.

## 4.2. Hechos y nivel de detalle

Un hecho representa un evento medible de un proceso. El nivel de detalle o grano define qué representa cada fila de una tabla de hechos. En nuestro caso distinguimos el incidente atendido y la movilización de un recurso: un mismo incidente puede generar varias movilizaciones, cada una con sus propios tiempos.

## 4.3. Dimensiones y modelo multidimensional

Las dimensiones describen las características de un hecho, como su fecha, ubicación o tipo de incidente. Un esquema estrella relaciona una tabla de hechos con sus dimensiones. Cuando varias tablas de hechos comparten dimensiones, forman una constelación de estrellas.

Las dimensiones compartidas usan las mismas claves y definiciones. Esto permite comparar resultados de incidentes y movilizaciones por fecha, hora, zona, tipo de incidente y tipo de propiedad.

## 4.4. Datamart y análisis multidimensional

Un datamart reúne datos de un ámbito de la institución; el nuestro se centra en la atención de incidentes. OLTP se orienta al registro de transacciones y OLAP a su análisis. Las jerarquías permiten pasar de un nivel general a uno más detallado, por ejemplo, de año a mes o de borough a ward.

## 4.5. Integración y significado de los datos

El proceso ETL —extracción, transformación y carga— prepara los datos para el análisis. En este caso requiere relacionar incidentes y movilizaciones, uniformizar formatos, revisar duplicados y distinguir los valores ausentes de los valores registrados.

## 4.6. Conceptos operativos

| Concepto | Significado en el proyecto |
|---|---|
| Tiempo de llegada | Intervalo desde la movilización hasta la llegada del recurso; no incluye por sí solo todo el tiempo desde la llamada. |
| Primer arribo | Llegada de la primera autobomba al incidente; se distingue del tiempo de cada recurso que participa. |
| Demora reportada | Motivo registrado para contextualizar la movilización; no expresa cuántos segundos se perdieron exclusivamente por ese motivo. |
| Falsa alarma | Clasificación final de un aviso atendido como posible incendio; se identifica por IncidentGroup, sin inferirla a partir del costo o el tiempo. |
| Costo nocional | Valoración estimada del tiempo de autobombas según una tarifa estándar; no corresponde a una pérdida presupuestaria demostrada. |

Las definiciones de tiempos y costo se apoyan en los metadatos y en las aclaraciones de LFB de [2024](https://www.london-fire.gov.uk/media/8863/foia84201-response-times-of-fire-brigades-and-data-collation-response.pdf) y [2022](https://www.london-fire.gov.uk/media/6796/foi-response-66361.pdf). La clasificación de falsas alarmas se interpreta con las [definiciones oficiales de Inglaterra](https://www.gov.uk/government/statistics/fire-and-rescue-incident-statistics-year-ending-march-2025/fire-and-rescue-incident-statistics-year-ending-march-2025).

[Volver al informe principal](../../README.md) · [Documentos](../README.md).

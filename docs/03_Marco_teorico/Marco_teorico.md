# 4. Marco teórico

## 4.1. Business Intelligence como apoyo a la gestión

Business Intelligence reúne y organiza datos para apoyar la toma de decisiones. Aplicaremos este enfoque para comparar los tiempos de respuesta y los recursos utilizados por LFB según la zona y el tipo de incidente.

El diseño dimensional sigue cuatro pasos: seleccionar el proceso, definir el grano, identificar las dimensiones y establecer las medidas ([Kimball Group, proceso de diseño](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/four-4-step-design-process/)).

## 4.2. Proceso, hechos y nivel de detalle

Estudiaremos la atención operativa de incidentes mediante los registros de incidentes y movilizaciones. Un **hecho** representa un evento medible de ese proceso. El **nivel de detalle o grano** establece qué significa una fila. Aquí existen dos: el incidente atendido y la movilización de un recurso hacia ese incidente. Un incidente puede requerir varios recursos; por eso no deben contarse como si fueran lo mismo.

Definimos una tabla de hechos para cada nivel de detalle. El incidente permite estudiar la demanda y su clasificación final; la movilización permite describir los recursos desplegados y el contexto de su desplazamiento. Las falsas alarmas se analizan como una categoría del incidente, no como un proceso con un grano diferente. Para comparar medidas de ambas tablas utilizaremos el mismo conjunto de incidentes enlazados. El costo nocional del incidente permanece una sola vez, aunque hayan participado varios recursos. Cada grano requiere su propia tabla de hechos; las medidas deben corresponder al evento representado por la fila ([Kimball Group, grano](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/grain/)).

## 4.3. Dimensiones y modelo multidimensional

Las dimensiones describen un hecho desde distintas perspectivas: cuándo ocurrió, dónde, qué tipo de incidente fue y qué clase de inmueble estuvo involucrada. En el despliegue se añaden estación, origen del recurso y motivo de demora reportado.

El modelo estrella relaciona una tabla de hechos con dimensiones descriptivas. Al compartir dimensiones entre dos hechos se forma una constelación de estrellas. Este diseño se adapta al proceso: las dimensiones comunes relacionan la demanda con el despliegue, mientras las propias de movilización describen su ejecución. En nuestro modelo definimos ocho dimensiones. Fecha, hora, tipo de incidente, tipo de propiedad y geografía serán conformadas: tendrán el mismo significado y las mismas claves en ambos hechos. Esto permite agregar cada hecho por separado y comparar los resultados en los mismos grupos ([Kimball Group, dimensiones conformadas](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/conformed-dimension/)).

## 4.4. Datamart y análisis multidimensional

Un datamart organiza información de un ámbito de la institución. El alcance aquí es la atención de incidentes durante 2025, con dos perspectivas relacionadas: tiempos de respuesta y recursos utilizados. Los incendios, falsas alarmas y servicios especiales se distinguen para comparar atenciones de características semejantes.

OLTP se orienta al registro de transacciones y OLAP a su análisis desde distintas perspectivas. Usaremos estas jerarquías para examinar un año por meses, un borough por wards y una categoría de propiedad por tipos específicos. Estos niveles forman las jerarquías de análisis.

## 4.5. Integración y significado de los datos

El proceso ETL —extracción, transformación y carga— prepara los datos para el análisis. En este caso requiere relacionar incidentes y movilizaciones, uniformizar formatos, revisar duplicados y distinguir los valores ausentes de los valores registrados.

## 4.6. Conceptos operativos

| Concepto | Significado en el proyecto |
|---|---|
| Atención de incidente | Evento atendido y clasificado por LFB; puede generar varias movilizaciones. |
| Movilización | Despliegue individual de un recurso hacia un incidente. |
| Tiempo de llegada | Intervalo desde la movilización hasta la llegada del recurso; no incluye por sí solo todo el tiempo desde la llamada. |
| Primer arribo | Llegada de la primera autobomba al incidente; se distingue del tiempo de cada recurso que participa. |
| Demora reportada | Motivo registrado para contextualizar la movilización; no expresa cuántos segundos se perdieron exclusivamente por ese motivo. |
| Falsa alarma | Clasificación final de un aviso atendido como posible incendio; se identifica por IncidentGroup, sin inferirla a partir del costo o el tiempo. |
| Costo nocional | Valoración estimada del tiempo de autobombas según una tarifa estándar; no corresponde a una pérdida presupuestaria demostrada. |
| Priorización | Selección fundamentada de zonas y tipos de atención que conviene revisar; requiere comparar atenciones de características semejantes. |

Las definiciones de tiempos y costo se apoyan en los metadatos y en las aclaraciones de LFB de [2024](https://www.london-fire.gov.uk/media/8863/foia84201-response-times-of-fire-brigades-and-data-collation-response.pdf) y [2022](https://www.london-fire.gov.uk/media/6796/foi-response-66361.pdf). La clasificación de falsas alarmas se interpreta con las [definiciones oficiales de Inglaterra](https://www.gov.uk/government/statistics/fire-and-rescue-incident-statistics-year-ending-march-2025/fire-and-rescue-incident-statistics-year-ending-march-2025).

[Volver al informe principal](../../README.md) · [Documentos](../README.md).

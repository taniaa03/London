# London
# London Fire Brigade (LFB) - Business Intelligence Solution

## 1. Integrantes del Grupo
* Tania Lisset Chavez Alvitez
* Brayan Anderson Rua Pomahuacre
* Alyssa Antuanette Trujillo Cruzado
* Thiago Cesar Ormeño Freundt

## 2. Marco Teórico
* **Principios de Business Intelligence:** Integración y transformación de datos operacionales para la toma de decisiones estratégicas en servicios públicos de emergencia.
* **Metodología de Modelado Dimensional (Ralph Kimball):** Implementación de un Esquema en Estrella (*Star Schema*) compuesto por una tabla de hechos transaccional y dimensiones conformadas.
* **Procesos ETL:** Extracción, limpieza y carga de registros de incidentes y movilizaciones hacia un repositorio analítico.

## 3. Descripción de la Entidad y Problemática
* **Entidad:** El proyecto toma como institución de referencia a la London Fire Brigade (LFB), el servicio de bomberos y rescate de la ciudad de Londres. Está dirigida por el London Fire Commissioner y supervisada por el Alcalde de Londres. Es el servicio de bomberos más ocupado del Reino Unido y uno de los más grandes del mundo: cada año recibe alrededor de un cuarto de millón de llamadas al 999, de las cuales aproximadamente 120 000 terminan en un incidente que requiere enviar un camión.
  
* **Problemática:** En una emergencia, los primeros minutos son decisivos. Por ello, el tiempo de respuesta es el indicador central con el que se evalúa el desempeño de un servicio de bomberos. La LFB se compromete a que el primer camión llegue en un promedio de 6 minutos y el segundo en 8. En promedio cumple (5 min 34 s entre enero de 2025 y julio de 2026), pero el promedio de Londres esconde brechas: solo el 65,5 % de los primeros camiones llega en 6 minutos o menos, y distritos como Hillingdon, Havering, Bromley y Enfield superan los 6 minutos. Además, las falsas alarmas representan el 57,6 % de las movilizaciones. La información para analizar estas brechas está dividida en dos archivos separados (incidentes y movilizaciones), lo que impide saber con rapidez dónde, cuándo y por qué no se cumplen los estándares.
  
* **Objetivo:** Diseñar un datamart que integre los registros de incidentes y movilizaciones de la LFB para analizar el cumplimiento de los estándares de tiempo de respuesta y el uso de sus recursos.

## 4. Modelamiento Dimensional (8 Dimensiones)


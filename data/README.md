# Datos oficiales del proyecto

Trabajaremos con los registros de London Fire Brigade publicados en London Datastore. El periodo de análisis es del 1 de enero al 31 de diciembre de **2025**. Los archivos originales incluyen otros años; conservar el original y filtrar `CalYear=2025` en ambas fuentes al preparar el análisis.

## Archivos para el estudio

| Fuente | Descarga completa | Cobertura de la copia del grupo |
|---|---|---|
| Movilizaciones | [Archivo oficial](https://data.london.gov.uk/download/24r65/7d5b4e2f-3ddb-48b9-8a4d-bf86bdf6a4ef/LFB%20Mobilisation%20data%20from%202025.csv) | 2025 a julio de 2026 |
| Incidentes | [Archivo oficial](https://data.london.gov.uk/download/em8xy/58m/LFB%20Incident%20data%20from%202024%20onwards.xlsx) | 2024 a julio de 2026 |

Catálogos oficiales: [Incident Records](https://data.london.gov.uk/dataset/london-fire-brigade-incident-records-em8xy) y [Mobilisation Records](https://data.london.gov.uk/dataset/london-fire-brigade-mobilisation-records-24r65).

## Cómo preparar las copias locales

1. Descargar los dos archivos completos mediante los enlaces anteriores.
2. Guardarlos en `data/raw/recientes/`, con los nombres indicados en el [manifest](manifest_descargas.json). Esta carpeta está excluida de Git mediante `.gitignore`.
3. Conservar los originales y aplicar el filtro `CalYear=2025` en la preparación del análisis.
4. Consultar el [diccionario de fuentes](../docs/04_Diccionario_datos/Diccionario_fuentes.md) y las [reglas de enlace y agregación](../docs/05_Modelo_multidimensional/Modelo_multidimensional.md) antes de combinar los archivos. Un incidente puede tener varias movilizaciones.

## Diccionarios oficiales y trazabilidad

- [Metadatos de incidentes](../docs/04_Diccionario_datos/metadatos/diccionario_incidentes.xlsx).
- [Metadatos de movilizaciones](../docs/04_Diccionario_datos/metadatos/diccionario_movilizaciones.xlsx).
- [Manifest de descargas](manifest_descargas.json): URL, tamaño, SHA-256, fecha de descarga y ubicación prevista para cada archivo. Incluye descargas históricas como respaldo; no amplían el alcance del estudio.

Los tamaños y hashes describen las copias descargadas por el grupo el 24 de septiembre de 2026. El proveedor puede actualizar los archivos de esos mismos enlaces; una descarga posterior puede tener otro contenido y otro hash. La copia original descargada se conserva localmente para mantener el corte del estudio.

Los archivos de datos grandes no se incluyen en Git; los diccionarios oficiales sí se incluyen. Los valores ausentes, duplicados y conflictos de identificación se tratan según las reglas documentadas en el modelo, sin reemplazar tiempos desconocidos por cero.

Source: London Fire Brigade / London Datastore. Contains public sector information licensed under the [Open Government Licence v2.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/2/).

[Volver al informe](../README.md).

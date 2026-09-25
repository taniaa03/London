# 5. Diccionario de datos

El diccionario documenta los campos publicados y su correspondencia con el diseño. La fuente de incidentes contiene 39 campos y la de movilizaciones 24; sus nombres originales se conservan para permitir la trazabilidad.

| Documento | Contenido |
|---|---|
| [Diccionario de fuentes](../04_Diccionario_datos/Diccionario_fuentes.md) | Los 63 campos: significado, unidad, uso en el modelo o conservación como respaldo. |
| [Diccionario de fuentes en CSV](../04_Diccionario_datos/Diccionario_fuentes.csv) | Versión tabular con las definiciones y notas originales del proveedor. |
| [Diccionario del modelo](../04_Diccionario_datos/Diccionario_modelo.md) | Todos los campos de las ocho dimensiones y los dos hechos: tipo de dato, rol, definición y transformación. |

Los campos de clasificación responden qué se atendió; los geográficos y temporales, dónde y cuándo; los tiempos y motivos de demora, cómo fue la llegada; y las medidas de despliegue y costo, qué recursos se asociaron a la atención.

Los [metadatos oficiales de incidentes](metadatos/diccionario_incidentes.xlsx) y [movilizaciones](metadatos/diccionario_movilizaciones.xlsx) se incluyen sin modificaciones.

## 5.1. Distinciones necesarias para el análisis

- FirstPumpArriving_AttendanceTime corresponde a la primera autobomba que llegó al incidente. AttendanceTimeSeconds corresponde a una movilización. No son observaciones intercambiables.

- TurnoutTimeSeconds describe la salida desde la movilización; TravelTimeSeconds, el viaje; AttendanceTimeSeconds, el intervalo hasta la llegada. Se usan los segundos publicados y no se reconstruyen a partir de marcas redondeadas a minutos.

- False Alarm es un valor de IncidentGroup. StopCodeDescription permite distinguir categorías como AFA, Good intent o Malicious; no contiene necesariamente la causa técnica de activación.

- DelayCode_Description describe un motivo de demora de la movilización. No indica por qué se activó una alarma ni cuantifica por sí solo los segundos perdidos por tráfico.

- PumpMinutesRounded conserva el redondeo del proveedor y no equivale a duración exacta del incidente ni a horas-persona.

- Notional Cost (£) es una estimación en libras y se mantiene a nivel incidente. Unirla con varias movilizaciones no autoriza a sumarla repetidamente.

Los valores ausentes se distinguen de cero y de categorías explícitas como Not held up. PumpCount se conserva en la fuente y queda fuera de las medidas por falta de definición suficiente. PumpOrder no se usa para inferir el primer arribo porque su diccionario solo lo describe como “Pump order”.

[Volver al informe principal](../../README.md) · [Índice de componentes](../README.md).

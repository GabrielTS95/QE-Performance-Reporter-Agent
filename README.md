# QE Performance Reporter Agent

Agente de QE especializado en el analisis de pruebas de performance. Su objetivo es tomar insumos crudos de Jira y JMeter, interpretar los resultados segun el tipo de prueba ejecutada y generar un reporte ejecutivo en HTML para facilitar la toma de decisiones tecnicas y de negocio.

## Objetivo

El agente ayuda a responder una pregunta central:

> La prueba de performance realizada cumple con los criterios esperados?

Para lograrlo, compara las expectativas definidas en Jira contra las metricas obtenidas en JMeter y produce un reporte final con resumen ejecutivo, contexto, metricas, hallazgos, cuellos de botella y recomendaciones.

## Estructura del proyecto

```text
qe-performances-reporter/
+-- AGENT.md
+-- README.md
+-- inputs/
+   +-- evidencias/
+-- outputs/
+-- skills/
    +-- skill-analisis-jira.md
    +-- skill-analisis-jmeter.md
    +-- skill-analisis-evidencias.md
    +-- skill-recomendaciones-monitoreo.md
    +-- skill-generar-reporte.md
    +-- skill-validacion-reporte.md
```

### Archivos principales

- `AGENT.md`: define el rol del agente, sus reglas globales y el flujo obligatorio de ejecucion.
- `inputs/`: carpeta donde se colocan los archivos de entrada, como XML de Jira y resultados de JMeter.
- `inputs/evidencias/`: carpeta opcional para imagenes de monitoreo, capturas de dashboards, logs, graficas APM o reportes complementarios.
- `outputs/`: carpeta donde se guarda el reporte HTML generado.
- `skills/skill-analisis-jira.md`: extrae la historia de usuario y los criterios de aceptacion desde Jira.
- `skills/skill-analisis-jmeter.md`: analiza las metricas de JMeter segun el tipo de prueba.
- `skills/skill-analisis-evidencias.md`: analiza evidencias complementarias cuando el usuario decide agregarlas.
- `skills/skill-recomendaciones-monitoreo.md`: recomienda metricas y agregaciones a exportar segun la herramienta de monitoreo antes de generar el reporte.
- `skills/skill-generar-reporte.md`: construye el reporte HTML final.
- `skills/skill-validacion-reporte.md`: revisa el HTML generado para asegurar claridad, consistencia, trazabilidad y completitud.

## Flujo de ejecucion

Cuando el usuario solicita generar un reporte, el agente sigue este flujo:

1. Pregunta obligatoriamente que tipo de prueba de performance se realizo, usando un bloque claro con ejemplos de archivos esperados.
2. Recibe una de las siguientes opciones:
   - `1`: Carga
   - `2`: Estres
   - `3`: Pico
   - `4`: Resistencia
3. Revisa si Jira o el usuario mencionan una herramienta de monitoreo y brinda recomendaciones de metricas, agregaciones y archivos antes de pedir evidencias.
4. Pregunta si el usuario desea agregar evidencias adicionales para enriquecer el analisis, mostrando ejemplos de capturas, logs y reportes complementarios.
5. Si el usuario responde que si, revisa `inputs/evidencias/` o las rutas indicadas por el usuario.
6. Revisa la carpeta `inputs/` para encontrar los archivos requeridos.
7. Analiza el XML de Jira para identificar la historia de usuario y criterios de aceptacion.
8. Analiza los resultados de JMeter y extrae metricas clave.
9. Analiza las evidencias complementarias cuando existan.
10. Compara expectativas contra resultados reales y evidencias disponibles.
11. Genera un reporte HTML en la carpeta `outputs/`.
12. Valida que el reporte sea entendible, consistente y trazable antes de finalizar.

## Tipos de prueba soportados

### 1. Prueba de carga

Evalua si el sistema cumple con los SLAs esperados bajo un volumen normal de usuarios. El foco esta en estabilidad, tiempos de respuesta y tasa de error dentro de los umbrales aceptados.

### 2. Prueba de estres

Busca identificar el punto de quiebre del sistema. El analisis se enfoca en determinar con cuantos usuarios el sistema degrada, colapsa o empieza a responder con errores como timeouts o codigos 500.

### 3. Prueba de pico

Evalua el comportamiento del sistema ante aumentos repentinos de trafico. El foco principal esta en la capacidad de absorber la rafaga y recuperar tiempos de respuesta normales.

### 4. Prueba de resistencia

Tambien conocida como prueba soak. Evalua el comportamiento del sistema durante un periodo prolongado para detectar degradacion progresiva, aumento sostenido de tiempos de respuesta o posibles memory leaks.

## Entradas esperadas

Coloca los archivos de entrada en la carpeta `inputs/`.

Archivos esperados:

- Un archivo XML exportado desde Jira, con la historia de usuario y criterios de aceptacion.
- Resultados de JMeter en formato HTML, CSV u otro archivo de resultados disponible.
- Opcionalmente, evidencias complementarias en `inputs/evidencias/`, como imagenes de monitoreo, capturas de dashboards, graficas APM, logs o reportes complementarios.

Ejemplos de archivos validos en `inputs/`:

```text
HU-1234.xml
jira-historia-pagos.xml
index.html
jmeter-dashboard.html
resultados.csv
resultado-prueba.jtl
```

Ejemplos de archivos validos en `inputs/evidencias/`:

```text
monitoring-api-pagos-cpu.png
grafana-memoria-pods.jpg
logs-timeouts-api-pagos.txt
apm-latencia-checkout.html
throughput-prueba-carga.csv
```

El agente no debe inventar metricas. Toda conclusion debe basarse estrictamente en los archivos disponibles dentro de `inputs/`.

Las evidencias complementarias ayudan a explicar el comportamiento observado, por ejemplo saturacion de CPU, consumo de memoria, errores en logs o degradacion visible en dashboards. Si una imagen no permite leer un valor con claridad, el agente debe marcarlo como no legible y no debe inventar la metrica.

## Recomendaciones de monitoreo

Antes de generar el reporte, el agente debe recomendar que evidencias exportar segun la herramienta de monitoreo indicada en Jira o por el usuario.

Para Azure Monitor, se recomienda exportar:

| Metrica | Agregacion recomendada | Archivo sugerido |
|---|---|---|
| Requests | Sum | `azure-requests.csv` |
| Response Time | Average | `azure-response-time-average.csv` |
| Response Time | Maximum | `azure-response-time-maximum.csv` |
| Http Server Errors / HTTP 5xx | Sum | `azure-http-5xx.csv` |
| CPU Time | Sum | `azure-cpu-time.csv` |
| Average Memory Working Set | Average | `azure-memory-average.csv` |
| Memory Working Set | Maximum | `azure-memory-maximum.csv` |
| Data In / Data Out | Sum | `azure-network.csv` |

Si existe Application Insights, tambien se recomienda exportar CSV desde Logs para requests por endpoint, failed requests, dependencies, failed dependencies, exceptions y traces.

## Salida generada

El reporte final se guarda en `outputs/` con el siguiente formato de nombre:

```text
reporte_[TIPO_DE_PRUEBA]_[FECHA].html
```

Ejemplo:

```text
reporte_CARGA_2026-09-19.html
```

## Contenido del reporte

El HTML generado debe incluir:

- Portada con tipo de prueba, fecha y fuentes analizadas.
- Resumen ejecutivo con resultado general, motivo principal, riesgo, recomendacion principal y nivel de confianza.
- Cards KPI con iconos grandes para las metricas principales.
- Graficos visuales por endpoint o transaccion cuando existan datos suficientes.
- Contexto de la prueba y descripcion de la historia de usuario evaluada.
- Criterios esperados extraidos de Jira.
- Cumplimiento de criterios de aceptacion extraidos del XML de Jira, con estado, evidencia, justificacion y recomendacion.
- Matriz visual de cumplimiento SLA.
- Analisis segun el tipo de prueba seleccionada.
- Galeria y analisis de evidencias complementarias, si fueron proporcionadas.
- Hallazgos principales separados en dato observado, interpretacion, impacto y recomendacion.
- Analisis de cuellos de botella.
- Riesgos tecnicos y de negocio.
- Recomendaciones tecnicas accionables.
- Limitaciones del analisis.
- Conclusion final ejecutiva.

## Estandar de consistencia

El reporte debe usar siempre los mismos estados para evitar interpretaciones distintas entre ejecuciones:

| Tipo | Valores permitidos |
|---|---|
| Estado de metrica | `Cumple`, `No cumple`, `Parcial`, `No definido en Jira`, `No determinado` |
| Resultado general | `Exitoso`, `Fallido`, `Parcial`, `No determinado` |
| Nivel de confianza | `Alto`, `Medio`, `Bajo` |

Si se usan colores en el HTML, deben ir acompanados por texto. El color no debe ser el unico indicador del estado.

## Trazabilidad de hallazgos

Cada hallazgo importante debe separar cuatro elementos:

| Campo | Descripcion |
|---|---|
| Dato observado | Valor exacto o senal encontrada, con fuente visible. |
| Interpretacion | Que significa frente al SLA, criterio esperado o tipo de prueba. |
| Impacto | Riesgo tecnico o de negocio. |
| Recomendacion | Accion concreta para corregir o validar el hallazgo. |

Las recomendaciones deben indicar prioridad, componente afectado, motivo, accion sugerida y beneficio esperado.

## Cumplimiento de criterios de aceptacion

El reporte debe evaluar cada criterio de aceptacion del XML de Jira. Esta seccion debe responder claramente que criterios cumplen, cuales no cumplen, cuales cumplen parcialmente y cuales no se pueden determinar por falta de datos.

La tabla debe incluir:

| Columna | Descripcion |
|---|---|
| Criterio | Identificador y nombre del criterio, por ejemplo `CA 01 - Objetivos de Performance para Carga`. |
| Esperado segun Jira | SLA, condicion o regla extraida del XML. |
| Evidencia encontrada | Metrica de JMeter, evidencia de monitoreo, log o dato usado para evaluar. |
| Estado | `Cumple`, `No cumple`, `Parcial` o `No determinado`. |
| Justificacion | Motivo breve basado en datos. |
| Recomendacion | Accion concreta asociada al criterio. |

Si no hay datos suficientes para evaluar un criterio, el agente debe marcarlo como `No determinado` y explicar que evidencia falta.

## Estandar visual del reporte

El reporte HTML debe ser ejecutivo, profesional y facil de leer. Debe abrirse directamente en navegador sin depender de internet, CDNs ni librerias externas.

Elementos visuales esperados:

| Elemento | Uso esperado |
|---|---|
| Hero principal | Mostrar titulo, tipo de prueba, resultado general, fecha, confianza y fuentes. |
| Cards KPI | Mostrar throughput, P90/P95, error rate, muestras, duracion o usuarios, con iconos grandes. |
| Graficos por endpoint | Barras horizontales para P90/P95, errores o throughput por transaccion. |
| Matriz SLA | Comparar esperado vs obtenido con estados visuales. |
| Galeria de evidencias | Mostrar imagenes de monitoreo, Aggregate Report o dashboards con caption. |
| Recomendaciones priorizadas | Tabla con prioridad, componente, motivo, accion y beneficio. |

Los graficos deben construirse con HTML/CSS usando datos reales de los archivos de entrada. Si no hay datos suficientes para un grafico, el agente debe declararlo en `Limitaciones del Analisis` en lugar de inventar valores.

## Estabilidad entre modelos

El framework esta disenado para reducir diferencias cuando se ejecuta con modelos distintos. Aun asi, si las instrucciones son demasiado abiertas, cada modelo puede interpretar el diseno, el orden de secciones o la redaccion de forma diferente.

Para controlar esto, `skills/skill-generar-reporte.md` define un contrato de salida estable. Ese contrato fija:

- Orden exacto de secciones.
- Nombres exactos de secciones.
- Estados permitidos.
- Reglas para decidir el resultado general.
- Orden de cards KPI.
- Orden deterministico de endpoints o transacciones.
- Reglas para no crear versiones alternativas del reporte si el usuario no las solicita.

`skills/skill-validacion-reporte.md` debe corregir el HTML si detecta que el modelo cambio esa estructura.

## Reglas del agente

- El agente siempre debe comunicarse en espanol.
- No debe iniciar el analisis sin consultar antes el tipo de prueba.
- Debe consultar si el usuario desea agregar evidencias adicionales antes de iniciar el analisis.
- Debe brindar recomendaciones de monitoreo antes de pedir evidencias, adaptadas a la herramienta detectada en Jira o indicada por el usuario.
- Debe mostrar preguntas claras, con saltos de linea, listas y ejemplos. No debe mostrar consultas largas en una sola linea.
- Debe indicar que el usuario responda solo con el numero de la opcion cuando presente alternativas.
- No debe inventar datos, metricas, SLAs ni conclusiones sin respaldo en los archivos de entrada.
- Debe usar los criterios de Jira como base para evaluar los resultados de JMeter.
- Debe evaluar cada criterio de aceptacion del XML de Jira e incluir estado, justificacion y recomendacion.
- Debe usar las evidencias complementarias como soporte contextual, sin reemplazar las metricas oficiales de Jira o JMeter.
- Debe declarar limitaciones cuando falten datos o una evidencia no sea legible.
- Debe generar un HTML visual y profesional con cards KPI, iconos grandes, graficos por endpoint/transaccion, matriz SLA y galeria de evidencias cuando existan datos.
- Debe respetar el contrato de salida estable para que el reporte mantenga estructura consistente entre modelos.
- Debe validar el HTML final antes de terminar el flujo.
- Debe guardar el reporte final directamente en `outputs/`.

## Ejemplo de uso

1. Agrega los archivos de Jira y JMeter dentro de `inputs/`.
2. Solicita al agente generar el reporte.
3. Selecciona el tipo de prueba cuando el agente lo solicite.
4. Indica si deseas agregar evidencias adicionales.
5. Si eliges agregar evidencias, coloca los archivos en `inputs/evidencias/`.
6. Revisa el HTML generado dentro de `outputs/`.

Mensaje sugerido:

```text
Genera el reporte de performance con los archivos disponibles en inputs.
```

El agente respondera solicitando el tipo de prueba antes de iniciar el analisis.

Ejemplo de primera consulta esperada:

```text
## Generacion de Reporte de Performance

Antes de iniciar el analisis necesito confirmar el tipo de prueba realizada.

Archivos esperados en /inputs:

- Jira XML: HU-1234.xml, jira-historia-pagos.xml
- JMeter HTML: index.html, jmeter-dashboard.html
- JMeter CSV/JTL: resultados.csv, resultado-prueba.jtl

Que tipo de prueba de performance realizaste?

1 - Carga
2 - Estres
3 - Pico
4 - Resistencia

Responde solo con el numero de la opcion.
```

Ejemplo de consulta de evidencias:

```text
## Evidencias Complementarias

Puedes agregar evidencias opcionales para enriquecer el diagnostico.

Ejemplos que puedes colocar en /inputs/evidencias:

- monitoring-api-pagos-cpu.png
- grafana-memoria-pods.jpg
- logs-timeouts-api-pagos.txt
- apm-latencia-checkout.html
- throughput-prueba-carga.csv

Deseas agregar evidencias adicionales?

1 - Si, agregare evidencias en inputs/evidencias
2 - No, continuar solo con Jira y JMeter

Responde solo con el numero de la opcion.
```

## Buenas practicas

- Verifica que el XML de Jira incluya criterios de aceptacion medibles.
- Incluye resultados completos de JMeter para mejorar la calidad del analisis.
- Incluye capturas de monitoreo con fecha, hora y componente visible para facilitar la correlacion.
- Usa nombres descriptivos para las evidencias, por ejemplo `monitoring-api-pagos-2026-09-19-1030.png`.
- Manten nombres de archivos descriptivos dentro de `inputs/`.
- Revisa que los SLAs esperados esten claramente definidos antes de ejecutar el reporte.

---
name: recomendaciones-monitoreo
description: Brinda recomendaciones de metricas, agregaciones y evidencias a exportar segun la herramienta de monitoreo usada antes de generar el reporte.
when_to_use: Usalo despues de conocer el tipo de prueba y antes de solicitar evidencias adicionales al usuario.
---

# Instrucciones de la Habilidad

1. Identifica la herramienta de monitoreo mencionada en Jira, en el mensaje del usuario o en los nombres de archivos disponibles.
2. Si no se identifica una herramienta, brinda una guia generica de observabilidad aplicable a cualquier tecnologia.
3. Muestra recomendaciones en formato claro, con titulo, lista de metricas y ejemplos de nombres de archivo para colocar en `/inputs/evidencias`.
4. No detengas el flujo para pedir confirmacion adicional. Luego de mostrar la guia, continua con la consulta de evidencias opcionales definida en `AGENT.md`.
5. Si la herramienta es **Azure Monitor**, recomienda exportar:
   - `Requests` con agregacion `Sum`.
   - `Response Time` con agregacion `Average` y, si esta disponible, tambien `Maximum`.
   - `Http Server Errors` o `HTTP 5xx` con agregacion `Sum`.
   - `CPU Time` con agregacion `Sum`.
   - `Average Memory Working Set` o `Memory Working Set` con agregacion `Average` y opcionalmente `Maximum`.
   - `Data In` y `Data Out` con agregacion `Sum`, si el trafico de red es relevante.
   - Dependencias, excepciones y trazas desde Application Insights o Logs si estan disponibles.
6. Si la herramienta es **Azure Monitor**, muestra esta guia:

   ## Recomendaciones para Azure Monitor

   Para que el reporte sea mas preciso, exporta metricas de la misma ventana de la prueba.

   Metricas recomendadas:

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

   Pasos sugeridos:

   1. Selecciona la misma ventana de tiempo de la prueba.
   2. En Azure Monitor > Metrics, agrega una metrica por grafico o exporta cada metrica por separado.
   3. Usa `Share` > `Download to Excel`.
   4. Abre el archivo en Excel y guardalo como CSV.
   5. Coloca los CSV en `/inputs/evidencias`.

   Si tienes Application Insights, usa `Drill into Logs` y exporta CSV de:

   - Requests por endpoint.
   - Failed requests por endpoint.
   - Dependencies duration.
   - Failed dependencies.
   - Exceptions.
   - Traces o logs de error.

7. Si la herramienta es **Grafana**, recomienda exportar CSV o capturas de:
   - Latencia promedio, P90/P95/P99.
   - Throughput o requests por segundo.
   - Error rate por endpoint.
   - CPU, memoria, red y saturacion.
   - Logs o traces correlacionados.
8. Si la herramienta es **Kibana / Elastic**, recomienda exportar CSV o capturas de:
   - Logs de error por timestamp.
   - HTTP status codes.
   - Latencia por endpoint.
   - Top errores y excepciones.
   - Correlation IDs si existen.
9. Si la herramienta es **Datadog, New Relic o AppDynamics**, recomienda exportar:
   - APM transactions por endpoint.
   - Error rate.
   - Latency percentiles.
   - Slow traces.
   - External dependencies.
   - Infra metrics de CPU, memoria y contenedores.
10. Si la herramienta es **CloudWatch**, recomienda exportar:
   - Request count.
   - Latency.
   - 4xx/5xx.
   - CPUUtilization.
   - MemoryUtilization si esta disponible.
   - Logs Insights con errores y excepciones.
11. Para cualquier tecnologia, indica que las evidencias deben cubrir la misma ventana temporal que JMeter:
   - 5 minutos antes de iniciar la prueba.
   - Toda la duracion de la prueba.
   - 5 minutos despues de finalizar.
12. Guarda mentalmente la herramienta detectada y las metricas recomendadas para considerarlas en las limitaciones del reporte si el usuario no proporciona esas evidencias.

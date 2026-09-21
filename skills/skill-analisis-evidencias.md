---
name: analisis-evidencias-complementarias
description: Analiza evidencias opcionales como imagenes de monitoreo, capturas de dashboards, logs, graficas APM o reportes complementarios.
when_to_use: Usalo cuando el usuario indique que desea agregar evidencias adicionales antes de generar el reporte de performance.
---

# Instrucciones de la Habilidad

1. Busca evidencias dentro de `/inputs/evidencias` y en cualquier ruta especifica indicada por el usuario.
2. Considera como evidencias validas archivos de imagen (`.png`, `.jpg`, `.jpeg`, `.webp`, `.bmp`), logs (`.log`, `.txt`), resultados tabulares (`.csv`) y reportes complementarios (`.html`, `.pdf` cuando sean legibles). Ignora archivos de documentacion interna como `README.md`.
3. Para imagenes o capturas de monitoreo, identifica solo informacion visible y legible:
   - Herramienta o dashboard observado, si aparece.
   - Rango horario, fecha o ventana de monitoreo, si aparece.
   - Servicio, endpoint, host, pod, contenedor o componente afectado, si aparece.
   - Senales de CPU, memoria, red, base de datos, latencia, throughput, tasa de error, saturacion, alertas o picos visibles.
4. Para logs o archivos de texto, identifica errores, timeouts, codigos HTTP, excepciones, mensajes de saturacion y marcas de tiempo relevantes.
5. Correlaciona las evidencias con el tipo de prueba seleccionado y con los resultados de JMeter:
   - En Carga, busca senales de incumplimiento de SLA o inestabilidad bajo trafico esperado.
   - En Estres, busca indicios del punto de quiebre, saturacion o errores durante la degradacion.
   - En Pico, busca el momento de la rafaga y la recuperacion posterior.
   - En Resistencia, busca degradacion progresiva, crecimiento de memoria, agotamiento de recursos o errores acumulados.
6. No inventes metricas desde una imagen poco legible. Si un valor no puede leerse con claridad, registralo como "no legible" o "no determinado".
7. Usa las evidencias como soporte del diagnostico. No reemplaces las metricas oficiales de Jira o JMeter salvo que la evidencia contenga un dato exacto, visible y confiable.
8. Normaliza cada evidencia en una tabla temporal con esta estructura:
   - `evidencia`
   - `tipo`
   - `ventana_horaria`
   - `zona_horaria`
   - `hallazgo`
   - `relacion_con_jmeter`
   - `limitacion`
   - `fuente`
9. Si una evidencia contradice una conclusion basada en JMeter, no descartes ninguno de los datos. Registralo como una inconsistencia y agregalo a las limitaciones del analisis.
10. Si una captura, CSV o log de monitoreo muestra una hora distinta a JMeter, tratala como posible diferencia de zona horaria, desfase de reloj, ventana de monitoreo ampliada o evidencia tomada fuera del periodo exacto de prueba. No ajustes horas sin evidencia.
11. Guarda temporalmente estos resultados para el reporte final:
   - Lista de evidencias revisadas.
   - Observaciones relevantes.
   - Ventanas horarias detectadas y zona horaria si aparece.
   - Correlacion con metricas de JMeter.
   - Limitaciones de lectura o calidad de la evidencia.
   - Aporte de las evidencias al nivel de confianza del analisis.

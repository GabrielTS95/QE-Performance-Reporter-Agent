---
name: analisis-jmeter
description: Extrae metricas clave de JMeter y las analiza segun el tipo de prueba seleccionado.
when_to_use: Usalo despues de conocer los criterios de aceptacion y el tipo de prueba seleccionado por el usuario.
---

# Instrucciones de la Habilidad

1. Busca en la carpeta `/inputs` los archivos de resultados de JMeter en formato `.html`, `.csv`, `.jtl` u otro formato legible disponible.
2. Extrae las metricas base disponibles:
   - Throughput.
   - Tiempo de respuesta promedio.
   - Percentil 90.
   - Percentil 95.
   - Percentil 99, si existe.
   - Tasa de error.
   - Minimo y maximo, si existen.
   - Usuarios, hilos, concurrencia o ramp-up, si existen.
   - Duracion de la prueba, si existe.
   - Endpoints, transacciones o samples con mayor latencia o mayor error, si existen.
3. Extrae y normaliza la ventana temporal de ejecucion de la prueba. Guarda estos campos temporales:
   - `inicio_ejecucion`
   - `fin_ejecucion`
   - `duracion`
   - `zona_horaria`
   - `fuente_temporal`
   - `nivel_confianza_temporal`: `Alto`, `Medio` o `Bajo`.
4. Usa esta prioridad para calcular `inicio_ejecucion` y `fin_ejecucion`:
   - Primero, usa timestamps crudos de JMeter en `.jtl` o `.csv` si existen. El inicio debe ser el timestamp mas antiguo. El fin debe ser el timestamp mas reciente mas el tiempo transcurrido del sample cuando ese dato exista.
   - Segundo, usa datos temporales visibles en el dashboard HTML de JMeter solo si el reporte muestra inicio, fin o duracion de la ejecucion.
   - Tercero, usa una ventana de ejecucion declarada explicitamente en Jira solo si el campo corresponde a la ejecucion de la prueba, no a la creacion o actualizacion del ticket.
   - Cuarto, usa evidencias de monitoreo solo como soporte de correlacion, no como reemplazo automatico de JMeter.
5. No uses la fecha de generacion del reporte, el nombre del archivo, la fecha de modificacion del archivo ni la fecha de Jira como inicio o fin de ejecucion.
6. Si solo existe duracion pero no existe inicio verificable, reporta la duracion y marca inicio/fin como `No determinado`.
7. Si las fuentes usan zonas horarias diferentes, no fuerces la coincidencia. Declara la zona horaria de cada fuente si esta disponible. Si no aparece, usa `Zona horaria no indicada` y agregalo a limitaciones.
8. Normaliza las metricas en una tabla temporal con esta estructura:
   - `metrica`
   - `esperado`
   - `obtenido`
   - `estado`
   - `fuente`
   - `observacion`
9. Usa solamente estos estados para evaluar metricas:
   - `Cumple`
   - `No cumple`
   - `Parcial`
   - `No definido en Jira`
   - `No determinado`
10. Si Jira no define un valor esperado, usa `No definido en Jira` y evita declarar cumplimiento o incumplimiento.
11. Si JMeter no contiene una metrica necesaria, usa `No determinado` y explica que falta el dato.
12. Ajusta el enfoque analitico segun el tipo de prueba seleccionado:
   - **Carga:** Evalua cumplimiento de SLAs, estabilidad general, tasa de error y comportamiento bajo trafico esperado.
   - **Estres:** Identifica punto de quiebre, degradacion progresiva, errores dominantes y capacidad maxima observada antes del colapso.
   - **Pico:** Evalua comportamiento durante la rafaga, impacto en latencia/error y tiempo de recuperacion posterior.
   - **Resistencia:** Evalua degradacion a lo largo del tiempo, crecimiento sostenido de latencia, errores acumulados y posibles fugas de memoria si hay evidencia.
13. Para cada hallazgo relevante, registra cuatro campos separados:
   - `dato_observado`: metrica exacta encontrada y su fuente.
   - `interpretacion`: que significa frente al criterio esperado o al tipo de prueba.
   - `impacto`: riesgo tecnico o de negocio.
   - `recomendacion`: accion concreta sugerida.
14. Evalua cada criterio de aceptacion extraido desde Jira contra las metricas disponibles de JMeter y las evidencias complementarias. Genera una matriz temporal con esta estructura:
   - `id_criterio`
   - `criterio`
   - `estado`: usa solo `Cumple`, `No cumple`, `Parcial` o `No determinado`.
   - `evidencia`: metrica, archivo o dato que respalda el estado.
   - `justificacion`: motivo breve y claro del estado.
   - `recomendacion`: accion concreta asociada a ese criterio.
   - `fuente`: Jira, JMeter, evidencia complementaria o combinacion de fuentes.
15. Reglas para evaluar criterios de aceptacion:
   - Usa `Cumple` solo si existe evidencia suficiente y todos los valores relacionados estan dentro del criterio esperado.
   - Usa `No cumple` si una metrica relacionada supera el SLA, hay errores relevantes o la condicion esperada no se sostiene.
   - Usa `Parcial` si una parte del criterio cumple, pero otra no, o si hay evidencia mixta.
   - Usa `No determinado` si faltan datos suficientes para evaluar el criterio.
   - No marques como `Cumple` un criterio funcional bajo carga si solo validaste performance y no hay evidencia funcional del comportamiento.
16. Determina el resultado general de la prueba usando solo estos valores:
   - `Exitoso`
   - `Fallido`
   - `Parcial`
   - `No determinado`
17. Calcula un nivel de confianza del analisis:
   - `Alto`: Jira tiene criterios medibles, JMeter contiene metricas completas y las evidencias adicionales confirman los hallazgos principales.
   - `Medio`: Jira y JMeter permiten concluir, pero faltan evidencias de infraestructura, desglose por endpoint o algun criterio secundario.
   - `Bajo`: faltan criterios, metricas clave o los archivos disponibles no permiten confirmar la causa.
18. Guarda temporalmente las metricas, ventana temporal de ejecucion, hallazgos, matriz de criterios de aceptacion, resultado general, riesgos y nivel de confianza para el reporte final.

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
3. Normaliza las metricas en una tabla temporal con esta estructura:
   - `metrica`
   - `esperado`
   - `obtenido`
   - `estado`
   - `fuente`
   - `observacion`
4. Usa solamente estos estados para evaluar metricas:
   - `Cumple`
   - `No cumple`
   - `Parcial`
   - `No definido en Jira`
   - `No determinado`
5. Si Jira no define un valor esperado, usa `No definido en Jira` y evita declarar cumplimiento o incumplimiento.
6. Si JMeter no contiene una metrica necesaria, usa `No determinado` y explica que falta el dato.
7. Ajusta el enfoque analitico segun el tipo de prueba seleccionado:
   - **Carga:** Evalua cumplimiento de SLAs, estabilidad general, tasa de error y comportamiento bajo trafico esperado.
   - **Estres:** Identifica punto de quiebre, degradacion progresiva, errores dominantes y capacidad maxima observada antes del colapso.
   - **Pico:** Evalua comportamiento durante la rafaga, impacto en latencia/error y tiempo de recuperacion posterior.
   - **Resistencia:** Evalua degradacion a lo largo del tiempo, crecimiento sostenido de latencia, errores acumulados y posibles fugas de memoria si hay evidencia.
8. Para cada hallazgo relevante, registra cuatro campos separados:
   - `dato_observado`: metrica exacta encontrada y su fuente.
   - `interpretacion`: que significa frente al criterio esperado o al tipo de prueba.
   - `impacto`: riesgo tecnico o de negocio.
   - `recomendacion`: accion concreta sugerida.
9. Determina el resultado general de la prueba usando solo estos valores:
   - `Exitoso`
   - `Fallido`
   - `Parcial`
   - `No determinado`
10. Calcula un nivel de confianza del analisis:
   - `Alto`: Jira tiene criterios medibles, JMeter contiene metricas completas y las evidencias adicionales confirman los hallazgos principales.
   - `Medio`: Jira y JMeter permiten concluir, pero faltan evidencias de infraestructura, desglose por endpoint o algun criterio secundario.
   - `Bajo`: faltan criterios, metricas clave o los archivos disponibles no permiten confirmar la causa.
11. Guarda temporalmente las metricas, hallazgos, resultado general, riesgos y nivel de confianza para el reporte final.

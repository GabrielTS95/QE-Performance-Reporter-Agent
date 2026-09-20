---
name: analisis-jira
description: Extrae la historia de usuario, criterios de aceptacion y expectativas medibles desde un archivo XML de Jira.
when_to_use: Usalo como primer paso para entender que se esperaba de la prueba de performance.
---

# Instrucciones de la Habilidad

1. Busca en la carpeta `/inputs` cualquier archivo con extension `.xml`.
2. Analiza el contenido y extrae especificamente:
   - Titulo, resumen o identificador de la historia de usuario.
   - Descripcion funcional del flujo probado.
   - Criterios de aceptacion.
   - SLAs o expectativas medibles, como tiempo promedio, percentil 90, percentil 95, tasa de error, throughput, concurrencia maxima, duracion esperada o ventana de prueba.
   - Riesgos, restricciones o notas funcionales relevantes para interpretar el resultado.
3. Normaliza los criterios esperados en una tabla temporal con esta estructura:
   - `criterio`
   - `valor_esperado`
   - `unidad`
   - `fuente`
   - `observacion`
4. Si un criterio no existe en Jira, no lo inventes. Registralo como `No definido en Jira` cuando sea necesario compararlo en el reporte.
5. Si un criterio es ambiguo o no medible, registralo como `No determinado` y explica brevemente la limitacion.
6. Guarda esta informacion temporalmente para que el analisis de JMeter y el reporte final puedan comparar expectativas contra resultados reales.

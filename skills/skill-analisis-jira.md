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
4. Extrae cada criterio de aceptacion de Jira como un item evaluable independiente. Si Jira usa etiquetas como `CA 01`, `CA 02`, `CA 03`, conserva ese identificador.
5. Normaliza los criterios de aceptacion en una tabla temporal con esta estructura:
   - `id_criterio`: ejemplo `CA 01`.
   - `criterio`: nombre o resumen corto del criterio.
   - `descripcion`: texto funcional resumido.
   - `expectativa_medible`: SLA, regla o condicion verificable extraida de Jira.
   - `tipo`: `Performance`, `Funcional bajo carga`, `Monitoreo`, `Roles/Permisos`, `Estabilidad` u otro tipo claro.
   - `fuente`: seccion exacta del XML o nombre del custom field.
6. Si un criterio no tiene una expectativa medible, no lo inventes. Registralo como `No determinado` y explica que falta un dato verificable.
7. Si un criterio no existe en Jira, no lo inventes. Registralo como `No definido en Jira` solo cuando sea necesario compararlo en el reporte.
8. Guarda esta informacion temporalmente para que el analisis de JMeter y el reporte final puedan comparar expectativas contra resultados reales.

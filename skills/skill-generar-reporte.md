---
name: generar-reporte-html
description: Construye un reporte HTML profesional, consistente y trazable con base en el analisis previo.
when_to_use: Usalo como paso final despues de comparar Jira, JMeter y las evidencias complementarias disponibles.
---

# Instrucciones de la Habilidad

1. Crea un documento HTML5 estructurado, usando CSS embebido limpio, corporativo, visual y legible. El reporte debe abrir directamente en navegador sin depender de internet ni librerias externas.
2. El diseno debe ser ejecutivo y amigable:
   - Usa un **hero principal** con titulo, tipo de prueba, resultado general, fecha, nivel de confianza y fuentes analizadas.
   - Usa **cards KPI grandes** para las metricas mas importantes, con iconos SVG inline grandes y texto corto.
   - Usa una **navegacion interna** por secciones para facilitar la lectura.
   - Usa **graficos HTML/CSS estaticos** cuando existan datos suficientes. No dependas de Chart.js, CDNs ni librerias externas.
   - Usa una **galeria visual de evidencias** si existen imagenes en `/inputs/evidencias`.
   - Usa tablas solo cuando la comparacion sea mas clara que un card o grafico.
3. Usa una estructura fija para todos los reportes:
   - **Portada/Hero:** nombre del reporte, tipo de prueba, fecha de generacion, resultado general, nivel de confianza y fuentes analizadas.
   - **Resumen Ejecutivo:** resultado general (`Exitoso`, `Fallido`, `Parcial` o `No determinado`), motivo principal, riesgo para negocio o produccion, recomendacion principal y nivel de confianza.
   - **Contexto de la Prueba:** historia de usuario, flujo evaluado, objetivo de la prueba, duracion, concurrencia y alcance cuando existan datos.
   - **Indicadores Clave:** cards KPI con iconos grandes para throughput, P90/P95, error rate, muestras, duracion o usuarios concurrentes cuando existan.
   - **Graficos por Endpoint o Transaccion:** barras horizontales para P90/P95, tasa de error, throughput o samples por endpoint/transaccion. Incluye referencias de SLA cuando existan.
   - **Criterios Esperados:** tabla con criterios extraidos de Jira. Si un criterio no existe, mostrar `No definido en Jira`.
   - **Matriz de Cumplimiento SLA:** cards o tabla visual con `Metrica`, `Esperado`, `Obtenido`, `Estado`, `Fuente` y `Observacion`.
   - **Analisis Segun Tipo de Prueba:** explica el resultado con el foco correspondiente a Carga, Estres, Pico o Resistencia.
   - **Evidencias Complementarias:** incluye esta seccion solo si el usuario agrego evidencias validas. Muestra una galeria de imagenes y una tabla con `Evidencia`, `Hallazgo`, `Relacion con JMeter` y `Limitacion`.
   - **Hallazgos Principales:** separa cada hallazgo en `Dato observado`, `Interpretacion`, `Impacto` y `Recomendacion`.
   - **Analisis de Cuellos de Botella:** identifica endpoints, componentes, recursos o patrones de degradacion que expliquen el resultado.
   - **Riesgos:** describe riesgos tecnicos y de negocio derivados de los hallazgos.
   - **Recomendaciones Accionables:** usa una tabla con `Prioridad`, `Componente`, `Motivo`, `Accion sugerida` y `Beneficio esperado`.
   - **Limitaciones del Analisis:** lista datos faltantes, criterios ambiguos, evidencias no legibles o restricciones de los archivos.
   - **Conclusion Final:** decision ejecutiva breve y consistente con las metricas.
4. Los graficos recomendados son:
   - **Top transacciones por P90 o P95:** barras horizontales ordenadas de mayor a menor.
   - **Concentracion de errores:** barras horizontales con porcentaje de error por endpoint/transaccion.
   - **Cumplimiento de throughput:** barra o card comparando TPS obtenido contra TPS esperado.
   - **Matriz SLA:** cards por criterio con estado visual.
   - **Evidencias:** galeria de capturas con caption y limitacion cuando aplique.
5. Si no existe suficiente informacion para un grafico, no inventes datos. En su lugar, muestra una nota en `Limitaciones del Analisis`.
6. Usa iconos SVG inline dentro de los cards KPI y callouts. Los iconos deben ser grandes, claros y consistentes visualmente.
7. Usa solamente estos estados para metricas:
   - `Cumple`
   - `No cumple`
   - `Parcial`
   - `No definido en Jira`
   - `No determinado`
8. Usa solamente estos resultados generales:
   - `Exitoso`
   - `Fallido`
   - `Parcial`
   - `No determinado`
9. Si usas colores, acompanialos siempre con texto:
   - Verde para `Cumple` o `Exitoso`.
   - Rojo para `No cumple` o `Fallido`.
   - Amarillo para `Parcial`.
   - Gris para `No definido en Jira` o `No determinado`.
10. No agregues secciones vacias. Si no hay informacion para una seccion obligatoria, incluye una nota breve en `Limitaciones del Analisis`.
11. No inventes metricas, SLAs, fechas, endpoints ni causas. Toda afirmacion debe tener una fuente: Jira, JMeter o evidencia complementaria.
12. Las recomendaciones deben ser especificas. Evita frases genericas como "optimizar el sistema" sin componente, motivo y accion.
13. Si existen imagenes de evidencia en `/inputs/evidencias`, referencialas en el HTML con rutas relativas desde `/outputs`, por ejemplo `../inputs/evidencias/nombre-imagen.png`. Si el nombre tiene espacios, usa URL encoding en el atributo `src`, por ejemplo `imagen%20(2).png`.
14. El reporte debe ser responsive:
   - En escritorio, usa grillas para KPI cards, graficos y evidencias.
   - En movil, las secciones deben apilarse sin romper el contenido.
   - Las barras y tablas deben conservar legibilidad basica.
15. Genera el codigo fuente y guardalo directamente en la carpeta `/outputs`.

---
name: validacion-reporte
description: Revisa el reporte HTML generado para asegurar claridad, consistencia, trazabilidad y completitud antes de finalizar.
when_to_use: Usalo despues de generar el reporte HTML y antes de dar por terminado el flujo.
---

# Instrucciones de la Habilidad

1. Abre el reporte HTML generado en `/outputs`.
2. Valida que el reporte sea entendible para negocio y tecnologia:
   - El resumen ejecutivo indica resultado, motivo, riesgo, recomendacion principal y nivel de confianza.
   - La conclusion final coincide con las metricas y hallazgos.
   - Las recomendaciones son accionables y tienen componente, motivo, accion y beneficio.
   - El reporte tiene diseno ejecutivo, visual y amigable.
   - Existen cards KPI con iconos grandes para las metricas principales cuando hay datos suficientes.
   - Existen graficos por endpoint/transaccion cuando hay datos suficientes para construirlos.
   - Las evidencias visuales se muestran como galeria si existen imagenes validas.
   - Existe una seccion de cumplimiento de criterios de aceptacion extraidos de Jira.
   - Cada criterio de aceptacion tiene estado, evidencia, justificacion y recomendacion.
   - El reporte respeta el contrato de salida estable definido en `skills/skill-generar-reporte.md`.
3. Valida consistencia:
   - Los estados de metricas pertenecen solo a `Cumple`, `No cumple`, `Parcial`, `No definido en Jira` o `No determinado`.
   - El resultado general pertenece solo a `Exitoso`, `Fallido`, `Parcial` o `No determinado`.
   - Los colores, si existen, siempre estan acompanados por texto.
4. Valida trazabilidad:
   - Toda metrica o conclusion tiene fuente visible: Jira, JMeter o evidencia complementaria.
   - No hay SLAs, endpoints, valores, fechas ni causas inventadas.
   - Las evidencias complementarias tienen hallazgo, relacion con JMeter y limitacion cuando aplique.
   - Cada criterio de aceptacion evaluado referencia una evidencia o declara claramente que no hay datos suficientes.
5. Valida completitud:
   - No existen secciones vacias.
   - Si faltan datos, estan declarados en `Limitaciones del Analisis`.
   - Si no hubo evidencias adicionales, no aparece una seccion vacia de evidencias.
   - Los criterios de aceptacion del XML de Jira no quedan sin evaluacion. Si alguno no puede evaluarse, debe figurar como `No determinado`.
6. Valida presentacion visual:
   - El HTML incluye CSS embebido y no depende de internet, CDNs ni librerias externas.
   - El hero principal muestra resultado general de forma visible.
   - Los graficos usan datos reales y no inventados.
   - Las rutas de imagenes de evidencia funcionan desde `/outputs`.
   - El reporte es legible en escritorio y no se rompe en movil.
7. Valida estabilidad entre modelos:
   - Las secciones principales usan exactamente los titulos definidos en el contrato de salida estable.
   - El orden de secciones coincide con el contrato de salida estable.
   - Las cards KPI mantienen el orden definido cuando existen datos.
   - Los endpoints o transacciones estan ordenados por las reglas deterministicas del contrato.
   - No aparecen secciones alternativas no solicitadas como `Version gerencial`, `Version tecnica`, `Detalle extendido` o similares.
   - La respuesta final no ofrece crear una version alternativa del reporte, salvo solicitud explicita del usuario.
8. Si encuentras inconsistencias, corrige el HTML antes de finalizar.
9. Si no puedes corregir una inconsistencia por falta de datos, deja una nota clara en `Limitaciones del Analisis`.

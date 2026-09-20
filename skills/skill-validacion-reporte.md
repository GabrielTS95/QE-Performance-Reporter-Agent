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
3. Valida consistencia:
   - Los estados de metricas pertenecen solo a `Cumple`, `No cumple`, `Parcial`, `No definido en Jira` o `No determinado`.
   - El resultado general pertenece solo a `Exitoso`, `Fallido`, `Parcial` o `No determinado`.
   - Los colores, si existen, siempre estan acompanados por texto.
4. Valida trazabilidad:
   - Toda metrica o conclusion tiene fuente visible: Jira, JMeter o evidencia complementaria.
   - No hay SLAs, endpoints, valores, fechas ni causas inventadas.
   - Las evidencias complementarias tienen hallazgo, relacion con JMeter y limitacion cuando aplique.
5. Valida completitud:
   - No existen secciones vacias.
   - Si faltan datos, estan declarados en `Limitaciones del Analisis`.
   - Si no hubo evidencias adicionales, no aparece una seccion vacia de evidencias.
6. Valida presentacion visual:
   - El HTML incluye CSS embebido y no depende de internet, CDNs ni librerias externas.
   - El hero principal muestra resultado general de forma visible.
   - Los graficos usan datos reales y no inventados.
   - Las rutas de imagenes de evidencia funcionan desde `/outputs`.
   - El reporte es legible en escritorio y no se rompe en movil.
7. Si encuentras inconsistencias, corrige el HTML antes de finalizar.
8. Si no puedes corregir una inconsistencia por falta de datos, deja una nota clara en `Limitaciones del Analisis`.

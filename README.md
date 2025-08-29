# Informe de Seguridad — Resumen General

## Resumen General
- Objetivo: proporcionar una visión ejecutiva de riesgos y medidas en el entorno local (El Campello/Alicante) con foco en condiciones ambientales, orden público, ciber/tecnología y movilidad.
- Alcance: activos de presentación (`mapa_trafico.html`, `qr_mapa.html`) y observación de entorno urbano.
- Conclusiones clave: requiere monitorización continua de clima/playas (AEMET), eventos de orden público, disponibilidad tecnológica y situación vial; se recomiendan canales oficiales y despliegue de evidencias en informe.

## AEMET / Playas
- Variables críticas: avisos meteorológicos, viento, oleaje, temperaturas, radiación UV, estado de playas y banderas.
- Acciones:
  - Consultar avisos AEMET (viento/lluvia/temperaturas) y estado de costa antes de eventos.
  - Incluir capturas/tabla de alertas del día y previsión 48–72h.
  - Medidas preventivas por nivel de aviso (verde/amarillo/naranja/rojo).
- Integración técnica (opcional): usar API abierta de AEMET para avisos/municipios y panel de playas municipal si existe.

## Criminalidad
- Enfoque: tendencias de hurtos, robos con fuerza, vandalismo y delitos contra patrimonio en la zona.
- Acciones:
  - Recopilar indicadores oficiales (p. ej., Ministerio del Interior / Policía Local) y variación interanual.
  - Mapear zonas calientes si hay datos geográficos disponibles (respetando privacidad y agregación).
  - Plan de mitigación: refuerzo en horarios/lugares críticos, sensibilización y medidas físicas.

## Disturbios Civiles
- Alcance: manifestaciones, protestas, aglomeraciones y eventos que puedan afectar a la seguridad/operaciones.
- Acciones:
  - Monitorizar convocatorias públicas autorizadas y comunicación municipal.
  - Evaluar impacto en rutas de acceso, horarios y necesidad de desvíos.
  - Definir umbrales de escalado y canales de coordinación con autoridades.

## Incidencias Tecnológicas
- Vectores: caídas de servicios (energía/ISP), fallos de aplicaciones críticas, ciberincidentes con impacto operativo.
- Acciones:
  - Vigilar páginas de estado de proveedores y configurar alertas.
  - Plan de continuidad: contactos, procedimientos de contingencia y tiempos objetivo de recuperación.
  - Registro de incidentes y lecciones aprendidas.

## Tráfico y Aparcamiento
- Situación actual: visualización con tráfico en tiempo real en `mapa_trafico.html` (El Campello por defecto), búsqueda y geolocalización opcional.
- Acciones:
  - Añadir puntos de interés (parkings, paradas, cortes previstos) y rutas alternativas.
  - Capturas para el informe en horas pico/valle y enlace/QR (`qr_mapa.html`) para acceso móvil.
  - Si es posible, integrar información municipal de ocupación de aparcamientos.

## Fuentes
- Meteorología y costa: AEMET (avisos y predicción), boletines municipales y salvamento en playas.
- Seguridad ciudadana: Ministerio del Interior, Policía Local, comunicaciones oficiales de la Subdelegación del Gobierno.
- Orden público/eventos: bandos municipales, agenda de eventos, autorizaciones de manifestaciones.
- Tecnología: páginas de estado de proveedores (energía, ISP, cloud) y CERT/INCIBE.
- Movilidad: DGT, Ayuntamiento (tráfico local, obras), operadoras de parking y transporte público.

---

Notas operativas
- Evidencias: incorpora en el informe capturas del mapa de tráfico, QR funcional, y tablas/resúmenes de cada apartado con fecha/hora y fuente.
- Privacidad y legalidad: usar datos agregados/anonimizados y citar fuentes oficiales; revisar RGPD si se tratan datos personales.

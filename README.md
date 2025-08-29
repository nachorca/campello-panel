# Informe de Seguridad — Mapa de Tráfico y QR

## 1) Alcance y Contexto
- Repositorio actual con dos páginas estáticas:
  - `mapa_trafico.html`: Mapa de Google Maps con capa de tráfico, búsqueda por geocodificación y geolocalización bajo demanda.
  - `qr_mapa.html`: Generador de QR que apunta a la URL donde se sirve el mapa.
- Uso previsto: informe/presentación con acceso interactivo (en local o publicado) y QR para facilitar el acceso móvil.

## 2) Activos y Datos Procesados
- **Clave de API**: Google Maps JavaScript API y Geocoding API.
- **Datos de ubicación**:
  - Coordenadas iniciales (El Campello, Alicante).
  - Geocodificación de direcciones introducidas por el usuario (a través de Maps JS).
  - Geolocalización del usuario (solo si pulsa “Mi ubicación”).
- **Direcciones URL**: para el QR (públicas si se publica el mapa).

## 3) Dependencias Externas
- `https://maps.googleapis.com` y `https://maps.gstatic.com` (Google Maps JS y recursos).
- `https://chart.googleapis.com` (generación de PNG del QR).

## 4) Riesgos Principales
- **Exposición/abuso de la API key**: uso fuera de tus dominios y consumo de cuota/costos.
- **Restricciones de origen (referrer) insuficientes**: rechazo en producción o fuga si están mal configuradas.
- **Privacidad del usuario**: solicitud de geolocalización y tratamiento de datos personales de ubicación.
- **Clickjacking o contenido embebido malicioso**: si se inserta el mapa en sitios no controlados.
- **Cargas de scripts externas**: riesgo de inyección si no se limita el origen.
- **Disponibilidad**: dependencia de servicios externos (Google APIs) y conectividad.

## 5) Controles Existentes en las Páginas
- Geolocalización solo on-demand (botón “Mi ubicación”).
- Sin almacenamiento de datos personales en backend (páginas estáticas).
- Interfaz minimalista sin formularios persistentes ni cookies propias.

## 6) Medidas Recomendadas (Prioridad Alta)
- **Restringir la API key** en Google Cloud:
  - Restricciones de aplicación: “HTTP referrers (sitios web)”.
  - Permitir: `http://localhost:5500/*`, `http://TU_IP_LOCAL:5500/*` (si presentas por LAN), y tu dominio público (p. ej. `https://tudominio.com/*`).
  - Restricciones de API: limitar a “Maps JavaScript API” y “Geocoding API”.
- **Publicar bajo HTTPS** cuando sea público (p. ej. GitHub Pages, Vercel) y actualizar referrers.
- **Aviso de privacidad** visible: explicar que el mapa usa Google Maps, y que “Mi ubicación” solo se usa en el navegador para centrar el mapa, sin enviarse a servidores propios.
- **No incluir la API key en informes**: si compartes el HTML, comparte la versión ya alojada y con key restringida por dominio.

## 7) Endurecimiento (Hardening) Recomendado
- Encabezados HTTP (configurar en el servidor; ejemplos abajo):
  - `Content-Security-Policy` (CSP) ajustado a Google Maps y Chart API.
  - `Permissions-Policy: geolocation=(self)` para limitar quién puede solicitar geolocalización.
  - `X-Frame-Options: SAMEORIGIN` o CSP `frame-ancestors 'self'` para prevenir clickjacking.
  - `Referrer-Policy: strict-origin-when-cross-origin` (evita fuga de rutas completas); ojo: si bloqueas referrer, las restricciones por referrer de Google podrían fallar.
- CSP sugerida (mínima) si mantienes JS/CSS en línea:
  ```
  Content-Security-Policy: \
    default-src 'self'; \
    script-src 'self' 'unsafe-inline' https://maps.googleapis.com https://maps.gstatic.com https://chart.googleapis.com; \
    style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; \
    img-src 'self' data: https://maps.googleapis.com https://maps.gstatic.com https://chart.googleapis.com; \
    connect-src 'self' https://maps.googleapis.com https://maps.gstatic.com; \
    frame-ancestors 'self';
  ```
  - Ideal: mover JS inline a archivos `.js` y usar `script-src 'self' https://maps.googleapis.com https://maps.gstatic.com https://chart.googleapis.com` con `nonce` o sin `unsafe-inline`.
- Revisar el uso de `chart.googleapis.com`: si prefieres cero dependencias, sustituir por una librería QR local (ej. `QRCode.js`).

## 8) Flujo de Publicación Seguro
1. Crear clave en Google Cloud, habilitar APIs y restringir (referrers + APIs).
2. Publicar `mapa_trafico.html` y `qr_mapa.html` en un hosting estático con HTTPS.
3. Configurar encabezados de seguridad en el servidor (CSP, Permissions‑Policy, Frame‑Ancestors).
4. Probar desde un navegador en modo incógnito y un móvil externo (4G) para validar accesos y restricciones.

## 9) Lista de Verificación (Checklist)
- [ ] La API key funciona en `localhost` y dominio público.
- [ ] Referrers exactos incluidos (con `/*`) y solo las APIs necesarias.
- [ ] Geolocalización solo bajo acción del usuario y con mensaje claro.
- [ ] Encabezados CSP/Permissions-Policy/Frame-Ancestors aplicados en producción.
- [ ] El QR abre el mapa correcto (LAN o dominio público) y carga bajo HTTPS.
- [ ] Sin errores en consola del navegador ni bloqueos CSP inesperados.

## 10) Incidencias y Respuesta
- Si detectas uso anómalo de la clave: rotar la key, endurecer referrers y revisar registros de uso.
- Configurar alertas de cuota/costos en Google Cloud Billing.

## 11) Configuraciones de Servidor — Ejemplos
### NGINX
```
add_header Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline' https://maps.googleapis.com https://maps.gstatic.com https://chart.googleapis.com; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; img-src 'self' data: https://maps.googleapis.com https://maps.gstatic.com https://chart.googleapis.com; connect-src 'self' https://maps.googleapis.com https://maps.gstatic.com; frame-ancestors 'self'" always;
add_header Permissions-Policy "geolocation=(self)" always;
add_header X-Frame-Options "SAMEORIGIN" always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
```

### Apache (.htaccess)
```
<IfModule mod_headers.c>
  Header always set Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline' https://maps.googleapis.com https://maps.gstatic.com https://chart.googleapis.com; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; img-src 'self' data: https://maps.googleapis.com https://maps.gstatic.com https://chart.googleapis.com; connect-src 'self' https://maps.googleapis.com https://maps.gstatic.com; frame-ancestors 'self'"
  Header always set Permissions-Policy "geolocation=(self)"
  Header always set X-Frame-Options "SAMEORIGIN"
  Header always set Referrer-Policy "strict-origin-when-cross-origin"
</IfModule>
```

## 12) Notas de Cumplimiento/Privacidad
- Indicar en el informe que se usa Google Maps (proveedor subencargado) y enlazar su política de privacidad.
- No se persisten datos personales; la geolocalización se procesa en el navegador. Si añades telemetría futura, evalúa DPIA/AVPD según reglamento aplicable (RGPD).

---

Anexos:
- Archivos relevantes: `mapa_trafico.html`, `qr_mapa.html`.
- Punto de inserción de la API key: `mapa_trafico.html:138`.
- Dominios a permitir en CSP: `maps.googleapis.com`, `maps.gstatic.com`, `chart.googleapis.com`.



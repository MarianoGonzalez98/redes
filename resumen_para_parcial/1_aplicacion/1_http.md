# REQUERIMIENTO HTTP

# HEADERS
- host: www.google.com
- Connection: Keep-alive <!-- para solicitar q se mantenga la conexión -->
- Keep-Alive: timeout=15, max=10 <!-- http1, ver si http1.1 -->
- Content-Type: text/html

## WEB-CACHE
- If-Modified-Since: Thu, 29 Apr 2017 17:31:00
- Last-Modified: Thu, 29 Apr 2017 17:31:00
- If-None-Match: hash <!-- INVESTIGAR -->

## posibilidad de hacer un upgrade durante la conexión http1.1 a 2:
<!-- investigar -->
- Connection: Upgrade, HTTP2-Settings
- Upgrade: h2c|h2.

## Posibilidad de negociar protocolo alternativo
- Alterantive Service: alt-svc.

## COOKIES


# CONEXION TCP ASOCIADA


# VERSIONES

## HTTPS

# CODIGOS DE RESPUESTA COMUNES:

## WEB-CACHE
200 u 304


# COMANDO CURL



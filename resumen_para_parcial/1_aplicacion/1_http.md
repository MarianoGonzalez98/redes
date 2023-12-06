
# REQUERIMIENTO HTTP

<Method> <URI> <Version>
[<Headers Opcionales>]
<Blank>
[<Entity Body Opcional>]
<Blank>
<Method HTTP 1.0> ::== GET, POST, HEAD, PUT,
DELETE, LINK, UNLINK

Formato URL: protocol://[user:pass@]host:[port]/[path]

# RESPONSE HTTP

<HTTP Version> <Status Code> <Reason Phrase>
[<Headers Opcionales>]
<Blank>
[<Entity Body Opcional>]

# HEADERS
- host: www.google.com
- Connection: Keep-alive <!-- para solicitar q se mantenga la conexión -->
- Keep-Alive: timeout=15, max=10 <!-- http1, ver si http1.1 -->
- Content-Type: text/html
- Location: indicates the URL to redirect a page to. It only provides a meaning when served with a 3xx (redirection) or 201 (created) status response.
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

- Mecanismo que permite a las aplicaciones web del servidor “manejar estados”.
- El cliente hace un request.
- El servidor retorna un recurso (un objeto HTTP, como una página HTML) indicando al cliente que almacene determinados valores por un tiempo.
- La Cookie es introducida al cliente mediante el mensaje en el header "Set-Cookie: unaCookie=uno" que indica un par (nombre,valor).
- El cliente en cada requerimiento luego de haber almacenado la Cookie se la enviará al servidor con el header "Cookie: clave=valor"
- El servidor puede utilizarlo o no.
- El servidor puede borrarlo.
# CONEXION TCP ASOCIADA


# VERSIONES

## HTTP 1.0
- Métodos: GET, POST, HEAD, PUT, DELETE, LINK, UNLINK

- HTTP/1.0 contempla autenticación con WWW-Authenticate Headers.
- En HTTP/1.0 no se contemplaron las conexiones persistentes por default. A partir de HTTP/1.1 [RFC-2068], si. En HTTP/1.0 se pueden solicitar de forma explícita mediante el header "Connection: Keep-Alive"

## HTTP 1.1
- Nuevos mensajes: OPTIONS, TRACE, CONNECT.
- Conexiones persistentes por omisión.
- Pipelining, mejora tiempo de respuestas.

Pipelining HTTP 1.1
- No necesita esperar la respuesta para pedir otro objeto HTTP
- Solo se utiliza con conexiones persistentes.
- Mejora los tiempos de respuestas.
- Sobre la misma conexión de debe mantener el orden de los objetos que se devuelven
- Se pueden utilizar varios threads para cada conexión.
  - Sin pipelining: 1RTT + FT por cada objeto, n objetos nRTT + nFT.
  - Con pipelining óptimo: n objetos: 1RTT + nFT.


## HTTPS
(HTTP sobre TLS/SSL)

- Utiliza el port 443 por default.
- Etapa de negociación previa.
- Luego se cifra y autentica todo el mensaje HTTP (incluso el header).



# CODIGOS DE RESPUESTA COMUNES:

- 200 
- 304 Not Modified:  indica que no hay necesidad de retransmitir los recursos solicitados. Es una redirección implícita a un elemento/recurso de caché.Esto sucede cuando el método de la solicitud es seguro (safe), como en el las peticiones con métodos GET o HEAD, o cuando el request (petición) está condicionada y usa la cabecera If-None-Match o un **If-Modified-Since**. El response 200 OK habría incluido los encabezados: Cache-Control, Content-Location, Date, ETag, Expires, y Vary.
- 301 Moved Permanently
- 302 Found: indica que el recurso solicitado ha sido movido temporalmente a la URL dada por las cabeceras "Location". The Location response header indicates the URL to redirect a page to. It only provides a meaning when served with a 3xx (redirection) or 201 (created) status response.
- 401 Unauthorized:  indica que la petición (request) no ha sido ejecutada porque carece de credenciales válidas de autenticación para el recurso solicitado.Este estatus se envia con un **WWW-Authenticate** encabezado que contiene informacion sobre como autorizar correctamente.
- 403 Forbidden: Hay similitudes entre el status 401 y el error 403, la diferencia es que este último no se soluciona con una re-autenticación. El acceso está permanentemente prohibido y ligado a la lógica de la aplicación, como el no tener los permisos necesarios para acceder al recurso.

## WEB-CACHE
- “Proxiar” y Cachear recursos HTTP
- Los cache como servers funcionan como Proxy
- Son servidores a los clientes y clientes a los servidores web.

- Los servidores agregan headers:
  - Last-Modified: date
  - ETag: (entity tag) hash

- Requerimientos condicionales desde los clientes:
  - If-Modified-Since: date
  - If-None-Match: hash

- Respuestas de los servidores:

  - 304 Not Modified.
  - 200 OK.
# COMANDO CURL




<!-- ------------ -->

## HTTP/2

Reemplazo de cómo HTTP se transporta. No es un reemplazo del protocolo completo.
https://docs.google.com/presentation/d/1r7QXGYOLCh4fcUq0jDdDwKJWNqWK1o4xMtYpKZCJYjM/present?slide=id.p19

### Problemas con HTTP/1.0, HTTP/1.1

- Un request por conexión, por vez, muy lento.
- Alternativas (evitar HOL):
  - Conexiones persistentes y pipelining.
  - Generar conexiones paralelas.

- Problemas:
  - Pipelining requiere que los responses sean enviado en el orden
  - solicitado, HOL posible.
  - POST no siempre pueden ser enviados en pipelining.
  - Demasiadas conexiones genera problemas, control de congestión,
  - mal uso de la red.
  - Muchos requests, muchos datos duplicados (headers).


### Diferencias principales con HTTP/1.1

- Protocolo **binario** en lugar de textual(ASCII), binary framing: (más eficiente).
- Multiplexa **varios request en una petición** en lugar de ser una secuencia ordenada y bloqueante.
- Utilizar una conexión para pedir/traer datos en paralelos, agrega: **datos fuera de orden**, **priorización**, flow control por frame.
- Usa **compresión de encabezado**.
- Permite a los servidores “pushear” datos a los clientes.
- La mayoría de las implementaciones requieren TLS/SSL, no el estándar.

### HTTP/2 mux stream, framing

- Puede generar una o más conexiones TCP. Trata de aprovechar las que tiene establecidas.
- Un **stream** es como una sub-conexión (una “conexión” http2 dentro de una conexión TCP).
- Un stream tiene un ID y una prioridad(alternativa) y son bidireccionales.
- Sobre una conexión TCP multiplexa uno o más streams (“conexiones http2”).
- Los streams transportan **mensajes**.
- Los mensajes http2 (Request, Response) se envían usando un stream.
- Los mensajes http2 son divididos en **frames** dentro del mismo stream.
- Los frames podrían ser de diferentes tipos.
- Un frame es una porción de mensaje: header fijo+payload variable (unidad mínima).
- Cada frame tiene un header en comun. Ej: HEADERS, DATA, DATA, DATA
- Frame types: HEADERS, DATA, PUSH_PROMISE, WINDOW_UPDATE, SETTINGS, etc.
- El mismo stream puede ser usado para llevar diferentes msj.

### HTTP/2 HEADERS

Se mantienen casi todos los HEADERs de HTTP/1.1.
No se codifican más en ASCII.
Surgen nuevos pseudo-headers que contiene información que estaba en el método y otros headers.
Por ejemplo: 

`HEAD /algo HTTP/1.1 `

se reemplaza con http2:

```
:method: head
:path: /algo
:scheme: https o http
:authority: www.site.com (reemplaza al headerHost: ).
```

Para las respuestas: `:status:` códigos de retornos 200, 301,
404, etc.

#### HTTP/2 priorización y flow-control

- Los streams dentro de una misma conexión tienen flow-control individual.
- El flow control permite al cliente pausar el stream delivery y resumirlo después.
- Los streams pueden tener un weight (prioridad).
- Los streams pueden estar asociados de forma jerárquica, dependencias.

![image-20230921230136944](./img/capa_aplicacion/image-20230921230136944.png)

- Cuando el cliente solicita una página, “parsea” el primer response HTML luego solicita el resto.
- El server puede enviar el HTML más otros datos, por ejemplo CSS o Javascript.
- No siempre es lo que necesita el cliente, depende de que funcionalidad ofrece.

![image-20230921230326865](./img/capa_aplicacion/image-20230921230326865.png)

#### Compresión y Soporte

- Compresión de encabezados.
- SPDY/2 propone usar GZIP.
- GZIP + cifrado, tiene “bugs” utilizados por atacantes.
- Se crea un nuevo compresor de Headers: HPACK.
- H2 y SPDY, soportados en la mayoría de los navegadores.

### Otras Características

- HTTP/1.1, posibilidad de hacer un upgrade durante la conexión:
  - Upgrade Header. Connection: Upgrade, HTTP2-Settings
  - Upgrade: h2c|h2.
- http2: Negociar el protocolo de aplicación:
  - ALPN: Application-Layer Protocol Negotiation.
  - Se negocia como extensión de SSL en Hello (Anteriormente NPN). Se ofrece: h2, h3, http/1.1, ...
- Posibilidad de negociar protocolo alternativo: Alternative Service: alt-svc.


## HTTP/3

Basado en HTTP over QUIC(UDP).
El protocolo de navegación HTTPS provee un canal de comunicación seguro entre el cliente y el servidor.
La seguridad que ofrece HTTPs implica que:
● La información viaja de manera cifrada de extremo a extremo.
● El cliente puede verificar la autenticidad del sitio visitado.
● Opcionalmente el servidor puede requerir autenticación del cliente.



# Capa aplicación

- [HTTP](#HTTP)
- DNS
- Correo
- FTP

# HTTP

Formato URL: protocol://[user:pass@]host:[port]/[path]

- URI
- URL
- URN

## HTTP 0.9


<details>
<summary>Pasos</summary>
<br>
1. Establecer la conexión TCP
<br>
2. HTTP Request vía comando GET.
<br>
3. HTTP Response enviando la página requerida.
<br>
4. Cerrar la conexión TCP por parte del servidor.
<br>
5. Si no existe el documento o hay un error directamente se cierra la conexión.
</details>

- Solo una forma de Requerimiento.
- Solo una forma de Respuesta.

![image-20230921171438162](./img/capa_aplicacion/image-20230921171438162.png)

``````
Request ::== GET <document-path> <CR><LF>
Response ::== ASCII chars HTML Document.

GET /hello.html <CR><LF>
GET / <CR><LF>
``````

## HTTP 1.0

- Se debe especificar la versión en el requerimiento del cliente.
- Para los Request, define diferentes métodos HTTP.
- Define códigos de respuesta.
- Admite repertorio de caracteres, además del ASCII, como: ISO-8859-1, UTF-8, etc.
- Admite MIME (No solo sirve para descargar HTML e imágenes).
- Por default NO utiliza conexiones persistentes.

Request:

``````
<Method> <URI> <Version>
[<Headers Opcionales>]
<Blank>
[<Entity Body Opcional>]
<Blank>
<Method HTTP 1.0> ::== GET, POST, HEAD, PUT,
DELETE, LINK, UNLINK
``````

Response:

```
<HTTP Version> <Status Code> <Reason Phrase>
[<Headers Opcionales>]
<Blank>
[<Entity Body Opcional>]
```


















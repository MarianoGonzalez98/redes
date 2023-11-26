# GENERAL

- Usa puerto UDP/TCP 53
- TCP se utilizará en caso de que la respuesta UDP exceda los 512 bytes o en el caso particular de una Transferencia de Zona



- Servidor Autoritativo: es un servidor que tiene a cargo una zona o sub-dominio de nombres. Es el responsable del mantenimiento de la misma. Pueden existir uno o más servidores responsables de una zona. A su vez, un servidor puede ser responsable de varias zonas.
- Servidor Local: es un servidor que es consultado dentro de una red, es el encargado de resolver los nombres solicitados por los clientes. Mantiene una cache con los nombres que va resolviendo. Al mismo tiempo, puede ser Servidor Autoritativo de la zona o espacio de nombres de los equipos de su red. Habitualmente también se los llama Servidor Recursivo porque responde estas consultas a los clientes internos o Servidor Cache porque almacena temporalmente los registros que obtiene.

# GLUE RECORDS
<!-- https://serverfault.com/questions/355887/why-does-dns-work-the-way-it-does/355927#355927 -->

# FLAGS
<!-- https://serverfault.com/questions/729025/what-are-all-the-flags-in-a-dig-response -->
- AA: Authoritative Answer, el servidor que responde la consulta, tiene autoridad sobre el nombre(servidor autoritativo).
- TC TrunCation (truncated due to length greater than that permitted on the transmission channel)
- RD: Recursion Desired. La consulta es recursiva.
- RA: Recursion Available. La recursión está habilitada en el servidor.
- QR specifies whether this message is a query (0), or a response (1)


# REGISTROS

## A (Address)
Mapean de un nombre de domino a una dirección IP. Pueden existir varios registros (A) con el mismo nombre, por ejemplo, para realizar balance de carga de un servidor muy accedido. Para aprovechar esto, el servidor de DNS debería responder con una lista de direcciones IP, siempre alternando el resultado con algún criterio, por ejemplo de forma circular.

www.l.google.com. 300 IN A 74.125.47.103

## MX (Mail Exchanger)
Indican, para un nombre de domino, cuáles son los servidores de mail SMTP encargados de recibir los mensajes para ese dominio.
De esta forma, no es necesario especificar el servidor completo de mail donde se encuentra la casilla destino, alcanza con indicar, solamente, el dominio. 
El servidor de mail SMTP que envía el mensaje deberá consultar, vía el servicio de DNS, cuáles son los servidores SMTP receptores para el dominio dado. 
Se puede detallar una lista de servidores y asignarles prioridades. Los valores más bajos son las mejores prioridades.Así, al momento de hacerse el envío del mensaje, si el servidor primario no está activo se podría recurrir a otro de menor prioridad para enviar el e-mail. Con la asignación de prioridades con igual valor se podría lograr un balance de carga entre los distintos servidores SMTP.


Suponiendo que se desea enviar un e-mail a la cuenta <pepe@gmail.com>, los servidores SMTP destinos en orden, de acuerdo a su prioridad, serían los siguientes:

gmail.com IN MX 5 gmail-smtp-in.l.google.com.
gmail.com IN MX 10 alt1.gmail-smtp-in.l.google.com.
gmail.com IN MX 10 alt2.gmail-smtp-in.l.google.com.
gmail.com IN MX 50 gsmtp147.google.com.

## CNAME (Canonical Name)
Mapean de un nombre de domino a otros nombres. Esta funcionalidad podría, también, resolverse con varios registros A.

www.google.com. 42938 IN CNAME  www.l.google.com.
www.l.google.com. 300 IN A      74.125.47.103

## NS (Name Server)
Indican los servidores de nombre autoritativos para una zona o sub-dominio.
No llevan asociado una prioridad, todos los servidores tienen la misma precedencia. Para lograr un mejor balance, al ir respondiendo se debería ir rotando el orden con el que se entregan los servidores autoritativos para una zona.

El software de DNS permite asignar roles de Servidor Primario y Servidor(es) Secundario(s). De esta forma, sólo se requiere configurar el primario y luego el secundario obtendrá una copia de la base de datos del servidor maestro/primario. Es importante que los servidores estén actualizados, por eso debe existir algún mecanismo para mantenerlos sincronizados. El Servidor Primario debería avisar a todos los Servidores Secundarios cuando se realiza un cambio, así estos re-copian la base de datos de nombres desde el servidor maestro. 
La comunicación entre servidores se realiza vía el mismo protocolo de DNS, con la diferencia que se hace sobre TCP en lugar de UDP, esto se debe a que, generalmente, los datos transmitidos ocupan más de 512 bytes (máximo para DNS sobre
UDP). Esta operación se llama Transferencia de Zona o, en inglés, **Zone Transfer**.

# less /etc/bind/db.cities.org
...
; ## ZONA RAIZ

cities.org. IN NS berlin.cities.org.
cities.org. IN NS brasilia.cities.org.
; ## BRAISLIA NO NECESITA GLUE RECORD ##

; ## ZONA delegada
trees.cities.org. IN NS brasilia.cities.org.
trees.cities.org. IN NS berlin.cities.org.
trees.cities.org. IN NS oak.trees.cities.org.
; ## GLUE RECORD ##
oak.trees.cities.org. IN A 192.168.40.1

## SOA (Start Of Authority)
Los registros (SOA) se crean por cada zona o sub-zona que brinda el servicio de DNS. En este
registro se especifican los parámetros globales para todos los registros del dominio o zona. Sólo se admite
un registro (SOA) por zona. Los parámetros que comprende son los siguientes.

- Nombre del servidor: nombre del servidor de nombres. En general, se llaman ns....

- E-mail del administrador: dirección de e-mail del administrador, reemplazando el símbolo arroba “@” por un punto “.”.

- Número de Serie: número de serie de la DB. Se debe actualizar cada vez que se hace alguna modificación al dominio. Se sugiere un formato YYYYMMDDSS. Tamaño de 32 bits.

- Refresh Time: cada cuánto tiempo los servidores secundarios deben refrescar desde el maestro/primario.
Tamaño de 32 bits. Según la [RFC-1912], se recomienda entre 1200 a 43200 segundos (horas).

- Retry Time: el tiempo que debe esperar un servidor secundario en intentar contactar nuevamente al primario, ante fallos de conexión, cuando necesita hacer un refresh. Tamaño de 32 bits. Según la [RFC-1912], se recomienda entre 180 a 900 segundos (minutos).

- Expiry Time: el tiempo que deja de ser autoritativa una zona. Luego de este tiempo, el servidor secundario debe chequear el Serial de la SOA del maestro y, si cambio, realizar una Transferencia de Zona: AFXR/IXFR. Tamaño de 32 bits. Según la [RFC-1912], se recomienda entre 1209600 a 2419200 segundos (semanas).

- TTL: en un principio se utilizaba como el Tiempo de Vida de un registro en una caché. Ahora se utiliza como el TTL de Cacheo Negativo: si se pregunta por un valor y el servidor autoritativo respondió que no lo tiene, no se volverá a preguntar por el tiempo de este valor. Tamaño de 32 bits. El TTL se puede indicar para cada uno de los RRs o definir uno global, depende de la implementación. Este valor no es parte del SOA. 
- 
## TXT (Textual)
Mapean de un nombre de domino a información extra asociada con el equipo que tiene dicho nombre, por ejemplo pueden indicar finalidad, usuarios, etc.

berlin.cities.org. IN TXT "Servidor Web y FTP"

## PTR (Pointer)
Mapean direcciones IP a nombres de dominio. Son el inverso de los registros (A), por esto, se los suele llama reversos.

100.1.20 IN PTR berlin.cities.org.


# Consultas por TCP/Flag TC
De acuerdo a la cantidad de informacióon a transmitir se utilizará TCP o UDP, siendo en la mayoría
de los caso UDP la elección. Se puede forzar el uso TCP desde el cliente en los comandos, por ejemplo la
opción +tcp del comando dig(1), o si la cantidad de información a transmitir no entra en un mensaje
de DNS el servidor indiciará el uso de TCP mediante el flag: TC


## reducir el tiempo de propagación a 1 hora luego de que se efectivice la modificación de los registros DNS afectados
<!-- parcial 2022-S1-3RA -->
<!-- 
¿¿

- Refresh Time: cada cuánto tiempo los servidores secundarios deben refrescar desde el maestro/primario.
Tamaño de 32 bits. Según la [RFC-1912], se recomienda entre 1200 a 43200 segundos (horas).

- TTL: en un principio se utilizaba como el Tiempo de Vida de un registro en una caché. Ahora se utiliza como el TTL de Cacheo Negativo: si se pregunta por un valor y el servidor autoritativo respondió que no lo tiene, no se volverá a preguntar por el tiempo de este valor. Tamaño de 32 bits. El TTL se puede indicar para cada uno de los RRs o definir uno global, depende de la implementación. Este valor no es parte del SOA. 
??
 -->


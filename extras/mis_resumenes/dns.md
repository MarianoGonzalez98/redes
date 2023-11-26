# DNS
Es un sistema distribuido de forma jerárquica, el cual está conformado por muchos servidores.
#### Objetivo principal:
Traducir nombres de dominio a direcciones de Internet (direcciones IP) y, de esta forma, lograr una abstracción de las direcciones de red utilizadas internamente por los protocolos.

## Sistema de Nombres
Organizado de forma jerárquica a través de dominios, sub-dominios y nombres finales.
Cada eslabón de la jerarquía es un dominio o sub-dominio (también llamados **ZONAS**) que contiene un espacio de nombres bajo esa denominación.

Existen dominios de primer nivel en la jerarquía, conocidos como **TLD** (Top Level Domains), que están predefinidos. Actualmente, los TLDs son manejados por el IANA (Internet Assigned Numbers Authority) a través del ICANN (Internet Corporation for Assigned Names and Numbers).El registro de los nombres no está concentrado todo en el ICANN delega registros por cada país (ccTLD). Por ejemplo, para la Argentina la entidad encargada es NIC.AR. Es también posible delegar dominios raíces a otras organizaciones.

Las organizaciones que mantienen los registros brindan servicios de consulta vía HTTP o el protocolo WHOIS [RFC-1834].

### TLD (Top Level Domains)
Los TLD más importantes se podrían clasificar en 3 grupos:

**gTLD, Generic TLD**: contienen dominios con propósitos particulares, de acuerdo a diferentes actividades.  Ejemplo .com, .edu, .gov, .int, .mil, .net y .org,

**ccTLD Country-Code TLD**: contienen dominios delegados a los diferentes países del mundo. Los códigos de los países est´an codificados en dos símbolos,

**.ARPA TLD**: es un dominio especial, llamado de infraestructura, usado internamente por los protocolos, creado para resolución de reversos: de direcciones a nombres.


Un **nombre de dominio** es una lista de labels (etiquetas) separadas por puntos que van desde el nodo (el label más a la izquierda) hasta la raíz del árbol (el punto). No son case-sensitive y cada label puede tener como máximo 63 caracteres. La cantidad máxima de labels es 127 y el nombre no puede tener más de 255 caracteres en total. Al nombre completo se lo identifica como **FQDN** (Full Qualified Domain Name) y se distingue por terminar con un punto (dot). Si no termina con punto se le puede agregar/concatenar al momento de hacer la búsqueda, un espacio del domino. Si el nombre sólo tiene un label se le agrega un domino, si tiene m´as eslabones, generalmente, se lo considera como un FQDN.



### Delegación de Sub-dominios/Zonas
Definido el espacio de nombres, y su administración, es necesario asignarlo a diferentes servidores de manera distribuida.

**ROOT Servers** (Servidores Raíces): encargados de atender la raíz del servicio. Se encargan de delegar cada una de las zonas generadas para los TLD, tanto gTLD como ccTLD. La delegación consiste en saber las direcciones IP de los servidores que se encargan de resolver (o sub-delegar) las zonas de manera autoritativa. La cantidad de servidores raíces está acotada a 13 debido a la limitación de 512 bytes de un mensaje de DNS sobre UDP.

Un **servidor autoritativo** tiene toda la información para una zona, puede producir cambios sobre la misma y es el que tiene la ultima versión.


### Registro de un nombre de dominio
En el proceso entran en juego diferentes entidades:
**Registrant**: cliente/entidad final, el que requiere registrar el nombre de dominio.
**Registry**: repositorio autoritativo responsable por el funcionamiento y la información en un TLD particular. Debe proveer la infraestructura de los servidores para resolver el servicio desde la raíz.
**Registrar**: interfaz entre el Registry y el Registrant. Puede proveer servicios adicionales al Registrant.

Depende de cada dominio como serán las políticas. Las políticas siempre deben estar en el marco de las definidas por el ICANN. Para un TLD debe existir solo una autoridad responsable, el Registry. Esta a menudo abre el servicio en varios Registrars como interfaz de registro. Los Registrant contactan a un Registrar para la operación.


### Servidores de DNS
Los tipos de servidores de DNS se pueden clasificar en 3 grupos, no siendo estos disjuntos, es decir, un servidor puede pertenecer a más de un grupo.

**Servidor Raíz**: es un servidor que cumple el rol descripto anteriormente en la sub-sección: Delegación de Sub-dominios/Zonas.

**Servidor Autoritativo**: es un servidor que tiene a cargo una zona o sub-dominio de nombres. Es el responsable del mantenimiento de la misma. Pueden existir uno o más servidores responsables de una zona. A su vez, un servidor puede ser responsable de varias zonas. Si un sub-sub-dominio es delegado dentro de esta zona, el servidor debe saber a que servidor(es) sub-delegarlo. 

**Servidor Local**: es un servidor que es consultado dentro de una red, es el encargado de resolver los nombres solicitados por los clientes. Mantiene una cache con los nombres que va resolviendo. Al mismo tiempo, puede ser Servidor Autoritativo de la zona o espacio de nombres de los equipos de su red. Habitualmente también se los llama Servidor Recursivo porque responde estas consultas a los clientes internos o Servidor Cache porque almacena temporalmente los registros que obtiene.

Otra solución en lugar de usar un DNS local (tanto en la red como en el propio equipo), es utilizar un **Open DNS**/Open NS, estos son servidores de DNS globales que permiten utilizase como locales admitiendo consultas recursivas.

Existe otro tipo de servidor de DNS llamado **Forwarder Name Server**. Este es un servidor que interactúa directamente con los ROOT Name Servers y con el resto del sistema de DNS, haciendo de intermediario o proxy entre los servidores locales y el mundo exterior. Este tipo de configuración se utiliza, habitualmente, por cuestiones de seguridad. Sólo se permiten/restringen consultas de DNS que salgan de la red local desde los Forwarder Name Server. Hay implementaciones livianas (lightweight) de Forwarders que combinan un DNS proxy con un DHCP server y un servicio de NAT.



## Protocolo DNS


### Funcionamiento del protocolo

##### Cliente/Servidor y Resolver
El protocolo de DNS puede trabajar sobre dos protocolos de transporte: TCP y UDP, siendo UDP
el m´as utilizado. El n´umero de puerto usado por el servidor es el 53 para ambos casos. El cliente puede escoger cualquier puerto no privilegiado. El protocolo UDP es utilizado en las consultas y en la mayor´ıa de las respuestas. TCP se utilizar´a en caso de que la respuesta UDP exceda los 512 bytes o en el caso particular de una Transferencia de Zona (copia de la base da datos de nombres de un servidor primario o master a uno secundario -antiguamente llamados esclavos-).

##### Resolución de Nombres, Iterativo vs. Recursivo



##### Estructura de los Mensajes de DNS



### Servicios y Registros de DNS
**Registros A**
**Registros PTR (Pointer)**
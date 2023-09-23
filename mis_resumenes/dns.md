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



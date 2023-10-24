# Capa Transporte
comunicación lógica HOST-TO-HOST (END-TO-END), comunica procesos.

## Funcionalidad de Capa de Transporte
- MUX/DEMUX App. to App. (puertos, Ports).
- Soporte de datos de tamanos arbitrarios
- Control y Deteccion de Errores, pérdida, duplicación, se corrompen.
- ¿Como enviar info sobre la red de acuerdo al estado de la misma? ¿Cuando y cómo una App. debe enviar datos?
  - Control de Flujo.
  - Control de Congestion.
- Dos modelos:
  - Modelo Confiable: TCP.
  - Modelo NO Confiable: UDP.

Protocolos de transporte para cada protocolo de aplicación:
![Alt text](images/image.png)

![Alt text](images/image-1.png)

![Alt text](images/image-2.png)

Trabaja con SEGMENTOS

Seleccion de protocolo de Transporte:La aplicacion de acuerdo a como esta programada selecciona el transporte. El acceso a los servicios de transporte se hace mediante API:**Network socket**.

# UDP vs TCP

## UDP
- User Datagram Protocol (RFC-768)
- Protocolo Minimalista. Menor Overhead.
- Características de IP: best-effort.
- Orientado a Packets/Datagramas (mensajes auto-contenidos)
- PDU: Datagrama (Por coherencia con nivel Transporte se suele llamar Segmento).
- Solo provee MUX/DEMUX.
- No requiere establecimiento de conexion
- Servicio FDX (dúplex completo (full-duplex, fdx) permite el envío de datos en ambas direcciones simultáneamente.)<!-- DUDA: OK?? -->
- Aplicaciones: video/voz streaming/TFTP/DNS/Bcast/Mcast

### Datagrama UDP

![Alt text](images/image-3.png)

- Puertos: MUX/DEMUX.
- Longitud: UDP HDR + Payload
- Checksum
  - Calculo Ca1, **Opcional**. 0 = Sin checksum
  - Calculado HDR + PseudoHDR + Payload.
  - PseudoHDR: IP.SRC + IP.DST + Zero + IP.PROTO + UDP.LENGTH. <!-- DUDA -->
  - PseudoHDR: proteccion contra paquetes mal enrutados. 
  - Aplicaciones de LAN por eficiencia lo podrían deshabilitar.
  - Si tiene error se descarta silenciosamente (no tiene control de errores, sólo detección)

El encabezado UDP provee: 
- MUX/DEMUX de aplic.
- Detección de errores (no obligatorio).

## SCTP
- Stream Control Transmission Protocol (RFC-4960).
Protocolo con menor Overhead que TCP y más servicios que UDP.
Orientado a Packets/Datagramas/Mensajes.
Incrementa Overhead end-to-end.
Asegura orden, secuencia con control de congestión.
Servicio FDX.
Aplicaciones: WebRTC y ssh lo podría utilizar (no es lo más común).

## TCP
- Transport Control Protocol (RFC-793)
- Protocolo confiable, ordenado, buffering, control de flujo y de congestión.
- Orientado a Streams (secuencia en orden de bytes).<!-- DUDA = archivo ?? -->
- PDU: Segmento
- Provee MUX/DEMUX
- Incrementa Overhead end-to-end para ofrecer confiabilidad
- Requiere establecimiento de conexión (y cierre)
- Servicio FDX
- Aplicaciones: FTP/HTTP/SMTP/acceso remoto(SSH,telnet,...)/Unicast

El encabezado TCP provee:
- MUX/DEMUX
- Detección de errores
- Sesiones
- Control de Errores
- Control de Flujo
- Control de Congestión.

Segmento TCP
![Alt text](images/image-4.png)

- Puertos: MUX/DEMUX.
- No tiene Longitud total, si de HDR LENGTH (variable, max 60B Unit=4B).
- Total LEN se computa para PseudoHDR, no viaja en el segmento.
- Checksum:
  - Cálculo Ca1. Obligatorio, calculado, igual que UDP.
  - Si tiene error podría pedir retransmisión, implementación de TCP descarta y espera RTO (Retransmission Timer).
- Necesidad de manejar Timers, RTO (tmout. por cada segmento). (implementaciones lo manejan máas eficiente).
<br>
- Campos de Sesiones: Flags: SYN, FIN.
- Campo de Detección de Errores: Checksum.
- Campos de Control de Errores: ACK, Nro. Sec (#Seq), Nro.Ack (#Ack).
- Campo de Control de Flujo: a los de errores se agrega, Win.
- Campos de Congesti ´on: agrega flags si participa la red.
- Máquina de estado finita por cada conexión.
<br>
- Permite Opciones y Negociación.
- TCP entrega y envía lo datos agrupados o separados de forma dis-asociada de la aplicación:
  - La aplicación puede enviar 300 bytes en un write y TCP lo podría enviar en 3 segmentos separados de 100 bytes c/u.
  - La aplicación puede enviar 100 bytes y luego otros 200 y TCP esperar para enviarlos todos juntos.
  - La aplicación puede intentar leer 200 bytes del buffer y TCP solo entregar 150 bytes y luego el resto.

### Otros campos TCP

- TCP entrega y envía lo datos agrupados o separados de forma dis-asociada de la aplicación.
- Datos Urgentes: URG.
  - Urgent Pointer válido su URG=1.
  - Indica: offset positivo + #Seq = last Data Urgent byte.
  - Indicar a la App. datos urgentes, debe leer.
  - Debería combinarse con PSH. Habitualmente llamado OOB data (TCP no soporta OOB!!!).
- Pushear datos: PSH.
  - Fuerza a TCP a pasar datos a la App.
  - No lo deja “Bufferear” los datos recibidos (Input).

Urgent Pointer:
![Alt text](images/image-8.png)

### Opciones TCP
- Maximum Segment Size (MSS), min. recomendado 536B, RFC-879 (basado en MTU=576, TCP+IP=40). Aclaraciones en RFC-6691.
- Window Scaling.
- Selective Acknowledgements (SACK).
- Timestamps.
- NOP.
- Otras.

## Establecimiento de Conexión
- Flags: SYN (Synchronize), ACK (Acknowledge) y RST (Reset).
- 3Way-Handsake (3WH).
- En el 3 segmento se puede enviar info.
- el ISN (Initial Sequence Number), se utiliza un contador que se incrementa cada 4 mseg.
- RST si no hay proceso en estado LISTEN.
- Open Pasivo (servidor) y Activo (cliente).
- Open simultáneo

### 3 way handshake
![Alt text](images/image-5.png)

### 3 way handshake (open simultáneo)

![Alt text](images/image-6.png)

## Control de Errores TCP
- Errores que pueden existir en IP:
  - Pérdida de paquetes: descartados.
  - Des-ordenados, retardados.
  - Duplicados.
  - Corrompidos.
- TCP intenta solucionarlos con el control de Errores que implementa.


## TCP Cierre de Conexión
- Flags: FIN (Finish), ACK y RST.
- 4Way-Close (4WC).
- Posibilidad de Half-Close.
- Podría cerrarse en 3WC.
- Espera en TIME WAIT, 2MSL (aprox. 2*2min).
- Evitar con SO_REUSEADDR.
- Cierre incorrecto con RST.
- Close simultáneo.

![Alt text](images/image-7.png)

## TCP Diagrama de Estados
https://users.cs.northwestern.edu/~agupta/cs340/project2/TCPIP_State_Transition_Diagram.pdf




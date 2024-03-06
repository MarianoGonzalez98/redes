
# Deteccion de errores
- Chequeo de paridad
  - de un bit
  - en dos dimensiones
- internet checksum (suma de comprobacion)
- Cyclic Redundancy Check (CRC)
  - bits de datos como los coeficientes de un polinomio

# Protocolos y enlaces de acceso múltiple
- punto a punto
  - ppp
  - enlace punto a punto entre switch ethernet y host
- **broadcast** (cable o medio compartido)
  - Ethernet "legacy"
  - HFC(Hybrid Fiber Cable)
  - 802.11

# Protocolos MAC
- particionado del canal
  - **ineficiente a baja carga, es justo y eficiente en alta cargo**
  - TDMA (Time Division Multiple Access)
  - FDMA (Frequency Division Multiple Access)
- acceso randómico
  - **eficiente a baja carga, un nodo unico puede utilizar completamente el canal**
  - ALOHA
  - CSMA, CSMA/CD, CSMA/CA
- toma de turnos
  - **overhead por polling, latencia, unico punto de fall**
  - polling


# Tecnicas de conmutacion de tramas
- Store and Forward
  - espera toda la trama y realiza FCS
  - el mas seguro
- Fragment Free
  - lee primeros 64 bytes
- cut-through
  - lee hasta Destination Address
  - el mas rápido

# IPv6

- MLD (Multicast Listener Discovery) reemplaza IGMP
- NDP (Neighbor Discovery Protocol) reemplaza ARP
- Msj de control ICMP informativos y de errores

# NDP
- RS ( multicast all routers :2)
- RA ( multicast all nodes :1)
- redirect

- NS (multicast)
- NA (unicast??)
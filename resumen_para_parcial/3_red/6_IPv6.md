# GENERALIDADES

# ESTRUCTURA DE DATAGRAMA


# DIFF CON IPv4

# TIPOS DIRECCIONES

# ALCANCE

# DIRECCIONES ESPECIALES
- Link-local Address= FE80::/64 (FE80::/10)
- Multicast Address=FF00::/8
  - Node-Local/Interface-Local= 
    - FF01::1, FF01::2 (no salen a la red).
  - Link-Local: (quedan en la LAN).
    - todos los nodos en lan=FF02::1
    - todos los routers en lan=FF02::2
- Any (sin especificar)= ::0/0 
- Loopback/Localhost= ::1/128
- Documentación=2001:db8::/32
- 6Bone=3FFE::/16, devueltas al IANA en 2006.

# HELPERS

# GENERACION DE IID ( EUI-64)


# Autoconfiguracion de direccion IPv6 usando EUI-64 recibiendo Router Advertisement con prefijo ABCD:1234::/64

# ICMPv6

## IPv6 Stateless Autoconfiguration

## Router Discovery

## Neighbor Discovery
- reemplaza ARP

# V O F
- b) En una red IPv6, las tramas Ethernet utilizan direcciones de 64 bits que se extienden utilizando el método EUI64.
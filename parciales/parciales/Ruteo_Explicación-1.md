# 1) Tenemos la red 192.168.10.0/24 para subnetear. ¿Es posible cubrir todas las redes con subnetting de máscara de longitud fija?
No, ya que la red que mas bits para hosts necesita es la red A que necesita 7 bits de host, por lo que me queda 1 bit para subnetting, con el que puedo obtener 2 redes, y no me alcanza para las 3 redes que necesito (necesito 2 bits de redes)

# 2) ¿Qué deberíamos aplicar?

Aplicando VLSM:

Como la red A tiene 100 hosts, necesita 100 direcciones ip, por lo que necesito 7 bits de host (2^7=128),
entonces subneteo 192.168.10.0/24 en 2 redes /25: 
- 192.168.10.0/25 --> le asigno esta ip a la red A
- 192.168.10.128/25

RED D necesita para 30 hosts 5 bits de host, por lo que subneteo 192.168.10.128/25 en 4 subredes /27:
- 192.168.10.128/27 --> asignada a RED D
- 192.168.10.160/27
- 192.168.10.192/27
- 192.168.10.224/27

Red C necesita 5 bits de host:
- 192.168.10.160/27 --> asignada a red C
- 192.168.10.192/27
- 192.168.10.224/27

Red B necesita 4 bits de host, subneteo 192.168.10.192/27 en 2 subredes /28:
- 192.168.10.192/28 --> asignada a Red B
- 192.168.10.208/28

para los routers subneteo 192.168.10.208/28 en 4 subredes /30

- 192.168.10.208/30 --> RtrA-RtrB
- 192.168.10.212/30 --> RtrB-RtrC
- 192.168.10.216/30
- 192.168.10.220/30

Subredes asignadas:
192.168.10.0/25 -> red A
192.168.10.128/27 -> red C
192.168.10.160/27 -> red D
192.168.10.192/28 -> red B
192.168.10.208/30 -> RtrA-RtrB
192.168.10.212/30 -> RtrB-RtrC

Tablas de ruteo:

Rtr-A:

| Destino        | Masc | Gateway        | Interfaz |
| -------------- | ---- | -------------- | -------- |
| 192.168.10.208 | /30  | -              | eth0     |
| 192.168.10.192 | /28  | -              | eth1     | red B |
| 192.168.10.0   | /25  | -              | eth2     | red A |
| 192.168.10.128 | /27  | 192.168.10.210 | eth0     | red C |
| 192.168.10.160 | /27  | 192.168.10.210 | eth0     | red D |
| 192.168.10.212 | /30  | 192.168.10.210 | eth0     |
| 0.0.0.0        | /0   | 192.168.10.210 | eth0     |

192.168.10.128/27 = 192.168.10.100|00000
192.168.10.160/27 = 192.168.10.101|00000
Se pueden sumarizar en: 192.168.10.128/26

Las ultimas 4 se pueden reducir, debido a que las 4 tienen el mismo gateway:
| Destino        | Masc | Gateway        | Interfaz |
| -------------- | ---- | -------------- | -------- |
| 192.168.10.208 | /30  | -              | eth0     |
| 192.168.10.192 | /28  | -              | eth1     | red B |
| 192.168.10.0   | /25  | -              | eth2     | red A |
| 0.0.0.0        | /0   | 192.168.10.210 | eth0     |


Rtr-B

| Destino        | Masc | Gateway        | Interfaz |
| -------------- | ---- | -------------- | -------- |
| 192.168.10.208 | /30  | -              | eth0     |
| 192.168.10.212 | /30  | -              | eth2     |
| 200.5.113.184  | /30  | -              | eth1     |
| 192.168.10.160 | /27  | 192.168.10.214 | eth2     | red D |
| 192.168.10.128 | /27  | 192.168.10.214 | eth2     | red C |
| 192.168.10.0   | /25  | 192.168.10.208 | eth0     | red A |
| 192.168.10.192 | /28  | 192.168.10.208 | eth0     | red B |
| 0.0.0.0.       | /0   | 200.5.113.186  | eth1     |


192.168.10.128/27 = 192.168.10.100|00000
192.168.10.160/27 = 192.168.10.101|00000
Se pueden sumarizar en: 192.168.10.128/26

<!-- duda: esta ok??? -->
192.168.10.0/25 y 192.168.10.192/27 se pueden reducir en:
192.168.10.0/24

| Destino        | Masc | Gateway        | Interfaz |
| -------------- | ---- | -------------- | -------- |
| 192.168.10.208 | /30  | -              | eth0     |
| 192.168.10.212 | /30  | -              | eth2     |
| 200.5.113.184  | /30  | -              | eth1     |
| 192.168.10.128 | /26  | 192.168.10.214 | eth2     | red C |
| 192.168.10.0   | /24  | 192.168.10.208 | eth0     | red A |
| 0.0.0.0.       | /0   | 200.5.113.186  | eth1     |



ARP:

PC-A hace ping a PC-B

Arp Request:
- ip origen: 192.168.10.3
- ip destino: 192.168.10.2
- mac origen: <mac_eth0_pc-A>
- mac destino: 00:00:00:00:00:00

Trama ethernet:
- mac origen: <mac_eth0_pc-A>
- mac destino: ff:ff:ff:ff:ff:ff

Tabla cam de Swt-1 despues del ping:
- p1: <mac_eth0_pc-A>
- p2 :<mac_eth0_pc-B>
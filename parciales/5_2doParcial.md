# 1
## b) Asumiendo que la red está funcionando correctamente, sin tener en cuenta lo evaluado en el punto a, indique de qué forma se podría reducir la tabla de ruteo del router C que se visualiza manteniendo el acceso a todas las redes.
191.8.0.128 10.0.0.5 /27 eth1
191.8.0.160 10.0.0.5 /28 eth1

191.8.0.100|00000
191.8.0.1010|0000
<!-- se pueden sumarizar si tienen mascara diferente?? si ambos fueran /27 si se podrian sumarizar, no?-->

/30
10.0.0.000000|00
10.0.0.000001|00
se pueden sumarizar a  <!-- esta mal porque no comparten el gatewat??  -->
10.0.0.0/29
ya que comparten la misma interfaz de salida

## c) La empresa decidió migrar únicamente los servidores de la Red C a una nueva red, Red D, conectada al router B usando alguna de las redes disponibles teniendo en cuenta que la dirección inicial a partir de la cual se realizó el subnetting es 191.8.0.0/23. Se debe asignar una de las redes libres de forma que se pueda aplicar CIDR en el router A desperdiciando la menor cantidad posible de direcciones y con la capacidad de asignar direcciónes IPs como máximo a 14 hosts. 


### i) Indique la dirección de red que se asignará detallando el desarrollo para su obtención.
### ii) Realice tabla de ruteo del router A de forma que se pueda acceder a todas las redes por el camino más corto, indicando las redes que se simplificaron
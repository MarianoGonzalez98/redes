# 1
En un escenario donde se envió un correo desde la cuenta docente@redes.edu.ar a alumno@gmail.com, responda las siguientes consultas, teniendo en cuenta que se obtiene solamente con la información que se muestra en la tabla:
![Alt text](images/image-6.png)

## ● ¿Qué host/s y en qué paso consultaron algunos de los MX de los dominios para establecer una comunicación?
Cuándo el MUA de docente@redes.edu.ar le envía el correo a su MTA, el MUA hace una consulta dns preguntando por el registro "A" de su servidor smtp, el cuál asumo que es mail.linti.edu.ar, para obtener la direccion IP y poder mandarle el correo.
Cuando el correo le llegue al servidor smtp "mail.linti.edu.ar", éste hará una consulta dns preguntando por el registro "mx" de "gmail.com". También necesitara el registro "A" correspondiente, para obtener la IP de un servidor smtp de alumno@gmail.com
## ● En función de la respuesta obtenida ¿tiene toda la información necesaria para establecer la comunicación?
No, falta la ip de los servidores smtp. Esta se obtiene a partir de los registos "A" del servidor dns del dominio "gmail.com"

# 2

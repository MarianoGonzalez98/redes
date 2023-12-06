# PDU
Segmento

# Diferencias, similitudes: udp y tcp

# Reconocer cantidad de conexiones por host

# Reconocer direcciones ip asignadas a un host
127.0.0.1 es una direccion válida.
0.0.0.0 no lo es

# COMANDOS
## TCPDUMP
## SS
    ss -nltu
    ss -natu
    ss -tnu


0.0.0.0:22 : This is a listen socket which accepts connections on any interface, port 22 for IPv4 connections only.


# PUERTOS USADOS
http / SSH / DNS / https / POP3 / IMAP / SMTP
- servidor dns: UDP/TCP 53
- pop3: 110 | 995 (TLS/SSL)
- imap: 143 | 993 (TLS/SSL)
- smtp: 25 
- ssh:  22
- https:443
- http: 80
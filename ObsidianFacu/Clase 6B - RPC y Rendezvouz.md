#clase 
Cuando tenemos situaciones cliente-servidor, donde hay comunicación bidireccional, debemos usar 2 tipos de canales (uno para requerimientos, y otro para respuestas), y si tenemos más de un cliente, necesitamos un canal de respuesta para cada cliente. 

RPC y Rendezvous son tecnicas de comunicación y sincronización entre procesos que suponen un canal bidireccional (O sea aplicaciones cliente-servidor)

### RPC (Remote Procedure Call)
Vamos a tener procedimientos remotos q son llamados. Cada vez que un proceso invoca a un procedimiento, se crea otro proceso para manejar la invocación. 
Tenemos módulos de programa, donde vamos a tener procedures que se exportan, que ven otros procesos. Podemos tener procedures y procesos locales. 
En **rendezvous** en lugar de generar procesos, se usa el mismo proceso para todos los casos

Los processo locales son llamados background para distinguirlos de las operaciones exportadas. 
Si el proceso llamador y el procedure llamado están en el mismo espacio, es posible evitar crear un nuevo proceso, mientras que se debe crear un proceso "server" si el llamado es remoto
![[Pasted image 20261019095733.png]]

Para sincronizar la comunicación, los módulos se pueden ejecutar:
- Con exclusión mutua (un proceso a la vez). En este caso las variables compartidas se protegen automáticamente contra accesos concurrentes, pero hay que programar sincronización por condición
- Concurrentemente: Necesitamos mecanismos para programar la exclusión mutua y sincronización por condición (cada módulo es un programa concurrente). Podemos usar cualquier método ya visto (semáfotos, monitores o incluso Rendezvous)

---
## Rendezvous
Combina la comunicación y sincronización. En este caso, un cliente invoca una operación mediante un "call" pero esta vez se sirve por un proceso existente en lugar de por uno nuevo. El servidor es un proceso activo con un "in" 
Un proceso servidor usa una sentencia de entrada para esperar un call y actuar
Las operaciones se atienden una por vez más que concurrentemente.

![[Pasted image 20261019101059.png|468]]
No usamos buffers, los procesos se "encuentran" en determinado momento


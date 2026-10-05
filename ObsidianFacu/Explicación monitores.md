### Sintaxis
En los programas tenemos 2 estructuras, los procesos y los monitores.
Los procesos están vivos continuamente. Y suelen haber más de uno
Los monitores son pasivos, están a la espera de que los procedimientos les hagan pedidos.. Podemos tener uno o más, incluso llegando a tener un arreglo de monitores. 
==No podemos declarar variables compartidas==, pero si podremos poner variables en los monitores que sean accesibles mediante los procesos de monitores

### Monitores
Representan un recurso compartido y tienen 3 partes:
- Declaración de las variables permanentes. Estas representan el estado del recurso compartido
- Procedimientos del monitor. Los procesos llaman a los procedimientos. Estos pueden tener variables locales
- Código de inicialización del monitor. Nos sirve para inicializar las variables permanentes, principalmente las variables q son particulares como arreglos o colas, etc. ==Si este código no terminó de ejecutarse==, no atiende a ningún proceso

Lo primero q ejecuta el monitor en el programa, es el código de inicialización.

#### Compartir recursos
Para compartir recursos entre procesos, vamos a guardar los recursos como estado(variables permantentes) de un monitor, y este tendrá un procedimiento para acceder a cada recurso, y también un procedimiento para guardar cada recurso

### Uso de procedimientos
Cuando un proceso hace un llamado a un monitor, este se bloquea y no atiende a nadie más. Esto es un problema si **en el procedimiento del monitor se llama a un procedimiento de otro**, ya que ahí se queda ocupado hasta que se termine la ejecución de los procedimientos. 
Los procesos NO acceden al monitor según un orden de llegada, sino que el monitor atiende de forma aleatoria a uno de los que está intentando acceder. 

### Sincronización
La ==Exclusión mutua== es implícita ya que el monitor solo atiende de a un proceso a la vez. 
La ==Sincronización por condición== es explícita y se debe hacer a través de las Variables Condición (VC). Estas son variables permanentes del monitor que solo se usan dentro de los procedimientos donde se declararon. Son colas de procesos demorados/bloqueados que se comportan como colas comunes.
Nos sirven para interrumpir la ejecución de un procedimiento llamado por un determinado proceso
- ==wait(vc)== duerme al proceso en la cola asociada a la vc
- ==signal(vc)== saca de la cola (despierta) al primer proceso dormido en vc, lo libera y lo envía a competir por acceder de nuevo al monitor para continuar la ejecución dessde donde se durmió
- ==signal_all(vc)== saca a todos los procesos de la cola y los envía a competir por acceder al monitor para terminar de ejecutar el procedimiento 
NO podemos usar funciones como isEmpty sobre variables condition

#### Orden de llegada
Los monitores por defecto no siguen un orden de llegada. Vamos a necesitar un monitor donde se forme la fila. Vamos a aprovechar la cola del wait a la vez que vamos a ir contando la cant. de procesos dormidos. USamos la técnica ==passing the condition==
![[Pasted image 20261001152732.png|464]]
#### Orden de llegada con prioridad
Ahora no usamos una única variable condición, sino que cada persona (proceso) va a tener una variable condición propia, que funcionará como un semáforo para cada proceso. En la imagen se puede ver que se encolan con prioridad en fila, y se quedan esperando que los desbloqueen, y cuando el de adelante es atendido, toma al que sigue en la fila y le avisa que puede pasar
![[Pasted image 20261001153711.png|483]]
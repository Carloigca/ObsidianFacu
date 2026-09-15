#clase
Tenemos un único flujo de instrucción, donde se ejecuta un proceso y las instrucciones escritas se convierten en instrucciones atómicas(en lenguaje de máquina, bajo nivel).
La ejecución Secuencial ejecuta una instrucción atómica después de otra, y se da el Determinismo (Para la misma entrada, multiples ejecuciones producen el mismo resultado)

---
En un programa concurrente, tenemos multiples flujos de instrucción
Aparece el concepto del No Determinismo, por lo que se obtienen historias diferentes. Algunas historias son inválidas por lo ue se deben sincronizar procesos. 

Esta sincronización puede ser por exclusión mutua < S >, o por Condición < AWAIT B;S >

Vimos la Sección Crítica (SC) y las barreras

---
Los dos grandes métodos de comunicación son memoria compartida y  memoria distribuida. 
Dentro de la memoria compartida, tenemos las varibles compartidas y los semáforos y monitores

==Semáforo== Instancia de un tipo de datos abstracto (O un objeto) con solo 2 operaciones (métodos) atómicas: P y V.
Internamente, el valor de un semáforo es un **entero no negativo**, y las operaciones:
- V: Señala la ocurrencia de un evento (incrementa)
- P: Se usa para demorar un proceso hasta q ocurra un evento (decrementa)

Se usa la analogía con la sincronización del tránsito para evitar condiciones. Nos permiten proteger Secciones Críticas y se pueden usar para implementar la sincronización por condición. 

##### Implementación
Se inicializan cuando se declaran. Por ejemplo:
- sem s; //NO
- sem s_mutex = 1;
- sem s_arreglo[5] = ([5] 1);

Si la demora del semáforo se hace sobre una cola (FIFO), las operaciones son fair, pero no vamos a asumir que se despierta en orden FIFO, no sabemos en qué orden se van a dar las instrucciones.

Donde antes usabamos una variable booleana "lock" o "free", ahora vamos a usar una variable entera que con 0 es false, y con 1 es true
![[Pasted image 20260907082635.png]]
P(free) si free es 1, lo deja pasar y decrementa el valor del semáforo (free), entonces mientras se da la SC, free queda en 0 y no lo puede implementar otro proceso. Después V(free) lo incrementa en uno para que otro proceso lo pueda usar. 
Es más simple que las soluciones de BusyWaiting, es una ==espera pasiva==. 
En una de las situaciones el semáforo se setea en 1, y en otra situación se setea en 0, preguntar

---
Barreras: señalización de eventos: 
Un semáforo para cada flag se sincronización, entonces un proceso setea el flag ejecutando V, y despues espera a que otro flag sea seteado, y luego lo limpia ejecutando P.
Una barrera para dos procesos relaciona los estados de los dos procesos para que podamos saber cuando un proceso llega o parte de la barrera.
El semáforo de señalización se inicializa en 0. Un proceso señala el evento con V(s) y otros procesos esperan la ocurrencia del evento ejecutando P(s).

---
Prod - Consum
Es el modelo de comunicación entre dos procesos. Sirve para sincronizar dicha comunicación. 
En la implementación se usa un buffer(cola) con un tamaño fijo, donde un productor va dejando productos mientras los consumidores van tomando cuando haya algun prod en el buffer. 

Semáforos Binarios Divididos: 
Se usa un buffer de un solo elemento (Producto), y dos variables enteras con 1 o 0, llamadas "vacío" y "lleno". Estas dos variables juntas forman el "semáforo binario dividido", donde el productor usa P(vacío), y el consumidor usa P(lleno)
![[Pasted image 20260907084441.png]]

NO hace falta que el productr pregunte si el buffer está lleno, ni el consumidor pregunta si está vacío.

---
Contadores de recursos: 
Cada semáforo cuenta con un nro de unidades libres de un recurso determinado. Esta forma de utilización es adecuada cuando los procesos compiten por recursos de múltiples unidades.

Ahora se usa un buffer de datos de tamaño N, una variable ocupado, otra libre, una variable "lleno", y la variable "vacio" que se va a inicializar en N. El siguiente ejemplo es con un prod y un consum
![[Pasted image 20260907085612.png]]

Para Agregar más consumidores y más productores, debemos agregar dos nuevos semáforos(en la imagen son los rojos), agregando exclusión mutua entre consumidores y tmb entre productores.
![[Pasted image 20260907085834.png]]


---
### El problema de los filósofos
Se usa para resolver el Deadlock. El Deadlock se da cuando los procesos compiten por conjuntos superpuestos de recursos, y las condiciones para que se dé, las condiciones son: 
- Exclusión mutua
- Retencion y espera 
- No apropiación
- Espera circular

![[Pasted image 20260907090135.png]]
Hay que ver cuál de las condiciones se puede romper, y por lo general desde el lado del Software es más facil romper la espera circular

---
Para la alocación de recursos y scheduling, cuando varios procesos compiten por el uso de recursos compartidos, podemos oprganizarnos de distintas formas dependiendo de cómo se quiera utilizar el recurso.
Si no importa el orden, se puede usar un semáforo como ya vimos

Si nos importa el orden, y tiene q ser de llegada, usamos una cola
![[Pasted image 20260907090945.png]]
Esto que se muestra en la imagen, Encolarte, preguntar por la condición, liberar, etc es Busy Waiting, LO QUE NO HAY Q HACER

Aparece la técnica de ==Pasing the baton== nos permite poner políticas arbitrarias, dejandonos elegir el proceso que accede.
Cuando un proceso está dentro de una sección crítica, mantiene (((Seguir escribiendo de la diapo de la foto)))![[Pasted image 20260907091259.png]]

se inicializa el semmáforo baton en 1. Tenemos un bool "libre", una cola de espera, y un semáforo que es un arreglo de N elementos inicializados en 0. N es la cantidad de procesos. 
El semáforo baton se usa en general, pero el semáforo arreglo se usa como semáforo privado para detener la ejecución de forma individual para cada proceso.
Armamos 2 protocolos, uno para request (id), y otro para release (id), y cada uno es una sección crítica en si, que permite coordinar el uso de otro recurso compartido, que también será una sección crítica en sí. 
Ver ejemplos de código

---
SJN (Shortest Job Next)
Es necesaria cuando varios procesos deben usar un unico recurso. Por lo que implementamos la técnica Passing the baton pero modificandolo en caso de que haya más de un recurso.
En este ejemplo CREO q es con un solo recurso
![[Pasted image 20260907092619.png]]

---
Lectores y Escritores
Hay procesos que solo Leen al BD, y procesos que la escriben. 
Los escritores acceden de manera recursiva para escribir, y no se puede meter ningun otro proceso. No debe haber nadie
Los lectores acceden y no se puede meter ningun escritor( Pueden haber varios lectores)
Tienen cndiciones distintasy son asimétricos, por lo que según el Scheduler tienen distinta prioridad, y hay que elegir de forma selectiva. 

Como problema de exclusión mutua es muy restringido por las condiciones, ya que si siguen llegando lectores, el escritor nunca podrá acceder a su SC

Como problema de ...
Se usa un semáforo para los escritores, otro para los lectores, y se pasa el baton cuando ya es el último del grupo (Creo jjdasj) 
![[Pasted image 20260907094322.png]]
Hay 5 secciones críticas, el acceso y salida de los lectores, y el acceso, salida y escritura de los escritores. 

---
### Desventajas de los semáforos
Si bien se usan y son muy importantes para SO y Redes, tienen alggunas desventajas:
- Variables compartidas libres, y hay que tener un control muy fino para saber cuándo acederlas
- Sentencias de control de aceso a la sección crítica dispersas en el código
- Al agregar procesos, se debe verificar
- a
- Aunque la exclusión mutua y la sncronización por condición son conceptos distintos, se programan de forma similar.

---
### Importante
Si nuestro código Concurrente proviene de un código secuencial que ya es eficiente, seguro nuestro código lo sea. 

---
## Explicación práctica
Semaforo: Tipo de dato abstracto que suele ser un entero
##### Declaración de semáforos: 
sem s;
sem mutex = 1;
sem espera [5] = ([5] 1);

Operaciones de los semáforos: 
P(s) -> < await (s > 0) ... sigue>

##### Ejercicio 4
Tenemos N clientes, que mandan secuencia de ADN a un Servidor (Tenemos un servidor) y esperan sus resultados. 
Siempre q veamos un proceso q manda información a otro, es modelo productor-consumidor. Recordar que no hay que preguntar por si la cola está vacía (en el servidor), hay que imlementar semáforos, y además los roles de productor-consumidor se pueden invertir. 

Cliente: Genera secuencia de ADN, lo encola, y espera resultado
Servidor: Recibe pedido con ID, resuelve la solicitud, y retorna el resultado con id

Vamos a tener q implementar un semáforo por id cliente, teniendo N semáforos. 

##### Ejercicio 5
Se supone q es como el 4, aparecen 2 servidores en lugar de uno, y tiene un clock. No lo explicó

##### Ejercicio 6
El orden de llegada en el 4 lo determinamos cuando se encolan los procesos. No podemos establecer un orden gracias a un servidor. Usando un único semáforo para gestionar el paso, no podemos asegurar que se siga el orden de llegada. 
Cada escalador va a tener su semáforo personal (tengo N semáforos).Ver  si la cola está vacía, no indica que el paso esté vacío, para saber si el paso  está libre, debemos usar un booleano.
Si el proceso "se manda a dormir", hay que liberar la sección crítica antes, pero como en este caso lo deberíamos usar adentro de un if, deberíamos ponerlo en ambos caso (en el if y en el else).
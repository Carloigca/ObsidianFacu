Volvemos a la pregunta de cómo implementamos la exclusión mutua y la exclusión por condición (AWAIT) en un lennguaje de programación. Debemos ver cómo se comunican los procesos. 
Con las variables compartidas teníamos busy waiting y dejamos de tenerlo con los semáforos

Semáforos:
- variables compartidas libres
- Sentencias de control de acceso a la sección crítica dispersas en el código
- Al agregar procesos, se debe verificar el acceso correcto a las variables
- Se puede olvidar de proteger las variables compartidas
- Aunque la exclusión mutua y la sincronización por condición son conceptos distintos, se programan de forma similar

---
Monitores:
Proponen un módulo de programa que encapsula los recursos compartidos, y solo se puede acceder a dichos recursos mediante operaciones. 
Tenemos una estructura de todo lo compartido, y solo podemos acceder mediante procesos (procedure) para manipular esos procesos. 
El monitor garantiza la exclusión mutua ya que solo puede acceder un proceso a la vez. Termina siendo implícita
Para la sincronización por condición, debemos usar variables condición, de modo que esta quede explícita

Los monitores tienen interfaz y cuerpo:
- Interfaz especifica las operaciones que brinda el recurso
- El cuerpo tiene variables que representan su estado y la implementación de los procedures indicados en la interfaz.
Se invocan: nombreMonitor.procedure(params)

Los procedure pueden tener parámetros de salida (declarados out en la declaración de los parámetros). procedure P1 (nombreVar: out Tipo;)

Si un productor y/o un consumidor pregunta por buffer vacío, es busy waiting y está mal

---
#### Sincronización por condición 
El primer proceso en llegar es el primero q se despierta (?)
Esta sincronización se programa con variables dcondición (cond cv)
El valor asociado a cv es una cola de procesos demorados, que no es visible directamente al programador. Tenemos las siguientes operaciones con las variables cv:
- wait (cv)
- signal (cv)
- signal_all (cv)
Además hay otras operaciones pero que no usamos en la práctica:
- empty (cv): retorna true si la cola controlada por cv está vacía
- wait (cv,rank): se usa orden de prioridad para despertar
- minrank (cv): el proceso tope de prioridad en la cola
Para el parcial de teoría debemos saber las dos disciplinas de señalización por más que usemos solo Signal and continued para la práctica

En todo momento la espera es pasiva, nunca activa.

---
Diferencias entre WAIT y P:
Wait: siempre se duerme el proceso
Con P, el proceso solo se duerme si el semáforo es 0

Diferencias entre Signal y V:
Con signal, si hay procsos dormidos, despierta al primero de ellos en orden FIFO segun se durmieron en wait. En caso contrario, no hay nadie dormido, no tiene efecto posterior, ya que no es igual al V
Con V, se incrementa el semáforo para que un proceso dormido pueda despertar, haciendo que un P continúe, pero no sigue ningun orden al despertarlo y TIENE efecto posterior, ya que al ejecutar muchos V, después muchos procesos que usen P pueden despertarse (Porque si se usaron muchas veces V, con P nunca se llega a 0)
![[Pasted image 20260914091317.png]]
En estos casos si se puede preguntar si la cola está vacía

---
A la hora de querer simular semáforos, puede pasar que si usamos un if, la variable numérica que lo simula puede terminar en numeros negativos. 

Técnica Passing the condition:
Sirve para conservar el orden, y no es distinto a passig the batton, pero en este caso usamos las condiciones para evaluar cuándo liberar o ocupar el semáforo. 
Primero debemos evaluar la condición por la cual el proceso se tiene q ir a dormir. Si no se debe dormir, trabaja. Tiene la siguiente estructura
![[Pasted image 20260914093936.png]]

---
Alocación SJN (Shortest Job Next) 
Se le da el recurso al que tiene lo va a usar menos tiempo. 
Primero si está libre, se usa, y sino espero. el procedure release libera si no hay nadie esperando
![[Pasted image 20260914094326.png|437]]

---
Sincronización por Condición Básica.
Desventaja: 
En el buffer circular, un prod puede depositar y un consumidor puede consumir en el buffer al mismo tiempo (por más que no sea la misma posición)

---
Covering conditions
disocié

---
Lectores y escritores:
El monitor SOLO regula el acceso a la BD, ya que si pusiéramos que el monitor es o contiene la BD, se daría exclusión mutua entre lectores y no nos interesa eso. 
![[Pasted image 20260914095504.png|472]]
((Notá la diferencia de cuando usa While y usa If, todavía no lo tengo del todo claro, tengo q implementarlo en la práctica))

Usando Passing the Baton, es importante usar if /else, no ifs separados. Ver ejemplo

---
Desventaja de los monitores en comparación con los semáforos
- evita el acceso compartido a elementos como vimos para los Lectores/Escritores

Las variables compartidas (que tienen Busy Waiting) son ineficientes, y solo las usamos si no contamos con otra herramienta
Los semáforos se usan en redes y en SO (lenguaje C)
Los monitores no son implementados tal cual en los lenguajes. y reducen la concurrencia cuando se quieren hacer accesos simultáneos a un recurso (ej un buffer)
![[Pasted image 20260914101501.png]]

![[Pasted image 20260914102029.png]]

![[Pasted image 20260914102038.png]]

![[Pasted image 20260914102045.png]]
Hoy en día no se usa ni semaforos ni monitores. Se usan variables mutex que aseguran exclusión mutua.
Y en la coordinación por condición, se usan variables condición sueltas.
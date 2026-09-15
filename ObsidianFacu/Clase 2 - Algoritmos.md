#clase
Un programa secuencial tiene un único flujo de instrucción. En ejecución, el programa se convierte en un proceso/unidad, y las sentencias son traducidas a instrucciones en lenguaje de máquina. Estas instrucciones en lenguaje de máquina son Atómicas, ininterrumpibles. 

En un programa concurrente, cada programa en ejecución es un proceso, y cada proceso tiene instrucciones que se pueden traducir en instrucciones atómicas que se van a ir intercalando durante la ejecución, lo que habilita muchas posibilidades de intercalar (interleavings) 
Obtenemos un No determinismo: Para una misma entrada (código) no obtenemos el mismo valor en todas las ejecuciones. O sea que obtenemos distintas historias, y no todas son válidas 

Para filtrar las historias inválidas, podemos ==Sincronizar procesos== mediante una de las dos formas: 
- Por exclusión Mutua (< S >). A lo sumo un proceso está en su SC (Sección Crítica)
- Por 

Problema de la sección crítica (Locks):
Barrera: 

La técnica ==Busy Waiting está mal==, es aceptable a nivel de Hardware, pero en la práctica no es lo mejor, entonces no deberíamos usarlo (mucho, supongo). Estas esperas activas son procesos que no hacen nada, yq ue gastan ciclos y energía del CPU hasta que alguien les cambie la condición. 
en esta técnica un proceso chequea repetidamente una condición hasta q sea verdadera: ... (Tiene 3 puntitos que desarrollan)

Trabajamos con los Estados de los procesos (Del CPU): Inactivo, Listo, Corriendo, Bloqueado, Completado (En la explicación del profe puso el gráfico)

La sección crítica es una sección de código que comparte memoria, y ==se deben cumplir las siguientes propiedades==: 
- Exclusión mutua: Al menos un proceso está en su SC
- Ausencia de Deadlock (Livelock): Todos los procesos no están bloqueados, están a la espera, sin hacer nada, pero al menos uno no está a la espera (entra en su SC)
- Ausencia de Demora Innecesaria: Si un proceso trata de entrar a su SC y los otros están en su SNC (No están en su SC), el primero no está impedido de entrar a su SC
- Eventual Entrada: un proceso q intenta entrar a su SC tiene posibilidades de hacerlo (Eventualmente lo hará)
La solución trivial es < SC >, ahora vamos a ver cómo implementar las sentencias Await
La solución al problema se puede usar para implementar una acción atómica condicional, una acción atómica condicional, y si S es skip y B cumple ASV:
- < S;> => SCEnter ; 
		S; 
		SCExit;

- < await (B) S; > => 
	SCEnter ;   
	while (not B) {SCExit; SCEnter;} 
	S; SCExit;

- < Await (B) > => while (not B) skip;

Esto es Correcto pero ineficiente, ya que un proceso continuamente entra y sale de la SC (Spinning), hasta que otro altere una variable referenciada en B, entonces para reducir la contención de memoria:  
SCEnter;
	While (notB) {SCExit;  Delay; SCEnter;}
	S;
SCExit;

Siempre vamos a intentar resolver la sección Crítica en dos procesos y después vemos si sumamos más. Primero vemos si la solución de Grado Grueso cumple las condiciones: 
- Exclusión mutua: Se excluyen el acceso?
- Asencia de Deadlock: Si hubiera deadlock, sc1, y sc2 estarían bloqueados en su protocolo de entrada, y esto no puede darse
- Ausencia de Demora innecesaria: Si sc1 está fuera de su SC o la terminó, in1 es fase, y si sc2 está intentando entrar, debería poder
Eventual Entrada: Puede pasar que en un escenario, el primero siempre se ejecute, y el segundo no ...
Se garantiza la eventual entrada con una política de scheduling ==fuertemente fair==.

La solución para cuando hay N procesos, es usar una variable booleana llamada Lock, esta técnica se llama SpinLock
Esta es la solución de Grado fino,  haciendo atómico al await de grado grueso, usando instrucciones atómicas a nivel de hardware: 
- Test & Set (TS)
- Fetch & Add (FA)
- Compare & Swap (CS)

SpinLock consiste en iterar (spinning) mientras esperan que se limpie lock, y si el scheduling va a ser fuertemente fair, se cumplen las 4 propiedades. 

![[Pasted image 20260831084139.png]]

---
#### Algoritmo Tie Breaker 
![[Pasted image 20260831084419.png|540]]
![[Pasted image 20260831084802.png|556]] 
(Desde la perspectiva del proceso) Digo que voy a entrar a la  SC, y ahí entro

Generalizándolo a N procesos se hace muy engorroso, complejo, ineficiente, y ==supuestamente no nos lo van a tomar==. 

---
#### Solución Fair = Algoritmo Ticket
Ya que el Tie-Breaker en N-procesos es complejo y costoso de tiempo, aparece este algoritmo:

Se reparten numeros y se espera a que sea el turno, entonces los procesos toman un numero mayor que el de cualquier otro que espera a ser atendido, y luego esperan hasta qur todos los procesos con nro más chico hayan sido atendidos.
En la práctica, se deberían resetear los numeros en algun momento para que no se vaya muy grande, y hay que analizar si se cumplen las propiedades.
![[Pasted image 20260831085943.png|559]]

Este algoritmo mejora Tie Breaker, pero depende de FA, que es simulable con otra SC. Si fetch and Add no existe, hay q simularlo

---
#### Algoritmo Bakery
Es similar al de ticket (Para mi). 
Cada proceso que trata de ingresar recorre los números de los demás y se auto asigna uno mayor. Luego espera a que su número sea el menor de los que esperan. Los procesos se chequean entre ellos y no contra un global
No requiere instruc especiales, ni ningun contador![[Pasted image 20260831092556.png]]
n st j => sirve para indicar que se skipee en la ejecución donde no sea mi turno (creo(?))

---
En la prog concurrente es importante tener una sincronización, en este caso mediane una Barrera (Barrier) 
### Barrera de Sincronización
Es un punto de demora a la que deben llegar todos los procesos antes de permitirles pasar y continuar su ejecución (A todos). Dependiendo de la apliación, las barreras pueden necesitar reutilizarse más de un vez, por ejemplo en algoritmos iterativos. 

Las implementamso mediante variables compartidas.
#### Flags y Coordinadores
Con contador compartido, cada proceso incrementa una variable Cantidad cuando llega a la barrera, y cuando Cantidad es N, los procesos pueden pasar. Pero después quién reinicia la variable?

Se debe implementar un nuevo proceso ==Coordinador== que utiliza más variables e indica cuándo se deben ejecutar los demás procesos, pero lo ideal es evitar este tipo de procesos Extra

#### Árboles
En este tipo de organización, los procesos son Coordinadores de otros procesos, y se usan unas barreras de arbol (Combining Tree Barrier) que construyen una Barrera Simétrica (O barrera de a par). 
![[Pasted image 20260831091850.png]]


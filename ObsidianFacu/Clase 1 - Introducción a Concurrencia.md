#clase 
Concurencia: 
Es cuando ocurren cosas simultáneamente o en paralelo. Cuando ejecutamos multiples actividades en paralelo o simultáneamente. 
Es un factor relevante para el diseño de Hardware, sistemas operativos, multiprocesadores, computación distribuida, programación y diseño. 

No determinismo: Para la misma entrada, no siempre vamos a obtener la misma salida.

La concurrencia es necsaaria para aplicaciones con estructuras mas naturales, para mejorar respuestas y eficiencia de un programa

Objetivos de los sistemas concurrentes
- Ajustar el modelo de arquitectura del hardware y software al problema del mundo real a resolver
- Incrementar la performance, mejorando los tiempos de respuesta de los sistemas de cómputo, a través de un enfoque diferente de la arquitectura física y lógica de las soluciones. 
---
Evolución de los procesadores
En el siglo XX, la computadora se componía por un único procesador, que representa una única unidad de procesamiento (UP), y que ejecuta una instrucción a la vez

En el siglo XXI se dio la **crisis del Hardware** (los cpu no podían ser más rápidos). Los cpu estaban compuestos por varias unidades de procesamiento (cores, UPs en un mismo disp.), y se llegó a la ejecución de múltiples instrucciones a la vez (Una por cada núcleo). Apareció la programación concurrente y la paralela. 

---
Ejemplo: Si tuvieramos una computadora con un solo CPU de un único Núcleo, podemos trabjaar con dos enfoques:
- Prog secuencial
- Prog concurrente (Sin paralelismo del Hw), que dedica una parte del tiempo a cada tarea, aprovechando los tiempos ociosos en la realización de cada uno de ellas. Problemas
	- Distribuir cargas de trabajo
	- Compartir recursos
	- Esperar puntos clave
	- 
Ejemplo 2: Ahora tenemos N unidades de procesamiento (Cores) para hacer las mismas N tareas, tenemos distintas soluciones
- Solución Paralela (Además concurrente): Todos los nucleos trabajan al mismo tiempo haciendo una única tarea. Concurrencia con paralelismo del Hw. Sigue siendo concurrente, pero además es paralelo. Consecuencias: Menos tiempo para completar el trabajo, Menos esfuersoz individual, y paralelismo del Hw. Dificultades:

"Proceso" es el elemento computacional q ejecuta un único flujo de control en forma secuencial: Ejecuta una instrución y cuando esta finaliza, ejecuta la siguiente
"Programa Concurrente" compuesto por múltiples procesos que trabajan "Simultáneamente" para resoler un mismo problema y/o usar recursos compartidos

El paralelismo es un concepto del Hw, solo llegamos a él cuando tenemos el Hw adecuado. 

El programa concurrente tiene P procesos, y 1 o más UP que puede ejecutar uno o más procesos. Si hay más de una UP, además de un sistema concurrente, es un sistema paralelo. 
En un sistema concurrente, el CPU es compartido entre varios procesos, por ejemplo por Time Slicing. 

---
Comportamientos de los procesos
Los procesos pueden actuar de distintas formas:
- Los procesos independientes son relativamente raros y poco interesantes. Pueden 
- La competencia (por recursos compartidos, típico en SO y Redes. 
- La cooperación es cuando los procesos colaboran para resolver uan tarea comun, sincronizandose y comunicándose
Cada proceso tiene su propio Espacio de direcciones y recursos

Los procesos livianos, threads o hilos:
- a

---
### Comunicación
Compitan o no, los procesos se comunican, se tienen q comunicar. El programa concurrente depende de cómo los procesos interactuan y se comunican. 
Formas:
- Memoria compartida: Los procesos intercabian info sobre la memoria compartida, o actúan coordinandose sobre datos residentes en ella. Se debe asegurar de no operar simultáneamente sobre la memoria compartida
- Pasaje de mensajes (Memoria distribuida): Se establece un canal (lógico o físico) para transmitir la info entre procesos. Esto requiere tener un protocolo adecuado, y para que la comunicación sea efectiva, los procesos deben "saber" cuándo tienen mensajes para leer y cuándo deben transmitir msjes.

### Sincronización
Tener info de lo que está haciendo otro proceso para coordinar actividades. Los procesos se sincronizan:
- Por exclusión mutua: Se asegura que solo un proceso tenga acceso a un recurso compartido en un instante de tiempo. Si el programa tiene secciones críticas q pueden compartir más de un proceso, la exclusión mutua evita q dos o más procesos se encuentren en la misma sección crítica al mismo tiempo. 
- Por colisión: Se bloquea un recurso para el proceso hasta q se cumpla una condición

Las variables compartidas **son distintas** de las variables globales q vemos en la programación secuencial. Process es distinto de procedure. 
Interferencia: un proceso toma una acción que invalida las suposiciones hechas por otro proceso. 

---
### Administración de recursos compartidos:
Esto incluye asifnación de recursos compartidos, metosdos de acceso a los recursos, bloqueo y liberación de recursos, seguridad y consistencia.
Una propiedad deseable...
Inanición y overloading son:

---
Independientemente del mecanismo de comunicación / sincronización enre procesos... 
Arrancó a volr el profe

---
Los procesos no son independientes y comparten recursos por lo q se hace necesario usar los mecanismos de exclusión mutua y/o sincronización que agregan complejidad a los programas
El no determinismo implícito en el interleavign de procesos concurrentes significa q dos ejecuciones del mismo programa no va a ser idénticas, y hay dificultad para la interpretación y debug
Overhead
.

Concurrencia: Concepto de software no restringido a una arquitectura particular de Hardware ni a un numero determinado de procesadores. Especificar la concurrencia implica especificar los procesos concurrentes, su comunicación y su sincronización. 
Paralelismo: Se asocia con la ejecución concurrente en múltiples procesadores con el objetiv principal de reducir el tiempo de ejecución. Los procesos colaboran y no compiten. 

Un programa concurrente se forma por un conjunto de "Procesos" secuenciales. La programaci+on secuencial estructurada se expresa con 3 clases básicas de instrucciones:
- Asignación
- Alternativa (Desición)
- Iteración (repetición con condición)
Ahora se requiere una clase de instrucción para representar la concurrencia. Agregamos las palabras claves:
- SKIP: Termina inmediatamente y no tiene efecto sobre ninguna variable del programa
- Sentencias de alternativa múltiple: Tenemos un if no determinístico con multiples condiciones y códigos (Como un Switch), y el programa solo va a ejecutar UNA de las condiciones verdaderas q haya.
- Sentencias de alternativa iterativa múltiple: Se da otra elección no determinísticamente. Elije de a una condición verdadera a la vez y vuelve a empezar hasta q todas sean falsas. 
- Sentencia "co": El cuerpo entre "co" y "oc" se va a ejecutar concurrentemente. OperaMP es un estandar para paralelismo, que ejecuta secuencial, hasta q le indique crear X procesos q se van a ejecutar concurrentemente. Y el programa no continúa hasta q todos terminen.
	- Fork: cuando se separa la ejecución en todos los procesos
	- Join: Cuando todos los procesos terminan la ejecución
	Aparecen los cuantificadores, que son procesos q ejecutan lo mismo con distintos datos. 
- Sentencia "Process": Es la definición de un proceso, y tmb tiene cuantificadores donde se usan N procesos independientes, c/u con su variable privada. Ejecua en Background. 
- NO HAY sentencia de "empeza a ejecutar". El proceso o hilo nace cuando lo creo. No hay q suponer q un proceso arranca distinto de otro solo por cómo se da el código, esto es el **no determinismo**. 

---
## Atomicidad de grano fino
"Estado" de un programa concurrente: Se detiene el programa con todos los valores de sus variables en ese momento. 
"Acción atómica": hace una transformación de estado, y esta transición es indivisible (Nadie la puede detectar o dividir), los estados intermedios son invisibles para otros procesos. Cada proceso ejcuta **instrucciones atómicas**. El SO hace un **intercalado** de las acciones atómicas q ejecutan los procesos individuales.
Llamamos "Historia" a una ejecución dada de un programa concurrente con un interleaving particular. El numero de posibles historias de un programa concurrente es enorme, pero no todas son válidas. Pero es la "Interacción" la que determina cuáles son válidas (no generan errores o interrupciones) 
La sincronización por condición nos permite evitar obtener historias no válidas. 

Acciones atómicas de grado fino: Instrucción atómica por naturaleza, es indivisible y se debe implementar por Hardware. 
"A=B" sabemos q no es atómica xq en nivel assembler se hacen 2 o más movimientos de registros y posiciones de memoria
"A=3" si q es atómica xq sabemos que solo se asigna el valor a la posición de memoria en assembler (No se puede dividir más)

"Referencia Crítica" es una referencia a una variable q es modificada por otro proceso. Vamos a asumir que cualquier referencia es una sola variable, leída y escrita atomicamente. 
**Propiedad de "A lo sumo una vez" (ASV)** se cumple si con "x= expr": 
- expr contiene a lo sumo una referencia crítica y x no es referenciada por otro proceso 
- expr no tiene referencias críticas, en cuyo caso x puede ser leída por otro proceso. 
Si una expresión cumple ASV, entonces puede ejecutarse como si fuera atómica aunque no lo sea. 
Si una expresión  o asignación NO cumple ASV, es mecesarop ejecutarla atómicamente. En general es necesario ejecutar secuencias de sentencias como una única acción atómica (Sincronización por exclusión mutua)

El mecanismo de sincronización para construir una acción atómica de **grano grueso** como consecuencia de acciones atómicas de graddo fino, que aparecen como indivisibles. 


Si nos aparecen los paréntesis cuadrados tipo <expr> significa que la expresión expr de adentro se debe evaluar atómicamente. Se dice q es un await sin condición booleana

< await (B) S; > Espera a que se cumpla B para ejecutar. Se usa para especificar sincronización. B representa una condición de demora. 
Ningun estado interno de S es visible para otros procesos. Mientras se ejecuta S ningun proceso debe modificar la condición B. 
El de arriba es el await general
El await para exclusión mutua es <expr>
El await para sincronizar por condición es < await (count mayor a 0) >

Si la condición satisface ASV, puede implementarse como Busi waiting o Spin loop
Entonces tenemos acciones atómicas condicionales o incondicionales. 
-------------------------------------
Acá me fui a llenar el termo, avanzó hasta Fairness y políticas de Scheduling
-------------------------------------
Una propiedad deseable en sistemas concurrentes es el equilibrio en el acceso a recursos compartidos por todos los procesos ( **fairness** ) .
Fairness: trata de garantizar que los procesos tengan chance de avanzar, sin importar lo que hagan los demás

Fairness debil:

Fairness Fuerte: Una politica de scheduling es fuertemente fair si:
- Es incondicionalmente fair
- Toda acción atómica condicional q se vuelve elegible eventualmente es 

Los SO no asignan tiempo por instrucción, asignan tiempo para la ejecución de varias instrucciones 
La politica Fuertemente Fair no es práctica, ya q puede funcionar para un programa, pero no para otro, y requiere conocer qué hace el programa. 

NO podemos depender de las políticas de Scheduling del SO porque es muy dificil de implementar. **Simular una política fuertemente fair es responsabilidad del programador.**
La ia justamente puede implementar la política fuertemente fair y es muy probable q se equivoque (algo así dijo, sigo sin entender el concepto de fair((?)


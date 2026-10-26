#clase Teoría 9

## POSIX Threads
Un Thread es un proceso "liviano" que tiene su propio contador de programa y pila de ejecución, pero no controla el "contexto pesado" (Por ejemplo, las tablas de página)
POSIX impulsó el desarrollo de una biblioteca en C para el multithreading, llamada Pthreads
Esta biblioteca para programación paralela en memoria compartida, permite crear threads, asignarles atributos, darlos por terminados, identificarlos, etc

Las Secciones Críticas (SC) se implementan usando bloqueo por exclusión mutua (Variables mutex)
Las variables mutex pueden tener uno de dos estados: 
- Locked
- Unlocked. Siempre se inicializan liberados
La operación "lock" es una operación atómica

![[Pasted image 20261026101845.png]]


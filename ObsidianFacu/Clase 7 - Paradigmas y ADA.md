#clase (Teoría 8)
Llegué 10 min tarde y estaba terminando de explicarlos, los explicó corriendo xq sigue con la teoría 9

Además de mostrarnos los algoritmos, nos explicó que en los sitios web de cada lenguaje de programación podemos ver las opciones de concurrencia q tiene cada uno.

ADA está dentro del GNU. Hay una especie de estándar de ADA.
La página de AdaCore tiene mucha información del lenguaje. 

los programas se deben validar formalmente para demostrar que este puede hacer lo q debe hacer. 
Es un lenguaje muy confiable. Se usa para distintas áreas donde no se permiten equivocaciones, como en la industria aérea. 

--- 
# ADA
(Teoría 7)
Desde el punto de vista de la concurrencia, un programa Ada tiene tasks que pueden ejecutar independientemente y contienen primitivas de sincronización.
Podemos tener tareas individuales e incluso definir tipos (TYPE) de tareas

En los header de los task, definimos los entrys, y dsp en el cuerpo definimos el código. En el header ponemos lo que se exporta. Los encabezados se repiten.

Cuando se quiere compartir el código, uno no comparte el código del body, sino que se manda en binario, ejecutable digamos, pero sí se da el header para que el que lo vaya a usar, sepa qué cosas le puede pedir a ese código en binario. Sirve como una interfaz 
#### Entrys
Los puntos de invocació (entrada) se llaman "entrys" y están especificados en la parte visible (header de la tarea). Pueden tener parámetros o no, y estos pueden ser IN, OUT, e IN OUT (Este ultimo no es por referencia)

Una tarea puede decidir si acepta la comunicación con otro proceso mediante una instrucción primitiva "accept". Estos accept pueden ser simples, o tener cuerpo (con "end")

#### Mecanismo de sincronización
El rendezvous es el mecanismo principal de sincronizacióń y tmb es el mecanismo de comunicación primario. 

"Entry call" es el llamado desde un task a un entry exportado por otro task. 
Los entry son llamados mediante "accept". Pero hasta que no se ejecute un accept, la tarea que declaró el entry se queda en espera.
![[Pasted image 20261026083815.png|475]]

Los Accepts pueden anidarse en algunas situaciones, de modo que acepte una entrada y dentro de este, se ejecute otro accept para otra tarea.

#### Error común con ACCEPT
Usar el Rendezvous como si fuera PMA o PMS. Hay situaciones donde es necesario hacerlo, pero realmente es mejor usar bien rendezvous sin la lógica de PMA o PMS
![[Pasted image 20261026084446.png|493]]

También se puede dar que se haga un Deadlock 
![[Pasted image 20261026084944.png|488]]

---
### Select
La sentencia Select permite que solo **un entry call** se ejecute, y tiene al menos 2 alternativas

Podemos tener un Select con Entry Call Condicional, y otro Temporal.
- El condicional tiene una u otra, usando ELSE
- El temporal Permite que se ejecute una sentencia primero, e integra un "OR DELAY" y si la primera sentencia no se ejecutó, se ejecuta la 2da. La duración del DELAY está especificado (En el header de la task?)

También podemos **tener múltiples Accept,** y cada uno tiene una condición booleana. Se separan usando OR WHEN
Cuando alguna alternativa es válida, se ejecuta el cuerpo de ese caso. 
Si hay 2 válidas solo ejecuta una y sale. Es como un IF no determinístico
Y si ninguno es válido, se queda esperando a que alguno sea válido. 

Aunque tmb **tenemos las alternativas del ELSE o OR DELAY** para los accepts.
Si ninguna alternativa es verdadera, se ejecuta el else o se espera el tiempo del delay. 

---
#### Atributo count
Es un atributo del Entry que sirve para usarlo en condiciones donde se le quiere dar prioridad a una alternativa en un select

---
# NO PODEMOS HACER:
Select
	accept
or
	Call

Ni tampoco:
Select
	Call
or
	Accept

---
Ahora explicó los ejemplos que tmb están en la explicación de Rendezvous. 


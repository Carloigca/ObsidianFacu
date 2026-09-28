#clase 
Llegué tarde y e perdí casi toda la clase.

PM = Pasaje de Mensajes. Vamos a comparar PM con el uso de monitores.

---
Para solucionar el problema de q hay muchos procesos, cada uno con su dato y todos quieren conocer los valores máximo y mínimo de todo el sistema, usaremos topologías distintas: centralizado, simétrico y en anillo circular![[Pasted image 20260928103107.png|626]]

el profe hizo énfasis en no usar valores predefinidos como máximo o mínimo (Ej 999 o -999), ya que al inicio estos toman el valor que carguemos en el sistema(o proceso). 

Hay una forma de resolver, en la que cada proceso recibía y envíaba su cantidad.  Y dsp otro proceso iba sumando el contador

---
Con la solución de la topología simétrica, nadie sabe de quién recibe, y todos envían a todos

Con la solución de anillo circular, se habla de pipelines. Es la forma en la q se implementa la red física de las facultades (Por fibra óptica). 

Da dos vueltas. En la primera hace comparaciones para sacar max y min global. En la segunda avisa a todos cuál fue el max y min global. 

##### Comentarios
Simétrica: 
- Más corta y sencilla de programar
- Mayor cant de mensajes (si no hay broadcast)
- Pueden transmitirse en paralelo si la red soorta transmisiones concurrentes, pero el overhead de comunicación acota el speedup
Centralizada y Anillo:
- Tienen una cantidad lineal de mensajes, pero tienen distintos parones de comunicación que llevan a distinta performance (Se puede generar Cuello de botella): 
	- Centralizada: Los msjes al coordinador se envían casi al mismo tiempo y solo se demora 
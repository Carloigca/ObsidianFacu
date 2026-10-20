#clase 
Modelo de memoria distribuida: se envían mensajes 
Hay 4 modelos teóricos para ver cómo interactúan los procesos con el envío de msjes
No hay uno mejor q otro, son todos distintos

#### Repaso Mensajes Asincrónicos PMA
El modelo asume q los procesos comparten canales (solo comparten eso).
Los canales son colas, donde los procesos envían msjes encolándolos. Los procesos pueden enviar y recibir msjes de los canales
Hacen Send en un canal y no esperan q el otro lo reciba. 
Hacen Receive en un canal varios procesos, lo hacen en orden siempre q haya un canal. Si  no hay mensaje, es bloqueante.
En productor-consumidor, usamos el canal como un buffer donde el productor deja los productos para el consumidor
Las guardas solo tienen una condición booleana, mientras que en PMS tienen una condición y una sentencia de comunicación
### Modelo de Mensajes Sincrónicos PMS
Los canales no son colas, son de tipo link o punto a punto
En este caso el proceso q envía, va a esperar que se reciba. Como la capacidad del canal es de a uno, no se pueden enviar muchos mensajes
**Se reduce el grado de concurrencia** 
Por cada proceso, va a haber un canal de comunicación entre cada par de procesos. Por ejemplo con 3 procesos, va a haber 3 canales. Con 4 procesos, vamos a tener 6 canales
Hacen Sync_Send, y los limita porque quedan esperando. Es bueno o malo dependiendo del problema. 
Al igual que en PMA, si no hay mensaje, el receive es bloqueante. 

En productor-consumidor es raro porque el productor queda detenido hasta que no aparezca un consumidor. Para solucionar esto, se pone un proceso "buffer" en el medio de los dos.
Este buffer recibe lo producido, y además espera msjes de los consumidores para darles el producto. 
En Cliente-Servidor...

Hay que tener cuidado con el **Deadlock**, que es similar al de los semáforos. Si tengo dos procesos intercambiando valores, y ambos envían y después reciben, los dos se bloquean porque para que uno envíe, el otro tiene q estar recibiendo. Siempre que uno envíe, otro va a tener q estar recibiendo.
Notación parecida a CSP (Communicating Sequential Processes). Es un lenguaje teórico que incluye PMS y comunicación guardada (if - do de multiples posibilidades)
Canal es un lin directo entre dos procesos, half-duplex(uno envía y otro recibe), y comunicación guardada (se usa un waiting selectivo, eligiendo a qué proceso enviarle)

### Sentencias de comunicación
```
Para enviar vamos a poner: 
	Destino ! port(e1,e2,...,eN)
	A ! canalUno (dato)
Para recibir vamos a poner:
	Origen ? port(x1,x2,...,xN)
	B ! canalUno (resultado)
```

El port se puede llamar como sea, el tema es que tanto origen como destino invoquen el mismo. 

Como hay muchos procesos q pueden hacer lo mismo (Cuando tenemos un arreglo de procesos), como en cliente-servidor que hay muchos clientes, **podemos recibir con asterisco** (recibe de cualquiera). Esta es la simplificación de poner un if no determinístico de CSP: 
```
Proc Server
if 
	C[1]
		code
	C[2]
		code
	C[3]
		code
	C[N]
		code
---
La simplificación queda como
C[*]
``` 

Con un **do** se definen 2 o más condiciones con una acción a ejecutar en cada caso
Y se va a repetir (en bucle) hasta que se dé una condición ajena a las indicadas.

---
### Comunicación Guardada 
![[Pasted image 20261019083231.png|571]]
La estructura (por ejemplo ==if==) que implemente las guardas, puede quedarse bloqueada esperando en caso de que todas las condiciones sean true, pero las comunicaciones no se puedan ejecutar. 

Si no hay if, sino ==do==, se va a iterar hasta que todas las guardas sean falsas

![[Pasted image 20261019083709.png]]
Esto simula pasaje de mensajes Asincrónico con un Productor-Consumidor
**No lo vamos a hacer en la práctica**, porque es dificil de implementar una guarda con sentencias de ENVÍO. 
Para esto, hacemos que el consumidor PIDA los datos al intermediario. Entonces el intermediario espera que el productor produzca, o espera que el consumidor le pida un dato. 

---
Acá mostró un caso de asignación de recursos, para le caso de un cliente-servidor donde el servidor da ciertos recursos y dsp se los devuelven

---
Para **Ordenar un arreglo**, vamos a usar la lógica en la que el más grande del arreglo de un proceso, es el más chico del arreglo del otro proceso. 
De modo que P1 termine teniendo por ejemplo un arreglo que vaya del 1 al 5, y P2 termine teniendo un arreglo que va de 6 a 10. 

![[Pasted image 20261019084532.png]]

Para  garantizar que el arreglo quede ordenado, se puede:
 - Implementar un coordinador
 - hacer las revisiones(?) necesarias

---
### Explicación práctica de PMA y PMS 
En Asincrónico **no hay variables compartidas**, solo máquinas separadas (**solo procesos y canales**), y si en una se declara una variable, hay que comunicarselo a la otra. 
No se requiere exclusión mutua, pero existe en el canal que es el guardador de ese recurso. 
Podemos tener *Canales simples* o un *Arreglo de canales*. Un canal es FIFO, se recibe en el orden q se envía
Tenemos el:
- Send Canal (msj)
- Send Canal[i] (msj)
Tenemos el receive
- Receive Canal (msje)
- Receive Canal [i] (msje)
Tenemos el empty:
- Empty (canal)
- Empty (canal[i])
El problema del empty es que puede ser realizado por varios a la vez, generando un toque de demora

Nos podemos permitir Busy Waiting si lo podemos solucionar. 

# Computación cuántica
Hay otros operadores lógicos, no más AND, OR, NOT, etc. 
El bit es una linea entre dos puntos, 0 y 1. Mientras q el Qbit es una esfera donde los polos son 0 y 1, pero este puede tener cualquier valor dentro de la esfera, el valor es un vector.
Un Qbit tiene valores infinitos, pero en algun momento estos valores se reducen a 0 o 1. En algun momento el Qbit toma ese resultado digital, es la forma de comunicar las dos computaciones. 
En la simulación tiene una flecha donde se muestra hacia qué valor va a tender. 
Si la flecha apunta horizonalmente, hay 50% de q sea 0 y lo mismo de q sea 1. Hay superposición en el Qbit. 
Al ejecutar con este Qbit podemos obtener cualquiera de los 2. *La  computación cuántica no es determinística, es probabilística* justamente por esto. 

Simulación de Qbit. Esfera de Bloch
https://javafxpert.github.io/grok-bloch/

Superposición: el qbit no es 1 ni 0. Si apunta al eje horizontal, hay 50% de q sea 1 y 50% de q sea 0. La superposición funciona como cuando vemos un vidrio en un día soleado: Vemos para afuera pero tmb vemos nuestro reflejo, vemos ambas cosas. 
Los resultados se miden estadísticamente ya que siempre se pueden obtener resultados distintos. Finalmente termina valiendo 1 o 0, y este resultado se trata de forma normal (como hacemos hoy en día con los bits)
Entrelazamiento: El estado de un Qbit depende del otro (compuerta CNOT de los Qbit)

IBM Quantum: procesador superconductor con 5 qbits conectados. Al hacer una operación con 2 qbits, estos tienen q poder conectarse, y si no se pueden conectar por los espacios, hay q cambiarlos de lugar (como q se intercambian la información creo)
IBM tiene 200 qbits como mucho. Los qbits no se pueden manipular facilmente. Hay un IBM Quantum de 127 Qubits. IBM solo da 10 minutos para usar la computadora y con contexto de la maquina, otras empresas cobran (o cobran mas supongo) y no te dan el contexto, no sabes si tiene ruido

Para poder mantener los estados cuanticos en superposición, los procesadores requieren estar en 0° grados. Por eso es dificil mantenerla. **No hay Hardware** q sostenga todas las características o condiciones q necesita esta tecnología. La radiación, el movimiento, cualquier cosa, afecta al Qbit, y se genera "**ruido**". Este ruido hace q los qbits colapsen antes obteniendo errores distintos.

IBM tiene el lenguaje Quiskit, y hay otro q no escuché, son el Assembler de la computación cuántica. IBM Composer creo q es el programa donde se usa
Hay librerías de Quiskit en python q se pueden usar en Jupyter Notebook


Con los Qbits tenemos la ventaja de q podemos mandar una única función en lugar de mandar varias funciones. Es una ejecución para todos, dsp colapso, y después obtenemos un resultado. El 80% puede ser un buen resultado, entonces no podemos estar seguros de q el resultado obtenido la primera vez sea el mejor. Tenemos q ejecutar muchas veces además porque tmb puede haber "ruido" 
LAs computadoras cuanticas son más eficientes en cuanto a tiempo y consumo. 

En un simulador, se obtienen resultados distintos a los de un qbit ya que el qbit puede tener ruido. Aunque los resultados generales sean distintos, los resultados probabilísticos suelen ser iguales (si era 80% 1, en ambos casos será un porcentaje alto para 1)

En unos años se prometen computadoras "Full tolerant" que no van a tener errores. Donde el ruido no se tome en cuenta o no exista. 

El motivo de q el simulador igual dé resultados probabilísticos, es la compuerta H. Si multiplicamos obtenemos 1 sobre raíz de 2, y al despejar la raíz de 2 podemos obtener un resultado positivo o negativo. Usando esta compuerta abrimos la posibilidad de q varíe el resultado

Además de IBM tenemos distintos procesadores, que en lugar de usar "superconductores", usan iones. Los iones cambian de estados, y hay láseres q permiten cambiarlos de lugar y rotarlos. Empresa "IonQ". Hay computadoras de 64 Qubits
Los iones se conectan todos con todos. 

La investigación va para el lado de descubrir cómo subimos el "nivel de abstracción", así como originalmente se trabajaban con bits

En el camino de un programa primero se hace un diseño, después se "transpila", Luego se encola, se controla, y se ejecuta en el computador cuántico, y recién ahí aparecen los Qubits. 

Hay muchas cosas de la computación tradicional q interactúan con lo cuántico. 

En LATAM se destacan brasil y méxico en cuanto a inversiones en la investigación. En arg tenemos buena base física. Se apoya muhco e la física ya q se apoya mucho en las propiedades físicas de los qbits. 

Ingeniería de software cuántico:
Cómo testeamos? Si cuando vemos al Qubit es porque ya colapsó y nos dio un resultado. No podemos poner un "checkpoint" como en el testing tradicional. 
Manifiesto Talavera (2020) de un grupo de España. Habla sobre la ingeniería, gestion calidad y reutilización vinculado a la ingeniería se soft. Y desarrollaron un ciclo de vida de esta ingeniería de soft cuantico. 

RIPAISC.net: Red Iberoamericana Para ...
Hay 30 organizaciones de todo el mundo y de otros países participando. 
Seminario virtual con una mirada más "dura" q trabaja más con la matemática https://go.ripaisc.net/holik-sem
Profesores: Alejandro Fernandez, profesor Corvalan(?)(Da una materia de esto)
Tesis: https://fotscode.github.io/qudos/

Dudas: 
- Un bit es corriente eléctrica, un Qubit es como un átomo que se analiza? Es como un ion, atrapado y manipulado. Analizan la posición de electrones. Por ejemplo con láseres se pueden manipular. La de la facu con 3 qubits es magnética y tiene "productos" afectados por estos imanes. 
- Lo más complejo? Comparacion con algo en bits de complejidad?
- QPU= la idea es adaptarla para q complemente las computadoras con bits o que en el futuro se trabaje unicamente con qbits?
- La q está en la facu, si bien tiene tecnología magnética, tiene mucho ruido o cómo se mantiene? 
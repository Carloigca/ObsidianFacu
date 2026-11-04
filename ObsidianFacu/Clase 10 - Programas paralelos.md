#clase
Esta clase no entra en el parcial práctico. 

Los sistemas paralelos se usan en todo. Si dependemos de las maquinas, las máquinas no pueden ser más rápidas. 

---
## Sistemas
- Concurrentes: 2 o más tareas sucediendo simultáneamente, aunque no necesariamente suceden en el mismo tiempo físicamente. En una mudanza, se turnan el ascensor. Es un concepto de Sw, donde hay varios procesos o hilos simultáneos
- Paralelos: 2 o más tareas sucediendo simultáneamente, pero usando multiples unidades de procesamiento con el objetivo de mejorar el rendimiento (reduciendo el tiempo de ejecución). Necesitamos más Hardware. En una mudanza, necesito 2 ascensores. Es un concepto de Hw, donde hay varios procesos o hilos pero ejecutándose en una unidad de procesamiento indepentdiente para mejorar el rendimiento
- Distribuidos: nodos independientes, cada uno con su propio cpu, memoria, etc, que colaboran mediante el envío y recepción de msjes para un objetivo comun, y aparrece al usuario como un sistema único y coherente. Es un programa concurrennte q implica comunicación por mensajes. Hay q aclarar q para ser considerados distribuidos, no deben ni compartir memoria, sino sería más como concurrencia. 

---
## Arquitecturas paralelas
Hay que entender perfectamente como funciona el hardware actual. Un programa no va a funcionar en todas las máquinas
Podemos clasificar las arquitecturas paralelas:
- Por la organización del espacio de direcciones: 
- Por el mecanismo de control
- Por la red de interconexión
- Por la granularidad

#### Por el espacio de direcciones
Según su espacio de direcciones, las aquitecturas paralelas se clasifican en:
- Multiprocesadores de Memoria Compartida
- Multiprocesadores de Memoria Distribuida
...

Adentro de los ==multiprocesadores de mem compartida== tenemos 2 clasificaciones:
- UMA: Cada CPU tiene su cache,, pero usan la misma memoria, teniendo el mismo tiempo de acceso
- NUMA: (Acceso a memoria No Uniforme) Cada CPU tiene un conjunto de bancos asociados en memoria. Cada cpu puede leer y escribir cualquier banco de memoria. 
Problemas de sincronización y consistencia son problemas de la memoria caché:
- Coherencia: hilos o procesos que deben ver en sus caches privadas el mismo valor de una variable compartida. Si un cpu modifica un dato cacheado, la caché del otro cpu debería "enterarse". Los fabricantes lo resuelven con protocolos de coherencia, se resuelve con Hardware que detecta la incoherencia. 
- Contención: Conflicto en acceso a un recurso compartido, como cuellos de botella, donde se saturan los buses. Cuando tenes varios hilos, capaz un hilo necesita usar más memoria cuando esta está llena, y eso implica sobreescribir algun dato cacheado por otro hilo, la caché no alcanza para cubrir todos los hilos

# Hay foto en Datos 2/11 8:32am
Puso a prueba, ejecutó ambos ejemplos. El primero tardó 7,5 segundos, y el segundo tardó 0,07109 segundos. Para analizar el tiempo, hay que ver la magnitud, no la totalidad, por lo que podemos multiplicar ambos números para verlos en una mayor magnitud
HPC= computo de alta performance (?), no solo tarda poco, sino que consume menos energía, dato muy importante hoy en día con celulares o dispositivos con baterías. 

Las GPU son arquitecturas de memoria compartida. Tiene varias unidades de procesamiento, lo común es entre 4 y 8

Adentro de los ==Multiprocesadores de memoria distribuida== tenemos varias máquinas conectadas por una red. 
Segun como se ejecuten las instrucciones sobre los datos, se pueden clasificar las máquinas siguiendo la **Taxonomía de Flynn**:
- SISD (Single Instruction Single Data): Computadora comun (previa a los 2000) con una unica cpu y hace un calculo a la vez, secuencial. Un único flujo de instrucción a la vez (con varios datos)
- SIMD (Single instruction multiple data): Una máquina con muchas CPU, donde cargo la misma instrucción, y cada CPU comienza a ejecutar la instrucción pero con datos distintos (distribuyendo la tarea entre todos), ejecuta sobre multiples flujos de datos. Es el modelo de las GPU
- MISD (Multiple Single Data: La más rara. Es una única cpu que ejecuta distintas operaciones con los mismos datos
- MIMD (Multiple Instruction Multiple Data): Puede ser una máquina con muchas CPU o muchas máquinas cada una con su CPU. Multiples instrucciones ejecutadas sobre multiples flujos de datos. Se pueden sub-clasificar en MPMD(muchos programas, o sea un programa por máquina) o SPMD (todos ejecutando un programa)

---
## Clasificación por la Red de interconexión
Podemos conectar los procesaddores y memorias usando redes:
- Estáticas: Vienen definidas de fábrica
- Dinámicas: Son modificadas según la necesidad. Se solían usar buses, pero genera contención, ya que lo usa uno a la vez. Se cambió por Crossbar, lo que es más caro pero más eficiente. Con crossbar, se establece un circuito entre cada cpu y cada banco de memoria, permitiendo multiples accesos simultáneos al mismo banco (No se disputan el bus) 

---
Una vez q entendimos el Hw paralelo, Hay que realizar un buen diseño. Cualquier buena solución de un algoritmo paralelo, puede diferir de lo visto para algoritmos secuenciales
Debemos :
- descomponer el problema, analizar las tareas que podemos dividir. Mapear tareas y datos a los nodos físicos de procesamiento

- Manejar la comunicación y sincronización. 
- Asegurar corrección, evitar deadlocks y evitar desbalances, ya que no se toleran fallos
- Obtener un grado de tolerancia a fallos
- Manejar le hereogeneidad
- Lograr la escalabilidad en todos los casos
- Mejorar la eficiencia energética

#### Descomposición 
Debemos ver el problemas como N tareas diferentes y pequeñas (descomposición de grado fino) para brindar la mayor flexibilidad a los algoritmos paralelos potenciales. 
Dependiendo de la arquitectura, vamos a tener granularidad fina, gruesa o media para descomponer los problemas

La descomposición funcional descompone el cómputo en tareas disjuntas y luego trata los datos. 
...

#### Aglomeracion
Como no sabemos la cant máxima de unidades de procesamiento q se pueden usar, hacemos P procesos ya que no buscamos concurrencia, si parralelismo (buscamos aprovechar el Hw). 
...

#### Características de las tareas
Una vez q se dividieron las tareas, las características de las tareas y "el contexto" (segun yo) van a impactar en la performance alcanzable por el algoritmo paralelo: 
- a
- b 
- c
- a

---
### Mapeo
Especifica dónde se ejecuta cada tarea, un problema no existente en monocores o con un scheduling automático
El objetivo siempre es minimizar el tiempo de ejecución, entonces hay 2 estrategias que a veces conflictúan

...
Cada vez q tenemos tareas, se arma un grafo con dependencias donde se organizan en el mismo nivel a las tareas q se pueden ejecutar a la vez 
Un buen mapping es critico para el rendimiento: 
1. Tratar de mapear las tareas independientes
2. 
3. 

==Problema NP Completo== En teoría de la computación, NP completo es una clase de problemas q requieren un tiempo exponencial para resolverlos, pero para resolverlos podemos usar un tiempo polinomial. En un problema lineal, si queremos hacer una distribución, requiere probar q todas las divisiones por lo tanto es peor al distribuir la tarea.

Si conozco las tareas puedo hacer un **mapeo estático**, donde las tareas se mapean a procesadores a priori:

En el **mapeo dinámico**, mapeo las tareas en tiempo de ejecución, y segun su demanda. Las tareas se generan dinámicamente y no se conoce su tamaño.

Hay que tener en cuenta que una mejor distribución no implica un mejor tiempo. 

--- 
# Métricas del paralelismo
Un sistema paralelo es Hw y Sw. Para medir los algoritmos, vamos a elegir la métrica que más convenga para el problema. 
### Speedup
El speedup (s) es el cociente entre el tiempo de ejecución del mejor algoritmo secuencial(Ts) y el tiempo de ejecución paralelo del algoritmo (Tp): S= Ts / Tp

El speedup optimo tiende a ser igual a la cantidad de unidades de procesamiento q estamos usando, aunque si la arquitectura implica usar cpus con distintas velocidades, se usa el Speedup óptimo:
S = Sumatoria de 0 a P del (potencia de calculo (i) / Potencia de calculo (mejor)

### Eficiencia
Indica qué tan eficiente es el uso de la arquitectura, más allá del tiempo, hablamos del uso óptimo de los recursos. Se calcula como el Speedup dividido el Speedup Óptimo: 
E = S / S(optimo)

El valor está entre 0 y 1 dependiendo de la efectividad del uso de los procesadores, y va a valer 1 cuando el speedup sea perfecto. 

### Escalabilidad
No podemos esperar los mismos resultados de un algoritmo en distintas máquinas. 

---
# Paradigmas de programación paralela

### Cliente servidor

### Master/slave o Master/worker
Se usa cuando la carga es desbalanceada o cuando tengo procesadores diferentes. 

### Pipeline
Cada problema/etapa se resuelve buscando que la solución sea más rápida. P

### Dividir y conquistar
Paradigma recursivo que para un proceso con 2 tareas, y cada una es la misma tarea pero se vuelve a dividir. Una vez que se resuelven las tareas, se juntan los resultados. Cada etapa ordena su pedazo del problema juntando resultads de sus divisiones. 

### SPMD 
Un unico programa que siempre hace lo mismo de forma iterativa (?)
Viendo la matriz de datos, i se usa para filas, j para columnas, y k para recorrer. 


---
# Importancia actual de los sistemas paralelos
Aceleran exponencialmente la resolución de problemas como el recorrido de matrices. Habilitaron a las criptomonedas y a los LLMs (ia).

Hoy en día no se puede esperar a que mejore el Hardware entonces debemos pensar formas de mejorar u optimizar los programas existentes. 
Hay que buscar mejorar el uso de las cosas, porque si bien implique meter más instrucciones, aumenta exponencialmente la eficiencia, aprovechando más los recursos. 

En el siglo XXI se interrumpió la tendencia de los procesadores a duplicar su potencia cada año, principalmente por problemas de temperatura y consumo.
Para acelerar las aplicaciones se comenzaron a usar sistemas paralelos, surgieron los multicores y el concepto de GPGPU. Esto impulsó el uso de arquitecturas híbridas (clusters de multicores con GPUs) y permitió, entre muchas cosas, que la IA sea lo q conocemos hoy. 
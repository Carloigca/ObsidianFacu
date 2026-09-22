#clase
### Técnicas para especificar
Hoy vamos a ver una de las técnicas. Con la profesora Cecilia vamos a trabajar una técnica específica, que consiste en una serie de tareas que dan como resultado un producto. 

- Mediante el camino feliz o escenario de éxito, logramos el objetivo
- Tmb podemos lograr el objetivo mediante un camino Alternativo, en el q no se sigue el flujo normal (camino feliz), pero igual se llega al objetivo. Ej: que me confunda la contraseña una vez y después la vuelva a ingresar correctamente.
- Mediante el camino de Excepción, o error, no lo logramos (objetivo negado). Es similar al alternativo, pero difiere en que llegamos a un punto donde ya no se va a obtener el objetivo. 
  Ej: Que se bloquee la cuenta dsp de un inicio de sesión

Estos workFlow o flujos de trabajo representan un módulo del sistema y se ramifican en multiples caminos posibles. 
## Scenarios
Podemos buscar "Scenarios Leite", o "Scenarios Leite WERPAPERS"
==Julio== Leite es un escritor que propuso la estructura
Los escenarios estan planteados para el mundo real, describen una función del sistema, una situación que encontramos en el dominio, y tienen 6 atributos que los estructuran:
1. Título-Title
2. Objetivo- Goal
3. Contexto- Context
4. Actores
5. Recursos
6. Episodios
Mediante estos elementos, vamos a agrupar  todos los conocimientos del experto. Luego en el código vamos a juntar todos los escenarios similares en un caso de uso
---
#### Context
Es el punto de partida, lo que se da por hecho, lo que se considera para poder entender/describir el escenario. Tmb considerado como las Pre-condiciones
#### Goal
Es a dónde buscamos llegar, lo que debe estar dado una vez que termine el workflow. Tmb considerado como las Post-condiciones, o como la Razón/motivo del escenario.
#### Título
Describe la función, como el ID de las HU vistas en Inge. Aclarando si es un escenario exitoso, fallido, o con qué características. No hay q enumerarlos.
#### Episodios
Son la secuencia de pasos q se dan para llegar hasta el objetivo. Se escriben en lenguaje natural, y en oraciones separadas, precisas y cortas. Deja en claro quién hace una acción, y cuál es esa acción.
Aparece la secuencia, el condicional,, y la no secuencia (Tareas en cualquier orden). Pero el profe solo usa la secuencia (en orden).
No vamos a usar excepciones como elementos, vamos a tratar las posibles excepciones como otros escenarios (de fallo digamos)
#### Actores
Podemos desarrollar Escenarios que sean solo del sistema, o sea que el actor podría ser el sistema, otros sistemas, o el tiempo mismo si llegara a ser necesario. 
#### Recursos
Pueden ser Herramientas, información, insumos. Lo que sea necesario para que se lleve a cabo cada episodio. Por ejemplo si plantáramos tomates, las semillas serían los Recursos. 

---
Solo vamos a especificar escenarios que merezcan el esfuerzo, para funcionalidades que requieran tener ese tipo de especificación. Pero tampoco hay q especificar de más. 
El escenario es el intermedio entre lo q hace el usuario y lo que termina haciendo el sistema. 

Debemos tener en cuenta:
- El criterio para escribir el escenario
- El nivel de detalle que debe tener el escenario
Es dificil definir estos parámetros de forma fija, ya que van a ir variando durante el desarrollo y a medida que el equipo siga trabajando. 
Los escenarios deberían tener entre 3 o 5 pasos, ya que si son menos, faltan detalles, y si son más, sobran. Pueden tener más o menos, pero es lo aconsejable

Los escenarios NO tienen bifurcaciones o iteraciones, ya que siempre van a ser una secuencia de pasos, ya sea que el usuario falla, o que el usuario accede al sistema. 

Podemos usar Agentes de IA, pero debemos leer, corroborar y adaptar todo lo que genere. 

---
### Comparación CU con HU
Las HU tienen los 3 parámetros (Como ==Rol== quiero ==Requerimiento== Para ==Motivo==), y el Motivo está por fuera del sistema, pero nos da una idea de cómo terminar de implementar el sistema. 

---
Probar prompts de ias para que genere Escenarios siguiendo el formato de Leite. Para Login y para el pago de servicios de modo online, con un pago o varios, con transferencia, con una tarjeta, o con varias.
Vamos a tener un Google Doc para poner el prompt y el resultado. para el lunes o martes. El miercoles nos da el Feedback
Podemos poner varios Prompts por grupo

Prompt: 
Genera los Scenarios Leite necesarios a partir de la siguiente especificación: 
El sistema debe permitir que un usuario inicie sesión con su email y contraseña, y solo tiene 3 intentos para ingresar correctamente sus datos. En caso de ingresar erróneamente sus datos, el sistema le debe informar del error, y en caso de que llegue a los 3 intentos, el sistema le deberá prohibir el acceso e informarle que envíe un mail a la administración.

---

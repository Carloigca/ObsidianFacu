#clase 
**Riesgos**: eventos inesperados q pueden afectar el desarrollo del proyecto
##### Gestión de Proyectos
Triple restricción: Se grafica con un triángulo, son las 3 restricciones de la gestión de proyecto:
- Alcance: Todo lo q debemos hacer en el proyecto. Se hace el WBS. 
- Tiempo: Se suele hacer diagrama de Gantt, pero antes se debe planificar todo 
- Costo: El cómo se ingresa y gestiona el dinero. A veces ingresa en partes

Por fuera de la triple restricción, otros aspectos de la gestión de riesgos son:
- Calidad (Se agrupa junto con la triple restricción, creo q tenía otro nombre)
- Integración (integra y agrupa los 9 aspectos)
- Riesgos. Tienen probabilidad, e impacto 
- Stakeholders
- Comunicación
- Recursos Humanos
- Subcontratación o tercerización.

---
### Proceso de construcción de un glosario LEL
1. Identificar simbolos
2. Categorizarlos (Según sujeto, verbo, objeto, estado (Clase pasada))
3. Describirlos
4. Identificar sinónimos. Al trabajar con un dominio particular, los significados son propios del dominio y puede pasar que vayan variando. Puede pasar que algunas palabras sean sinónimos o estén relacionadas, por más que según su significado de la RAE no esté relacionado 

Hay un autor q habla de las características esenciales del software. El software es complejo por naturaleza, debe incorporar conocimiento del dominio sin simplificarlo. Si simplificamos el dominio, el sistema no resuelve tan bien los problemas, no los abarca bien. 
El soft es conocimiento empaquetado. 

---
Las categorías están relacionadas. Sujeto y objeto son sustantivos, y un estado es un verbo (participio pasado)
Puede darse el caso particular donde una misma palabra esté en varias categorías, o que esté mal categorizada. 

---
Las oraciones Kernel (Kernel Sentences) son las oraciones compuestas por sujeto + Accion + Objeto.
El sujeto es un actor (fuera del alcance o decisión del sistema), la acción es una funcionalidad del sistema, y el objeto es un dato del sistema. 

---
## Etapas de procesamiento de LEL y Scenarios con ia
#### Pre procesamiento y transcripción
- Mejorar la calidad de transcripción y revisar palabras
- Dividir el texto
#### LEL
- Identificar sinónimos
- Corregir
- (((Ver foto)))

#### Scenarios
- 1 escenario por verbo
- La noción da origen al objetivo
- Impactos -> episodios
- Actor/recursos -> LEL
![[Pasted image 20260923190512.png]]

---
## WBS
Estructura de desglose de tareas. Cuando medimos el alcance de un proyecto, se arma un arbol  donde las hojas son las tareas que deben tener un seguimiento en el proyecto. La raíz o nivel 0 es el proyecto como un todo. Después cada nivel se organiza según el proyecto. No es necesario que todas las hojas estén en el último nivel, o que tengan una cantidad fija de hojas. Esto nos ayuda a evaluar el esfuerzo necesario para el proyecto. 
Por ejemplo, nivel 0 es el proyecto, nivel 1 las sprint, nivel 2 las HU, nivel 3 los scenarios.


En los asistentes como Taiga juntamos el LEL con los Scenarios (Sce) para organizar mejor el desarrollo. 
Facilita la visibilidad del avance


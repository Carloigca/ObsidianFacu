#clase 
... Habló el inicio del tema, que nos va a llevar más de una clase

La deuda técnica aparece ene l diseño de bases de datos. Mmismo concepto pero ditinta a la de objetos2

El diseño de un esquema de bases de datos es uno de los factores más importantes de los que depende el éxito de la base de datos relacional. 
### Normalización
Vamos a ver la forma de hacerlo mannual para poder entender qué pasa por atrás cuando lo hacemos automático. 
Nos sigue preocupando la normalización porque las bases de datos relacionales se siguen usando mucho hoy en día. Puso en el ppt una comparación de otros tipos de bases de datos, y los principales motores. La normalización es necesaria en la industria

---
### Conceptos generales
#### Anomalías
Surgen a partir del diseño de una relación en el esquema q estoy analizando. Generan inconsistencia, y se deben concer de antemano 
Podría darse que en el esquema
Al trabajar con normalización y diseñar, es importante saber qué significan los atributos (su semántica), ya que nos ayudan a notar las anomalías.
Borrado, inserción, actualización, duplicado, 

#### Dependencia funcional (df)
Concepto fundamental en la Normalización, es una restricción de la BD. 
No entendí bien el ejemplo q puso con tuplas y nros
En el ejemplo PERSONA (DNI, nombre, edad, fecha_nac) hay dependencia porque desde DNI puedo tener todos los datos.
Ver los ejemplos de df en la teoría, xq dependiendo de la semántica de los atributos, vamos a encontrar más o menos df's. 
PRIMERO leemos la semántica, y después buscamos las df's (Anomalías), SIN ASUMIR nada

#### Dependencia funcional trivial
Es un caso especial de la df. Cuando tenemos unos atributos donde el conjunto x define al y, los atributos de y deben estar en X (teoría de conjuntos)
X -> Y entonces Y esta contenido en X. X determina a Y, e Y está contenido o parcialmente contenido en X (?)
Si un conjunto determina a otro, el conjunto determinado se encuentra dentro del conjunto determinante. 


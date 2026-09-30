#clase 
SP es como una unidad donde procesamos algo, nos da resultados, y le podemos dar parámetros 

#### Funciones
Se arma el cuerpo como en las funciones, pero acá agregamos un return. 
La diferencia con las funciones es que las funciones siempre devuelven un valor

==SELECT INTO==. Funciona igual q el SELECT, pero este pone el valor seleccionado en una variable, mientras q el SELECT común devuelve una o mas tuplas

---
#### Excepciones
Situaciones donde hay un error inesperado. Las identificamos, levantamos (Capaz), capturamos y manejamos. 
Evento q interrumpe el flujo normal de una consulta, implicando una condición inesperada o problema con la BD. 

CONTINUE: El programa actual continuará la ejecución del procedimiento 
...

mysql_error_code: es un literal entero q indica el código de error. Por ejemplo 1051 significa tabla desconocida. 

Sql State son cadenas de 5 caracteres (nros y letras). Por ejemplo 42S02 es de tabla desconocida

El codigo de error y el estado son formas distintas de capturar el error. 

Se puede hacer declare condition_name para dar un nombre a una condición que queramos definir. 
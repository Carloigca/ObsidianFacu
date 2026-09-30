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

Las excepciones se pueden manejar con Declare Handler
CONTINUE: El programa actual continuará la ejecución del procedimiento 
EXIT: Finaliza la ejecución del procedimiento

mysql_error_code: es un literal entero q indica el código de error. Por ejemplo 1051 significa tabla desconocida. 
Listado de código de errores en:
https://dev.mysql.com/doc/mysql-errors/9.7/en/server-error-reference.html

sqlstate_value son cadenas de 5 caracteres (nros y letras). Por ejemplo 42S02 es de tabla desconocida

El codigo de error y el estado son formas distintas de capturar el error. 

Se puede hacer declare condition declarando un condition_name para dar un nombre a una condición que queramos definir. 

Sql warning usa el valor de sql state q comienza con 01
not found usa el valor q comienza con 02
sql exception usa los valores que NO comienzan con 00, 01, o 02

Si no se declara el handler para una excepción, la acción que se va a ejecutar depende de la clase de condición que levantó dicha excepción:
- SQLEXCEPTION: Se termina la ejecución como si hubiera un Handler exit
- SQLWARNING: Se sigue ejecutando como si hubiera un Handler con Continue
- NOT FOUND: Si la condición se generó de forma normal, la acción va a ser Continue, pero si se levantó con Signal o Resignal, la acción será exit

---
### Cursores
Estructura q se usa para almacenar las tuplas obtenidas en una consulta SQL ejecutada. Se los puede recorrer para obtener de a una tupla a la vez

Podemos realizar las siguientes operaciones sobre un cursor: 
- DECLARE: declara cursor
- OPEN: abre el cursor ya declarad
- FETCH: recupera un valor del cursor ya abierto
- CLOSE: cierra el cursor ya declarado

--- 
### Transacciones
Son un conjunto de sentencias que conforman una unidad lógica de trabajo, y que deben cumplir con las **propiedades ACID**. Se manejan dentro de los SP (Store Procedure). 
Se inician con las sentencias "START TRANSACTION" y se terminan con las sentencias "COMMIT" o "ROLLBACK"

##### Propiedades ACID
ATOMICIDAD
CONSISTENCIA
AISLAMIENTO
DURABILIDAD

---
### Triggers
Es un objeto con nombre denntro de la DB que se asocia con una tabla y se activa cuando se da un evento en particular dentro de la misma. Permite automatizar acciones o aplicar una lógica ante el cambio producido
SOLO puede haber un disparador por tabla que corresponda a un momento y sentecia dado.![[image.JTODW3.png]]
Tenemos las instrucciones: 
- {BEFORE | AFTER}
- {INSERT | UPDATE | DELETE}
- FOR EACH ROW
- BEGIN y END
##### Limitaciones
El trigger no puede referirse a tablas por su nombre directamente incluyendo la misma tabla a la que está asociado. Se pueden emplear las palabras "OLD" y "NEW"
OLD es un registro existente que se va a borrar o que va a actualizarse antes de q esto ocurra. Una columna OLD es solo de lectura y requiere privilegios de SELECT. 
NEW es un registro nuevo que se va a insertar o un registro modificado dsp de que se da una modificación. Una columna NEW requiere privilegio de SELECT. Es posible una columna NEW con BEFORE, pero para eso es necesario el privilegio de UPDATE
No puede usar procedimiento mediante la sentencia CALL, y no puede  usar sentencias que inicien o finalicen una transacción. 

Para poder manipular un trigger, el usuario necesita el permiso TRIGGER

---
### Índices
Son archivos auxiliares q usa el DBMS para recuperar registros según algunos criterios de ordenación, que facilitan las búsquedas.

Si tengo una tabla de 10 columnas, no tiene sentido generar 10 indices. Es hasta peor ya que debemos insertar en ambos lados. 

---
# terminar índices


---
### Optimización de consultas

Select_type: 
- SIMPLE
- PRIMARY: Si tengo consultas me va a explicar la mas externa
- UNION
- DEPENDENT UNION
- UNION RESULTA
- SUBQUERY
- DEPENDENT SUBQUERY
- DERIVED

Tipos de Join
- System: La tabla tiene una fila.
- Const: En la tabla solo hay una fila (registro) que coincide con la busqueda. los valores de las columnas pueden verse como constantes por el optimizador. Es rápido porque son leídas una vez. Aparece cuando se comparan todas las partes de una PK o un índice unique con valores constantes
- eq_ref
- ref: Varias filas son leídas para cada combinación de filas de las tablas anterores. ...
- Ref_or_null: Como los valores null muchas veces afectan, metemos índices. Ahora es igual a ref pero contemplamos filas q tengan valores null
- Unique_subquery
- index_subquery
- range
- index
- all. Es el peor caso salvo q sea necesario. 
- possible_keys: Posibles claves usadas para encontrar el resultado
- key:
- key_lenght
- ref. Cant de constantes o columnas usadas con la clave para seleccionar las filas
- rows: cant de filas **estimadas** necesarias en la ejecución
- Extra: información adicional sobre cómo se ejecuta la consulta

---
### Seguridad de los datos
Va medio rápido. 
La triada de seguridad es más q nada para ciberseguridad, ya q hacen mucho hincapié en eso:
C: Confidencialidad
I: Integridad
D: Disponibilidad

Podemos respaldar la BD con BACKUP DATABASE indicando dónde dejar el backup
Podemos restaurar la BD con RESTORE indicando dónde buscar el backup ya hecho. 

#### Errores de la configuración
Se usó el usuario root con una contraseña muy conocida. Este usuario solo se debe usar en casos de emergencia. 
Después los usuarios de Tomás y Juan no deberían tener todos los derechos. Está mal que todos los usuarios o varios tengan esos permisos/derechos, ya que se pueden confundir. 
La password se guardó así nomás en la tabla. Hay que encriptarlas previamente así como cualquier información sensible del usuario
Tmb acá habló de los entornos de desarrollo y de no sé qué más

Respaldo: Está bien respaldar distintas bases de datos, pero no es suficiente con guardarlo así nomás en la computadora. 
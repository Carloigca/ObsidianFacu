Es un lenguaje especialmente diseñado para manipular y consultar bases de datos relacionales q se convirtió en un estándar. 
Se basa en el álgebra relacional y el cálculo relacional, y se compone por un lenguaje de definición de datos (DDL), un lenguaje de manipulación de datos (DML), y un lenguaje de control de datos (DCL)

#### DDL
El DDL nos permite definir la estructura y características de los elementos de las bases de datos como son las tablas, colunas, claves primarias y foráneas o restricciones. 

(((Comenzó a ir rápido el profesor)))

Las tablas no se pueden nombrar con palabras reservadas como USER

CREATE para crear tabla
ALTER para modificar cosas de la tabla
DROP para borrar la tabla (Solo el creador de la tabla o el usuario root pueden ejecutar esta sentencia)

##### Restricciones
(((Creo q me falta anotar las primeras)))
Pueden existir tablas sin Primary Key pero es una mala práctica. 
Con Not Null nos aseguramos que el atributo no sea nulo.
Con Unique marco las claves alternativas. o los atributos que uso para una clave natural.  No conviene tener más de un atributo como Primary Key, ya que si necesitamos usar esa PK como FK, vamos a tener q repetir esos campos en otra tabla . 
Con Default establecemos un valor por defecto para el campo.
Con check especificamos una condición que el atributo debe cumplir. Por ejemplo (que una letra sea de A a F)
Con autoincrement establecemos q un atributo se vaya incrementando en cada tupla q se crea, en la tabla solo puede haber un atributo con esta restricción

#### DCL
Los usuarios no se eliminan, se hacen bajas lógicas. 
Con CREATE USER y DROP USER creamos y eliminamos un usuario e indicamos el host donde se establece la conexión. 
Con GRANT otorgamos permisos, y con REVOKE le sacamos permisos. Tenemos GRANT ALL, y REVOKE SELECT ON.

El control de acceso consiste en: 
1. Comprobar si el usuario tiene permitido conectarse
2. Se comprueba cada comando ejecutado por el usuario para ver si tiene los permisos para hacerlo. 
Si cambian los permisos, estos cambios se reflejan recién al arrancar el servidor de la BD, cuando se cargan las tablas de permisos los cambios no tienen efecto inmediato a no ser que lo fuerce (con el comando FLUSH PRIVILEGES)

#### DML
Nos permite modificar los datos de las tablas.
Tenemos las operaciones: 
- INSERT 
- UPDATE
- DELETE. Deja un Log cuando se ejecuta, por lo q se puede ver qué se hizo. Al igual que con el Update, se debe agregar siempre el Where, y debe estar bien usado. Un DELETE sin where elimina toda la tabla.
- TRUNCATE. Borra todos los registros de la tabla sin dejar un Log a diferencia del Delete
- SELECT
- SELECT WHERE
- JOINS 
- ORDER BY. SIEMPRE va a ser la última sentencia en ejecutarse, por eso siempre se escribe último 
- GROUP BY
- HAVING

##### Subconsultas
Podemos usar EXISTS o IN.
El EXIST corta la ejecución cuando enncuentra una tupla q cumpla la condición, es más "Performática"
Primero se resuelven las subconsultas, luego las consultas que las englobaban. 

---
### Vistas
Una vista es una representación lógica de un subconjunto de atributos de una o más tablas (NO es una copia). Es una tabla lógica q se basa en otra tabla o conjunto de tablas. 
Una vista no se puede llamar como una tabla, ya que comparten el mismo espacio de nombres.
Restricciones: sirven para restringir el acceso de datos, Mostrar solo algunas columnas o filas de una tabla a determinados usuarios, nos permite hacer consultas complejas de forma más facil de usar para los usuarios, Nos permite hacer una vista q oculte una consulta q reúne a varias tablas.
Podemos presentar diferentes vistas de los mismos datos. 
...

---
Store Procedures (SP)
Son bloques de código almacenados q se pueden ejecutar repetidamente por el nombre q le demos.  Se almacenan en el servidor de forma pre-compilada, de modo que se pueda ejecutar más rápido q  una consulta normal. Sirve para poder llamarlos desde aplicaciones externas 

Nos proporciona: 
- Acceso homogéneo: de modo que cuando se necesiten realizar las mismas operaciones en distintas aplicaciones (Con diferentes lenguajes), se utilicen
- Consistencia de las operaciones: Para cada ejecución, la secuencia de instrucciones retorna los resultados de manera consistente 
- Seguridad: Permite NO acceder a las tablas directamente, solo por medio del SP 
- Mejora la Performance: se disminuye el tráfico entre el servidor y el cliente 
- Consultas complejas: tenemos estructuras y funciones adicionales para resolver las consultas complejas

Pueden recibir 3 tipos de parámetros: IN, OUT, e IN/OUT

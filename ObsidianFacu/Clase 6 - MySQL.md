#clase 
Qué es un DBMS? MySQL, PostGre, etc. 

Está compuesto por 
- DDL (DataDefinitionLanguage). Permite definir estructuras con los q
- DML (Data Manipulation Language). CRUD, las operaciones
- DCL (Data Control Language). Permisos de usuarios, el control
- Data Dictionary. Las variables propias de las bases, buffer, cant. de usuarios q pueden acceder, el cómo se guardan los datos en disco. 

Características
- Abstraccción de la info
- Independencia
- Redundancia minima
- Consistencia
- Seguridad
- Integridad
- Respaldo y recuperación
- Control de la concurrencia
- Tiempo de Respuesta

---
##### Abstracción de la info
a

##### Independencia
debo poder modificar el esquema de la bd sin tener q hacer cambios en las apps que la usan

##### Redundancia minima
Hay que evitar la aparición de info repetida o redundante. El objetivo debería ser lograr una redundancia nula, aunque en algunos casos la complejidad hace necesaria la aparición de redundancias

##### Consistencia
a

##### Seguridad
Si bien solemos pensar en hackeos, en realidad los problemas de seguridad suelen ser internos (que un usuario sin querer modifique mal la tabla). Por eso se deben organizar los permisos de usuarios para establecer quiénes pueden modificar qué cosas

##### Integridad
Ante fallos de Hw o de usuarios, debemos asegurar que no se corrompan los datos.

##### Respaldo y Recuperación
Lo primero q necesitamos es conservar la información haciendo backups. Y debemos poder restaurarla. Por lo general se hacen backups en el mismo server de la DB, lo que está mal. Además tampoco se suelen probar los backups, donde pueden aparecer corrupciones que nos imposibiliten recuperarlos 
Cómo hago? Con qué estrategia? Ante un fallo debemos poder levantar todo lo antes posible

##### Control de concurrencia
Hay cientos o miles de usuarios accediendo en el mismo momento, entonces hay q controlar la concurrencia para que no se generen las inconsistencias

##### Tiempo de respuesta
El acceso debe ser eficiente minimizando el tiempo en el que el DBMS da la información solicitada.

---
Los motores de BD son:
- Relacionales
- NoSQL (BD2): Documentos, Clave-valor, Grafos, y columnas

### BD Relacionales
Son BD generalmente muy grandes, probadas y que cuesta migrarlas a otros modelos. Siguen siendo las más usadas. 
Los datos se almacenan en tablas o relaciones, compuestas de Filas/tuplas y Columnas/campos. 
Todo se fundamenta en la teoría de las BD Relacionales de Codd, que es matemática y tiene un conjunto de reglas para su organización.

---
#### Claves Primarias
Conjunto de atributos de la tabla que cumple con unicidad y no nulidad.

#### Clave Foránea
Tiene el Id de otra tabla, establece una relación con esa otra tabla y se utilizan para mantener la integridad referencial q asegura la coherencia de los datos relacionados.

---
### MySQL
Gestor de BD propiedad de Oracle, que tiene 2 versiones (comunity y enterprise)
MariaDB es muy similar a MySQL pero últimamente se comenzó a diferenciar 

Se controla por consola y existen distintos IDEs que dan soporte como Workbench o DBeaver (multi-motor)
Al instalarla, se crea el superusuario "Root". Es el usuario propietario del Data Dictionary, que se suele usar de entrada pero que no debe usarse siempre porque debe ser privado de los que gestionan la BD

Tiene motores de almacenamiento q determinan como se organizan, almacenan y manejan los datos en el sistema. 
MySQL tiene soporte para distintos motores de almacenamiento, como InnoDB, MyISAM, Memory (es raro), etc. Para ver los distintos motores, existe el comando "Show Engines". 
InnoDB es el motor por defecto que ofrece soporte para las transacciones que cumplen con las propiedades ACID, garantizando la integridad referencial y la recuperación ante fallos.

Memory mantiene los datos en memoria (HEAP)

---
[[SQL]]

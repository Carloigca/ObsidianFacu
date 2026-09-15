#clase
# Modelo de datos

Notación especifica para los datos con los q voy a trabajar
Siempre voy a tener una estructura y unas construcciones respecto a esos datos. Ocasionalmente voy a definir operaciones con esos datos. 

Dentro del modelo de datos tenemos una clasificación a nivel de qué es lo q estoy representando:
- Modelos lógicos
	Se alejan del hardware, son mas teóricos. Tengo 2 tipos de modelos principales
	- Basados en objetos
		- Modelos orientados a objetos
		- Modelo de entidades y relaciones
	- Basados en registros (modelo relacional)
- Modelos físicos
	Tienen base en algun Hardware específico

## Modelo de Entidades y relaciones
Se define a partir de una estructura y unas relaciones
Su estructura son: Las entidades, relaciones y atributos
- Entidad: Es una "cosa o concepto" que puede ser identificada y distinguible de otra "Cosa o concepto" (No es necesario q sea algo tangible/real) 
- Relación: Es una asociación entre 2 entidades
- Atributo:  
- Dominio de un atributo: Conjunto de valores q puede tomar un atributo particular
- Conjunto de entidades: Hay q diferenciarlo de las entidades. Ej: "Personas" o "Empleados"
- Conjunto de relaciones: Lo mismo q lo anterior

Restricciones:
- Cardinalidad: Tenemos "uno a uno", "Uno a muchos", "Muchos a muchos" (no se si hay otra). ponemos la caridnalidad de la entidad cerca de la punta opuesta. Si junto Persona y Auto, a la derecha pongo 1,n xq una persona puede tener uno o muchos autos. Y a la izq pongo 1,1 xq un auto solo tiene un dueño
- Nombres: No puede haber nombres duplicados en entidades, atributos o relaciones. 
- Notación gráfica: Rectangulos entidades, rombos relaciones, chupetines los atributos, y cardinalidades minima y máxima. Las claves son atributos con el circulo lleno. Las relaciones pueden tener atributos, pero no atributos clave. 
### Rol de una entidad en una relación
Las entidades juegan roles en las relaciones. O sea que por ejemplo una persona puede tener una relación con una persona (como por ejemplo ser supervisor/jefe de otra)

En los ejemplos suelen omitir atributos, pero una entidad DEBE tener al menos un atributo, ya que debe tener al menos un identificador (q es un atributo). Por eso aunque un ejemplo no tenga todos los atributos, nosotros tenemos q poner todos. 

## Especialización y Generalización
### Especialización
Es el resultado de tomar un subconjunto de entidades de un nivel para formar un conjunto de entidades de nivel más bajo. Teniendo Persona, genero Alumno y Profesor. ==Puede haber otros tipos de Personas==

### Generalización
Es cuando decimos que todo el conjunto de entidades de nivel más bajo son todas del nivel mas alto. Teniendo Alumno y Profesor, genero Persona. ==No puede haber otros tipos de personas==

## Agregación 
No podemos tener atributos "opcionales" Siempre deben tener un valor como "Si" o "no" . Nos quedamos solo con relaciones binarias xq en las ternarias o de más, se confunde la interpretación de la cardinalidad. 
La agregación es la solución para querer hacer relaciones con relaciones (O querer agregar atributos para guardar cosas). Podemos estar seguros de q hay q poner agregación, si la relación se debería repetir cambiando de atributo. 
En la agregación agarro dos (o más supongo) entidades relacionadas, las encapsulo en un rectángulo, y de ahí saco una relación para con otra entidad (la que genero en base al resultado de la relación englobada). El ejemplo en la teoría es "compañía" - entrevista -"solicitante_Empleo" 
Y del resultado de esa entrevista se saca la relación con una nueva entidad llamada "Oferta_de_empleo"
La agregación SOLO se usa cuando en la relación original (Compañía - Solicitante) tengo N en la cardinalidad máxima de los dos lados. 

---
### Modelo relacional
Tmb tiene estructura y restricciones:
Estructura:
- a
Restricciones:
- b
---
### Tranformación 1 a 1 desde el modelo de entidades y relaciones al modelo relacional: 
Estos modelos son independientes, pero se puede llegar de uno al otro mediante un camino. Si tengo una entidad en el EyR, voy a pasar a tener un esquema en el Relacional.
Si tengo una entidad en el modelo de EyR, y le aplico la transformación 1 a 1, la Entidad pasa a ser un esquema de entidad (), y la relación pasa a ser un esquema de relación (una tabla igual q la de entidad). Hay 2 reglas, y la 2da tiene 3 puntos.

#### Relación: 
Teniendo una relación, necesito definrile un identificador para el esquema de entidad, y puedo poner como identificador a cualquiera de los atributos de cada tabla, pero no a lso dos.
En Profesor - Dicta - Curso, termino teniendo 3 esquemas, donde Dicta usa como identificador el identificador de Profesor o el de alumno, aunque use los dos para representar la relación previa. 

#### Rol:
Teniendo Persona con relación a otra persona

#### Generalización
Tenemos 3 estrategias  para resolverlas:
- Tabla para el conjunto de entidades de nivel más alto, donde agregamos un atributo "Discriminante" que diferencie a las "Subtablas" de la de nivel más alto. Ej: Si tengo alumno, profesor y Persona, hago tabla persona con el "atributo" discriminante "tipo"
- Tabla para cada conjunto de entidades del nivel más bajo, dejando dos tablas o más con atributos repetidos (dni, nombre, f.nac, etc)
- Una tabla para el conjunto de entidades de nivel más alto y una tabla para cada conjunto de entidades del nivel más bajo. Hacemos tabla para todos, pero hay información repetida (Todos los alumnos y profesores identificados con su dni, están en Persona tmb)
#### Especialización
Tenemos 2 estrategias (descartamos la 2da estrategia anterior):
- Tabla para el nivel más alto
- Tabla para todos los niveles

#### Agregación
Suponiendo q tenemos la agregación de la relación entre 2 entidades, de la q sale una relación a una 3era entidad
En la tabla de la relación agregada a la 3era entidad, ponemos como identificador a los identificadores de las 3 entidades. 

---
### Ejercicio:
![[Pasted image 20260826093834.png|520]]

---
#### Modelo entidad-relación
Siempre q queramos hacer los modelados entidad-relación. Hay q relevar(preguntar) lo más posible del dominio antes de asumir cosas. 

La profe separó lo que se debe hacer de lo que realmente se hizo. 
Dijo que el mantenimiento compuesto por una tarea es lo que se va a hacer
El servicio pagado por un cliente y realizado por un empleado es algo que realmente se hizo (para la bd ya está dado, registrado)
![[Pasted image 20260826093757.png|490]]

##### Transformación 1 a 1 para modelo relacional
Cliente(#Cliente, Domiclio, CUIT, CUIL, Razon_social, Nombre_apellido)
//-Para Jurídica y Física dejé sus atributos como opcionales
EsDueño (#Cliente, Patente) //Uso Patente como ID
Vehiculo (Patente, Modelo, Marca)
Tiene (Patente,#Motor) //Uso Patente como ID
Realiza (Patente,#Servicio, Km_Actual) //#Servicio como ID
Servicio (#Servicio, Monto, Fecha_egreso, Fecha_ingreso, Motivo)
Posee (#Servicio,#Detalle) //#Detalle como ID
Detalle (#Detalle, Precio, Descripción)
Realizado_por (#Detalle, CUIL)
Empleado (CUIL, Nombre_apellido, Fecha_nacimiento, Fecha_ingreso, Turno)
//-Para Administrativo dejo turno en Empleado como opcional
Contiene (#Detalle,#Mantenimiento,#Tarea) //TODO es ID
Mantenimiento (#Mantenimiento, Km, Nombre)
Compuesto por (#Mantenimiento,#Tarea, Valor) //#Mantenimiento,#Tarea como ID
Tarea (#Tarea, Descripcin, Tiempo)
![[Pasted image 20260826093542.png]]

---

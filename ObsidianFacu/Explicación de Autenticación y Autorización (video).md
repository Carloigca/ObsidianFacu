#extra 
#### Autenticación
El sistema debe poder determinar quién se está conectando al sistema para usar sus funcionalidades. Es el "Quién es?"
#### Autorización
Una vez q se conocen los usuarios, se debe revisar si los usuarios tienen permisos para ciertas acciones dentro del sistema. Es el "Qué puede hacer?"

---
### Modelos
Todo se esconde detrás del concepto de un usuario. Un usuario usa el sistema y nosotros lo debemos modelar. 
Admin> src> core (ya que en web vemos cómo disponibilizar el sistema, mientras que en core tenemos lo relacionado al negocio) 
core > auth > tenemos user.py, role.py, permisos.py 

Un usuario tiene un rol -> un rol tiene N permisos -> 
Cuando el user hace un request, validamos al usuario, después  vemos el permiso para saber si lo dejamos pasar o se lo negamos

Los 3 tipos de Entidad heredan de BaseModel (definido en una clase previa), donde podría poner el "created_at" y "updated_at". 
#### User.py
![[Pasted image 20261007212253.png|400]]
Tenemos la información del modelo en la BD. 

#### Role.py
![[Pasted image 20261007212331.png|416]]
Agrupa distintos permisos para asignar un rol a un usuario

#### Permisos.py
![[Pasted image 20261007212432.png|420]]
En los controladores vamos a tener endpoints distintos, y cada uno debe tener asociado un permiso para indicar si un usuario puede o no acceder al endpoint. 

Hasta el min 10 mostró

En init.py puso las funciones básicas y necesarias. Hasta el minuto 22:00 las mostró. 

---
## Seeds
No es fundamental, pero hay q implementarlo. Como necesitamos datos pre-cargados para usar el sistema (permisos, roles, algun usuario para iniciar sesion) cuando lo iniciamos, es un tema crear todo, y para eso usamos las Seeds
Son datos q se crean de forma automática para que el desarrollo sea más comodo. Hay seeds de producción tmb como permisos, roles de u8suario y de admin, y dsp hay seeds q son más de desarrollo, por ejemplo crear un user por defecto(con nombre admin, un usuario por cada rol, etc). 
Casi todo es información base que necesita el sistema en producción, excepto la creación de un usuario admin
![[Pasted image 20261007235814.png|423]]
auth es el blueprint para lo que es autorizaciones.
Admin ya se crea con permisos, mientras q los otros roles se crean sin permisos al inicio, aunque obviamente nosotros lo podemos cambiar 

Después en web > init Importamos los seeds, y creamos una función con el comando seeds, donde le indicamos con qué parámetros crear el usuario inicial. 
al poner el comando seeds, se corre todo lo que esté en el archivo. En producción no se corre este comando, ya que es solo se corren de forma local, y no tenemos acceso a la terminal para correrlo. 
Para poder usarlos, se puede usar un if que indique: si el env. es producción, se puede llamar a seeds (Yo entendí q solo se podía en desarrollo, se debe haber confundido).
En producción necesitamos datos creados, entonces para eso podemos llamar la función seeds 

---
## Sesiones
Min 33:10
Qué es una sesión? Como HTTP no tiene memoria de forma nativa, se añaden características de contexto o memoria para las request. Eso es la sesión, una pequeña memoria q nos permite conservar la info de un usuario durante un intervalo de timepo. Nos permite hacer cosas q con HTTP puro no podríamos. 

Para las sesiones usamos la librería Flask Session. Nos permite administrar las sesiones de un usuario de forma simple. 
Desde la página de flask session se pueden ver configuraciones. Usamos la version filesystem.  
![[Pasted image 20261008001422 1.png|421]]
Para configurarla vamos al archivo config. y seguimos los nombres que muestran en la página oficial para configurarla (en este caso las cosas en mayus). Hay q tener cuidado con lo de las cookies (?) 
La configuración no va a depender de si estamos en modo producción, desarrollo o testing. 
Siempre tratamos de q el ambiente de desarrollo sea lo más cercano posible a el de producción, ya q nos garantiza más el funcionamiento de desarrollo en producción. 

Dsp importamos el config desde el init al mismo nivel.  
min 39 aprox muestra cómo aplicar la configuración digamos

min 41:30 aprox pasa al iniciar sesión

---
## Login
En la carpeta Core declaramos la lógica de negocio, mientras que en la carpeta web declaramos lo relacionado a HTTP 
Dentro  de los controladores de web, tenemos auth.py donde declaramos el Blueprint, y el endpoint get de login, que en este caso carga el template básico de login creado en la dirección templates>auth>login.html![[Pasted image 20261008092338.png|565]]

![[Pasted image 20261008092055.png|573]]

Cuando el usuario carga sus credenciales, se impacta el Post Autenticate.

El endpoint Post Autenticate usa las funciones definidas en el init.py de core>auth
Una vez que validamos al usuario, limpiamos la session y dsp guardamos los datos del usuario que queramos, ya que es una memoria que funciona como un diccionario
![[Pasted image 20261008093112.png|428]]

#### Logout
Si la sesion no estaba iniciada, session no debería tener un id guardado
![[Pasted image 20261008093303.png]]

---
### Flash
Los flash usados en estos endpoints son para dar avisos rápidos sin necesidad de crear más contenido tipo HTML. Se pone flash con dos parámetros: el primero es el texto a mostrar, y el segundo es el tipo de aviso, que si es error se muestra rojo, si es success verde, y después están info y warning también. 

En el archivo home.html declaramos el html de las flash
![[Pasted image 20261008094102.png|461]]
![[Pasted image 20261008094133.png|497]]
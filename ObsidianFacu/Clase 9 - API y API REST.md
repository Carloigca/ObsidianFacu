#clase 
Vamos a hacer API pero tmb vamos a ver API REST. 

El controlador HTML recibe la petisión como una ruta, y sabe qué función da la respuesta.
A parte tenemos el controlador API que sabe cómo servirle al modelo las funciones de la app JSON

En el modelo chequeamos cosas, pero en el controlador API tmb. 

---
Una API no siempre es una API web o por HTTP. Los SO tienen APIs que corren sobre el sistema. Una API también puede usar HTTP y no ser API REST. 
Al desarrollarla, el q la use debe conocer lo q puede hacer y las respuestas q puede obtener. 

---
Un ==recurso== puede ser algo individual o una colección de objetos, una solicitud, una colección de solicitudes. No es necesariamente una tabla física de la BD, sino q puede ser una solicitud, un servicio, un proveedor. 
Una ==URL== nos permite identificar el recurso q estamos poniendo. 
El ==Endpoint== no es solo la URL, sino tmb el método HTTP. Ya que con la misma URL podemos hacer GET, DELETE, etc. 

---
JSON es un formato para la representación de un recurso q definimos nosotros. Para cada recurso q vamos a exponer, hay q definir bien qué atributos exponemos y cómo. 

---
REST es algo a nivel arquitectónico:
- es cliente-servidor, y las solicitudes son sin estado de sesión en el servidor. O sea q el sv no tiene memoria de las peticiones
- Caché, capas, e interfaz uniforme
- Y código bajo demanda, es opcional

Content Type es el formato enviado, y el Accept es el formato aceptado 

PUT crea o reemplaa el estado representado en la URL
PATCH aplica modificaciones parciales definidas
PUT no es PATCH con otro nombre, son distintos. 

Consideramos seguros a los métodos que no solicitan cambiar (modificar) el estado del recurso. 
Consideramos idempotente a los métodos que al ejecutarlos no cambian el resultado ni modifican más los recursos. 

GET es seguro e idempotente
PUT/DELETE no es seguro y es idempotente (Por más que el código de estatus sea distinto en cada pedido)
POST/PATCH no es seguro y no está garantizado q sea idempotente

---
Errores:
- 400: Bad Request
- 415: 
- 422: JSON válido, datos que no cumplen el contrato. Para validación y lo documentamos
- 404: No se encontró la solicitud individual pedida
- 409: Conflictos
- 500: Fallo inesperado en el servidor. 
- 200: La colección buscada existe, aunque no hay conicidencias (Se devuelve una lista vacía)
- 204: se eliminó correctamente (con DELETE) un recurso. El response viene vacío

---
#### Operaciones de la API
- Listar (GET)
- Consultar una (GET con especificación en la URL)
- Crear (POST)
- Cambiar cantidad (PATCH)
- Borrar si el dominio lo permite (DELETE)

Para eliminar una solicitud, por lo general el dominio va a necesitar q hagamos bajas lógicas pero puede darse q debamos hacer físicas, y por eso aclaramos "si el dominio lo permite". 

---
Marshmallow
En la diapo 21 de la clase muestra un ejemplo de los campos q se aceptan, si son obligatorios, y si tienen valores mínimos o máximos. 
Podemos definir los mensajes que se van a devolver para los casos donde el dato es menor al minimo o mayor al máximo. 
El cliente de la API no sabe los datos q se guardan. Se crea un o varios esquemas genéricos y los podés usar en distintos métodos de la API. 
Usa dump y load, y nos termina dejando duplicadas las cosas en el ORM y en el Marshmallow. 

Donde ponemos las solicitudes API, ponemos Blueprint. No terminé de entender bien cómo se implemente marshmallow ahí tmb. 

---
Lo que testeamos con curl o con Postman, o Insomniac, no nos asegura q funcione en el navegador (por CORS). 

Necesitamos una ==estrategia de deduplicación== para evitar que haya solicitudes repetidas (en caso de q se pierda la respuesta del servidor). 
Por ejemplo si se manda un POST igual dos veces, se vuelve a crear el dato en la BD. 

---
##### Documentación
Open API nos permite describir la API HTTP de forma estructurada. 
### **1. Conceptos básicos: Autorización y Autenticación**

Estos dos mecanismos resuelven problemáticas completamente distintas en la seguridad de un sistema [cite: 48, 49]:

```
Identificación   ──►   ¿Quién dice ser la persona?    (ej. "usuario@hospital.gob.ar")
Autenticación    ──►   ¿Cómo prueba su identidad?    (contraseña, hash, 2FA, Google OIDC)
Autorización     ──►   ¿Qué acciones puede realizar? (reglas de negocio y permisos en el backend)
```

- **Autenticación**: Proceso mediante el cual el sistema o un proveedor externo verifica la identidad declarada por el usuario [cite: 49]. Por ejemplo, validar una contraseña contra un hash almacenado o verificar un proveedor externo con **OpenID Connect (OIDC)** [cite: 4, 11, 49].
- **Autorización**: Mecanismo que evalúa si un usuario ya autenticado cuenta con los permisos necesarios para ejecutar una acción o acceder a un recurso específico [cite: 48, 49]. **Estar autenticado no implica tener permisos** [cite: 49]: un usuario que inicia sesión correctamente puede tener permiso para cargar un pedido pero ser rechazado al intentar autorizarlo o ver información de otro sector [cite: 8, 48, 49].
- **Acceso delegado (OAuth 2.0) vs. Capa de Identidad (OIDC)**:
    - **OAuth 2.0**: Es un marco para delegar acceso a recursos protegidos sin entregar contraseñas directas al cliente [cite: 50].
    - **OpenID Connect (OIDC)**: Agrega la capa de autenticación e identidad sobre OAuth 2.0 emitiendo un **ID Token** para que la aplicación reconozca quién inició sesión [cite: 58, 59].
- **Tokens y JWT**:
    - **JSON Web Token (JWT)**: Formato compacto de texto plano compuesto por tres partes: `Header.Payload.Firma` [cite: 59, 63]. **Firmado no significa cifrado** [cite: 64]: el _payload_ está codificado en Base64URL y cualquiera puede leer sus datos (_claims_); la firma criptográfica garantiza únicamente integridad y autenticidad del origen [cite: 63, 64].
    - **ID Token vs. Access Token**: El _ID Token_ es para la aplicación cliente (informa quién se autenticó), mientras que el _Access Token_ es para el servidor de la API (acredita qué recursos puede consultar) [cite: 59, 65].
- **Códigos HTTP de control de acceso**:
    - **`401 Unauthorized`**: Falta de credenciales, token ausente, firma inválida o sesión expirada [cite: 6, 67].
    - **`403 Forbidden`**: La identidad es válida y conocida, pero el usuario no tiene los permisos suficientes para la acción o el recurso [cite: 6, 8, 67].

---

### **2. El DOM (Document Object Model)**

El **DOM** es la interfaz orientada a objetos provista por el navegador que representa el código HTML como un árbol jerárquico de nodos en memoria [cite: 160, 214]. Permite a JavaScript leer, modificar, crear o eliminar elementos de la página de forma dinámica [cite: 214].

- **Tipos de nodos en el árbol**:
    - **Document**: Nodo raíz del documento HTML cargado (`document`) [cite: 215].
    - **Element**: Representa cada una de las etiquetas del documento (`<div>`, `<button>`, `<h1>`, etc.) [cite: 215].
    - **Text**: Representa el contenido textual interno de una etiqueta [cite: 215].
    - **Attribute**: Propiedades asociadas a un elemento (`src`, `href`, `id`, `class`) [cite: 216].
- **Selección moderna**: Se realiza a través de selectores CSS mediante `document.querySelector(selector)` (obtiene el primer elemento coincidente) y `document.querySelectorAll(selector)` (retorna una lista de nodos `NodeList`) [cite: 216, 217].
- **Manipulación de contenido y atributos**:
    - **`textContent`**: Modifica exclusivamente el texto plano sin interpretar etiquetas [cite: 218]. Es la opción segura recomendada [cite: 218].
    - **`innerHTML`**: Reemplaza o interpreta el contenido como marcado HTML [cite: 218]. Debe manejarse con extrema precaución para evitar vulnerabilidades XSS [cite: 218].
    - Modificación de atributos vía `getAttribute()`, `setAttribute()` y `removeAttribute()` [cite: 219].
- **Gestión de estilos y clases (`classList`)**: La buena práctica exige no abusar de estilos directos en línea; se utiliza `classList.add()`, `classList.remove()`, `classList.toggle()` y `classList.contains()` para activar reglas de CSS existentes [cite: 220, 221].
- **Creación y eliminación de nodos**: Inserción en tres pasos mediante `document.createElement()`, configuración de propiedades y agregado al árbol con `padre.appendChild(hijo)` [cite: 222]; o eliminación con `nodo.remove()` [cite: 222, 223].
- **Eventos y `preventDefault()`**: Mediante `addEventListener('evento', callback)` se capturan las interacciones del usuario [cite: 223, 224]. Al trabajar con formularios, se invoca `event.preventDefault()` para cancelar la recarga estándar del navegador y permitir que el envío y validación se gestionen de manera asincrónica [cite: 224, 225].
- **Modos de carga de scripts (`<script>`)**:
    - Por defecto: Bloquea el procesamiento sintáctico del HTML mientras descarga y ejecuta el script [cite: 229, 230].
    - **`async`**: Descarga en segundo plano y ejecuta de inmediato apenas finaliza la descarga, sin garantizar el orden de ejecución [cite: 231, 232].
    - **`defer`**: Descarga en segundo plano y posterga la ejecución hasta que el HTML termina de construirse, respetando el orden estricto de inclusión [cite: 232, 233].

---

### **3. Peticiones Asincrónicas**

- **El hilo principal y el bloqueo del navegador**: JavaScript se ejecuta en el navegador en un único hilo de ejecución (_single-threaded call stack_) [cite: 27, 29]. Este hilo atiende la lógica, escucha los eventos de teclado y ratón, y renderiza la pantalla [cite: 27]. Si una petición HTTP fuera sincrónica y tardara varios segundos, la interfaz quedaría totalmente congelada para el usuario [cite: 27, 28, 29].
- **Modelo de concurrencia y Event Loop**: Las operaciones de red se delegan a las APIs internas del navegador [cite: 29, 30]. Cuando la respuesta del servidor arriba, su función asociada (_callback_) pasa a la cola de tareas [cite: 30]. El **Event Loop** revisa constantemente la pila de llamadas (_call stack_) y, una vez que está vacía, transfiere la tarea para su ejecución sin interrumpir la fluidez visual [cite: 29, 30].
- **Evolución del procesamiento asincrónico**:
    - **AJAX / `XMLHttpRequest`**: Objeto pionero que permitió solicitar datos al servidor sin recargar la página completa mediante estados `readyState` (0 a 4) y manejadores de evento `onreadystatechange` [cite: 31, 32].
    - **Fetch API y Promesas**: Reemplazo moderno nativo basado en objetos `Promise` [cite: 38]. Una promesa representa un valor eventual que transiciona de estado **pending** a **fulfilled (resolved)** o **rejected** [cite: 39]. Permite encadenar operaciones con `.then()` y capturar fallos globales al final con `.catch()` [cite: 40, 41].
    - **`Async / Await`**: Azúcar sintáctica introducida en ES8 sobre las promesas [cite: 42]. `await` pausa la ejecución dentro de una función marcada como `async` hasta que la promesa se resuelva, permitiendo leer y estructurar el código de manera secuencial y controlar excepciones con bloques `try / catch` [cite: 42, 43].
- **Mecanismo CORS (Cross-Origin Resource Sharing)**: Es una política de seguridad enforced por el navegador cuando una aplicación cliente solicita recursos a un origen distinto (diferente protocolo, dominio o puerto) [cite: 44].
    - En peticiones complejas (como aquellas con métodos `PUT`/`DELETE` o con encabezados `Authorization`), el navegador emite automáticamente una solicitud previa (_preflight_) mediante el método `OPTIONS` [cite: 19, 44, 262].
    - El servidor debe responder con cabeceras `Access-Control-Allow-Origin`, `Access-Control-Allow-Methods` y `Access-Control-Allow-Headers` [cite: 19, 44]. **CORS es una restricción del cliente web; no reemplaza la autenticación ni la autorización del backend** [cite: 262].

---

### **4. Desarrollo con Vue.js**

- **Framework Progresivo y Single Page Application (SPA)**: A diferencia de las arquitecturas tradicionales donde el servidor compila y envía páginas HTML completas ante cada clic [cite: 82, 183], en una SPA con Vue el navegador carga la aplicación una sola vez [cite: 82]. La vista y el estado residen en el cliente, y la comunicación con el servidor en Flask se realiza intercambiando exclusivamente datos en formato **JSON** [cite: 80, 82].
- **Patrón MVVM, Reactividad y Virtual DOM**:
    - **MVVM**: El **Model** son objetos JavaScript planos; la **View** es el DOM; y el **ViewModel** es la instancia reactiva de Vue encargada de sincronizarlos mediante enlaces de datos y escuchas de eventos [cite: 88, 89].
    - **Virtual DOM**: Vue mantiene una copia ligera del árbol DOM en memoria [cite: 89]. Al mutar el estado reactivo, Vue calcula la diferencia mínima (_diffing_) y actualiza en el DOM real únicamente los nodos modificados (_patch_), evitando repintados masivos costosos [cite: 89].
- **Single-File Components (archivos `.vue`)**: Desarrollados con herramientas de andamiaje como **Vite** [cite: 91, 117]. Agrupan en un solo archivo: `<script setup>` (lógica en JavaScript), `<template>` (marcado HTML dinámico) y `<style scoped>` (estilos CSS encapsulados) [cite: 110, 119].
- **Composition API vs. Options API**:
    - _Options API_: Organiza el componente en un objeto con opciones fijas (`data`, `methods`, `computed`, `watch`) [cite: 120, 121].
    - _Composition API_ (estándar recomendado en Vue 3 con `<script setup>`): Agrupa la lógica por funcionalidad utilizando funciones reactivas (`ref`, `computed`, `onMounted`) [cite: 121, 124]. Facilita la reutilización de código mediante _composables_ [cite: 121, 125].
    - **`ref` vs. `reactive`**: Se aconseja usar `ref()` por defecto para cualquier tipo de dato (requiere `.value` en el script) [cite: 122, 123]. `reactive()` solo admite objetos y pierde la reactividad al desestructurarse o reasignarse [cite: 122, 123].
- **Directivas fundamentales**:
    - `:atributo` (`v-bind`): Enlaza atributos HTML con variables dinámicas [cite: 98].
    - `@evento` (`v-on`): Escucha eventos del DOM (ej. `@click`, `@submit.prevent`) [cite: 98, 100, 131].
    - `v-model`: Enlace bidireccional (_two-way binding_) para formularios y controles de entrada [cite: 88, 102].
    - `v-if` / `v-else` vs. `v-show`: `v-if` crea o destruye físicamente el nodo del DOM [cite: 99]; `v-show` conmuta la propiedad CSS `display: none` [cite: 99].
    - `v-for`: Itera sobre colecciones y exige el uso del atributo identificador `:key` para optimizar el renderizado [cite: 99, 100].
- **Comunicación entre componentes**: Sigue la regla estricta de **"Props hacia abajo, eventos hacia arriba"** [cite: 113, 114]. El componente padre transfiere datos de solo lectura mediante `props` (`defineProps`) [cite: 113, 124], y el componente hijo notifica cambios o eventos a su padre emitiendo señales con `$emit` (`defineEmits`) [cite: 113, 114, 124].
- **Vue Router y Pinia**:
    - **Vue Router**: Mapea URLs a componentes sin recargar el navegador [cite: 138, 140]. Permite proteger pantallas con _guards_ de navegación según el rol del usuario [cite: 17].
    - **Pinia**: Solución oficial para gestionar el **estado global**, centralizando datos transversales (como el usuario autenticado o tokens) en _stores_ reactivos para evitar el paso excesivo de propiedades entre componentes intermedios (_prop drilling_) [cite: 141, 142].

---

### **5. Aspectos de Seguridad**

- **Principios de diseño seguro**:
    - **Defensa en profundidad y Principio de menor privilegio**: Cada componente debe tener únicamente los accesos mínimos indispensables para operar [cite: 198].
    - **Validación en el servidor como regla no negociable**: Las validaciones en la interfaz gráfica (HTML5/Vue) mejoran la experiencia de uso, pero **nunca reemplazan las validaciones del servidor** [cite: 11, 12, 135]. Cualquier atacante puede enviar solicitudes directas a la API omitiendo el navegador [cite: 135, 194].
- **Inyección SQL (SQLi - OWASP A05)**:
    - Ocurre cuando se construyen consultas SQL concatenando o interpolando directamente cadenas enviadas por el usuario (ej. `email == "1' or '1'='1"`) [cite: 26, 198, 199, 200].
    - **Prevención**: Utilizar siempre **consultas parametrizadas** (mediante marcadores posicionales `%s` donde el motor separa formalmente el código de los datos) o interactuar a través de un **ORM como SQLAlchemy** [cite: 11, 26, 195, 200, 201].
- **Cross-Site Scripting (XSS - OWASP A05)**:
    - Sucede cuando la aplicación recibe scripts maliciosos de un usuario y los renderiza en el navegador de otra persona sin filtrarlos ni codificarlos [cite: 26, 202]. Puede ser _Stored_ (guardado en la base de datos) o _Reflected_ (devuelto en mensajes de error o parámetros de URL) [cite: 26, 203].
    - **Prevención**: Validar entradas [cite: 203], evitar manipular texto con `innerHTML` usando `textContent` en su lugar [cite: 218], y confiar en el **escape automático de motores de plantillas como Jinja2**, evitando el uso del filtro `|safe` en datos aportados por usuarios [cite: 26, 188, 203].
- **Gestión segura de Cookies y Sesiones**:
    - **Client-Side vs. Server-Side**: Las sesiones por defecto de Flask son _Client-Side_ (se guardan en una cookie firmada pero no cifrada, por lo que su contenido es visible en el navegador) [cite: 26, 175]. Para proteger datos sensibles se debe usar _Server-Side Sessions_ (con librerías como `Flask-Session`), donde la información real reside en el servidor y el cliente solo transporta el `session_id` [cite: 5, 26, 176, 177].
    - **Atributos esenciales de cookies**:
        - **`Secure`**: Exige que la cookie viaje únicamente bajo conexiones cifradas HTTPS [cite: 26, 174].
        - **`HttpOnly`**: Bloquea el acceso a la cookie desde JavaScript (`document.cookie`), impidiendo el robo de identificadores ante ataques XSS [cite: 26, 150, 174].
        - **`SameSite`** (`Lax` o `Strict`): Restringe el envío de la cookie en peticiones entre distintos dominios, mitigando ataques de **Cross-Site Request Forgery (CSRF)** [cite: 26, 174].
- **Protección de credenciales**: Las contraseñas nunca deben guardarse en texto plano ni viajar en parámetros de URL; deben almacenarse mediante **algoritmos de hash seguro con sal** (como _bcrypt_ o _Argon2_) [cite: 1, 11, 149].

---

💡 ¿Te gustaría que armemos un diagrama paso a paso de cómo interactúan todas estas piezas juntas: desde que el usuario envía sus credenciales en Vue, pasando por la validación en Flask con hash/JWT, hasta la persistencia en PostgreSQL?
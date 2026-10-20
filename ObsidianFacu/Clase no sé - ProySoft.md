Clase del 20/10

Los microservicios son divisiones de los servicios. en conjunto hacen lo mismo que la aplicación completa (monolíticas)
Se usa una unica UI pero internamente funcionan muchos microservicios e incluso muchas bases de datos
Los servicios se comunican mediante una API Gateway (Es un punto de entrada con una capa de seguridad). Es unpunto de acceso a distintos servicios. 
Podemos tener distintos Gateway por ejemplo uno para clientes móviles de clientes de navegador

Otro patrón es el de mensajería. Un message Broker, capa de mensajería. Recibe tareas, las encola y dsp alguien las lee. 
Los servicios está aislados temporalmente, solo se comunicarían por este message broker

---
Vamos a usar Frameworks (vue)
Los datos son reactivos, por lo que cuando se modifique un dato, se va a modificar toda la vista. 
Con los frameworks vamos a relacionar estado y DOM.

Tenemos para elegir Angular, React y Vue. 
Tenemos q elegir el mas facil y rápido de aprender

Angular: 
completo, http y typescript (JavaScript)
Ruteo y estados incluidos, actualización, deteccion de cambios y signals

React:
Es librería, lo mantiene meta, tiene vistas jsx, usa virtual dom. Es el más usado

Vue:
Fw progresivo, Usa html pero podemos usar javascript o typescript. Usa reactividad y virtual DOM. La curva de aprendizaje es mas tranquila (creo)


Vite viene en crecimiento. 
El profe tiene una página donde se muestra año a año el progreso y uso de los frameworks y librerías

---
Vue3: tiene directivas (?) 
Vamos a ver su codigo, y las reacciones.
Podemos tener un template y Varios componentes
Hay 2 estructuras de API, nos recomiendan usar la más nueva. Los hooks tienen un ciclo de vida

Fw reactivo. Cuando cambian los datos, Vue actualiza las vistas.
Cuando hablamos de datos, modelos, estados, hablamos de variables JS. Si algo cambia, se modifica la vista, y si interactuamos con la vista interactuando, se modifican los datos y el resto de la vista (?)
Todo se modifica en vivo, por eso se dice reactividad. Usamos el patrón MVVM (Model-View-ViewModel). Vue se encarga de que funcione
Vue usa Virtual DOM, ya que modificar el DOM real es costoso, entonces usa una representación liviana del DOM. El template se compila y devuelve un virtualDom, cuando algo cambia, se vuelve a ejecutar la función de render, y cuando hay una diferencia entre VirtualDom y Dom real, ahí se aplican las modificaciones en el Dom real (se parchea). El usuario ve el Dom real. 
Vue.js está enfocado en la vista.  

Podemos instalar linkeando CDN con vue 3
Vite es multiframework 

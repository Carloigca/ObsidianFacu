#clase 
Para usar Power BI debemos recuperar una cuenta o usar un módulo 
Al hacer el sistema de gestión, tenemos la BD con la estructuras tipicas de BD. Al hacer un tablero, buscamos visualisaciones de metricas agruádas. y al cambiar de fltros, las métricas se cambian. Al usar formas normales y evitar redundancias, podemos diseñar una BD distinta, mas ancha, redundante, que sigue un modelo estrella y es más agil para las consultas. Pero es más lento para las escrituras. Tenemos q hacer el proceso de TTL (?) o DTL(?) no entendí bien esa letra. 

Vamos a defender mostrando en la compu. 
Con supabase vamos a compartir la API con otros compañeros. 

---
SPec driven development: Usar documentación para q trabaje la ia. 
Vibe coding: tirar un prompt solo


Al programar, dsp de muchos prompt, se acaba la ventana de contexto de la ia. Se olvida de 

## Fundamentos 
LLMla ia no piensa, predice. Es no deterministico, ya q el mismo prompt puede dar resultados distintos. No tiene memoria propia ya q por lo general se olvida las conversaciones previos. 
Alucina con confianza porque responde con seguridad cuando se puede estar confundiendo

La ventana de contexto es la memoria del modelo. Se llena rapido, y con contextos largos, pierde la precisión. Contextos a demanda: hay contextos fijos que se pueden preestablecer (CREO)

### Spec Driven Development
Se usa la especificación como fuente de verdad. 
El paradigma es: 
1. Specify o Spec. Requerimientos, tareas, especificaciones técnicas, tecnologías, documentos, etc, Es el spec.md
2. Plan. Deriva de la especificación, son las partes a hacer y sus conexiones
3. Tasks: derivan del plan, es como hacer un Login. Se separan las subtareas y los tets
4. Implement. Implementación de las tareas ya descritas.

Es distinto al vibe coding. Puso diferencias en una tabla
Un Diagrama Entidad Relación (DER) sirve para q el agente lea detalles o entienda mejor todo. 
Una spec completa tiene un objetivo (el por qué), criterios y algo más
Mientras más completo sea el spec, mejor. Le da mejor base a todo el desarrollo posterior

Una vez q tenemos el spc, pasamos al plan y las tareas, especificando detalles como que no queremos q se dupliquen datos, podemos especificar el esquema, los endpoints, etc. 

El Agent.md es de estandar abierto, cada herramienta...

Qué cosas no van en Agents MD?  Consejos genéicos, documentación entera, secretos, tokens o UrLS Regas contradictorias o desaxctualizadas

TOOLS y MCP
En lugar de conectarse a API, se implementan MCP para ejecutar otras acciones. Supabase tiene MCP copado. Playwright sirve para testing, está en navegador, y sirve para el frontend. Sighub tiene para hacer PR, Pull, y branches. 

---
SEGURIDAD, permisos y hooks
Podemos crear un archivo con permisos para claude Code. Indicando si puede pushear, ejecutar comandos, etc. 
Y hooks q son como subtareas, como q dsp de actualizar un archivo, ejecute otra cosa. Son como tareas q se corren en cierto momento)

---
Skills. 
No se carga en el AGENTS.md. El agente decide cuándo las usa, y se pueden crear scripts de ayuda para q ejecuten. Se puede crear una Skill para crear tests, así el agente cuando deba hacerlo, carga lo q está debajo, los detalles. 

---
Podemos crear SUBAGENTEs para distintas funcionalidades, para el front, para el back, para las conexiones, un orquestador (para saber qué aggentes usar), uno revisor (para q valide el código, al que se le carguen las tools para q valide bien el código. Se le puede indicar qué usar. Se recomienda OPUS (claude) y dsp pasarlo a Sonnet para el código).

---
Repaso:
- AGENTS.md para conocimiento base
- Skill para rutinas ocasionales
- Subagents para aislamiento de tareas
- Hooks/settings para reglas inquebrantables. 

Specs -> Tareas -> Código -> Revisión

La revisión humana debe estar antes de cada commit o push. 
A veces la ia hace tests, pero no valida las reglas de negocio, y esta es la parte más importante, ya que los endpoints pueden estar bien, pero los tests no. 

---
Checklist recomendaciones;
- Exportar la documentación de TTPS a texto (Markdown y Mermaid)
- .

Vamos a tener q poder explicar lo que haga la ia. 

---
# SUPABASE
herramienta recomendada para los módulos. Si bien está en la nube, alojado, se puede correr y usar local y gratis
Hay q tener cuidados:
- No cambiar columnas
- Si no se usa, se pausa el proyecto
- Si no se ponen permisos, un grupo puede leer y escribir datos ajenos

Es un posgre con papota porque puede hacer lo mismo que posgre pero mejor. 
Tiene métodos de autenticación, APIs, funciones, triggers, etc. Tablas, vistas, etc. Y hasta podemos hacer migraciones de forma casi directa. 
 Tiene pros y contras (como seguridad y permisos)
 - .
 - .

Vemos a necesitar módulos para cada grupo, mediante algun parámetro, o mediante Grant o RLS (creo). Se puede ver qué roles pueden acceder a cada dato y a cada fila 

Para integrar los grupos, vamos a definir contratos (con la documentación), URLs, los datos a manejar, los cambios.
Vamos a tener un "lado" proveedor y otro consumidor (La palabra no era lado, era otra)

Servidores MCP se pueden descargar desde tiendas de Claude (?) por si lo queremos integrar en el backend, aunque no lo vale. 


Luego de analizar en profundidad las 61 páginas del PDF de las Etapas 1 y 2 y contrastarlas contra el estado actual de tu rama `feature/modulo-entregas` (último commit `60d4b4a`), este es el diagnóstico completo, separando claramente **tu Módulo de Entrega de Pedidos** de las **especificaciones generales / transversales del proyecto**.

---

## Veredicto general

- **Tu Módulo de Entrega de Pedidos (Modo Kiosco + API de entregas):** **Cumple en gran medida (~95%)** con todos los requerimientos funcionales, visuales, táctiles, de accesibilidad y reglas de negocio pedidos en las páginas 4–5, 31–33, 46–50 y 58–59 del PDF. Solo quedan unos pocos casos borde menores para pulir.
- **Especificaciones generales del Trabajo Integrador (sistema completo):** **Cumplimiento parcial** en esta rama. La base de Etapa 1 en `admin/` está bien encaminada, pero hay **varios puntos críticos de equipo/integración** que deben resolverse antes de la entrega del 20/11/2026 (especialmente la versión de Vue, el `.env.production`, los guards/login por rol y la integración de las ramas del resto de tus compañeros).

---

## Parte 1: Auditoría del "Módulo de Entrega de pedidos"

### 1.1 Lo que SÍ cumple tu implementación

| Requerimiento del PDF | Estado | Evidencia en el código |
| :--- | :---: | :--- |
| **Modo Kiosco — Pantalla completa sin menú lateral ni navbar** (págs. 40, 46) | ✅ | `meta.kioskMode: true` en [router/index.js](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/router/index.js#L28-L36), `<header v-if="!route.meta.kioskMode">` en [App.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/App.vue#L66) y layout fullscreen en [KioskDeliveryView.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/views/entregas/KioskDeliveryView.vue#L455-L463). |
| **Flujo lineal + accesos discretos en esquina** (pág. 46) | ✅ | Buscar → revisar comidas → confirmar. Accesos en la esquina superior derecha para entregas del turno, totales del día y cierre de sesión en [KioskHeader.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/components/entregas/KioskHeader.vue#L74-L104). |
| **Controles táctiles ≥ 48×48px, sin hover ni doble clic, textos legibles** (pág. 47) | ✅ | Teclas de `64×64px`, tarjetas de comida `min-height: 92px`, botones de acción `48–60px`, `touch-action: manipulation` y `user-select: none` en [KioskDeliveryView.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/views/entregas/KioskDeliveryView.vue#L465-L469) y [PersonMealCard.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/components/entregas/PersonMealCard.vue#L190-L253). |
| **Teclado numérico en pantalla** (pág. 47) | ✅ | Dígitos `0–9`, borrar, limpiar, buscar; `readonly` + `inputmode="none"` para evitar teclado del SO; validación en vivo (solo números, 7 u 8 dígitos) en [NumericKeypad.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/components/entregas/NumericKeypad.vue#L27-L86). |
| **Comida en curso destacada + cambio manual** (pág. 47) | ✅ | Determinada automáticamente desde `GET /api/configuration` (`horarios_comidas`) con fallback en [kioskHelpers.js](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/utils/kioskHelpers.js#L64-L88) y selector manual en [KioskHeader.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/components/entregas/KioskHeader.vue#L108-L120). |
| **Triple codificación visual (color + ícono + texto) + menú solo lectura** (págs. 47, 49) | ✅ | `Autorizada pendiente` (`✓` verde), `Ya entregada` (`✓✓` azul) y `No autorizada` (`✕` rojo), mostrando el tipo de menú sin permitir editarlo en [PersonMealCard.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/components/entregas/PersonMealCard.vue#L53-L93). |
| **Vuelta al inicio automática e inactividad (60 s)** (pág. 47) | ✅ | Limpia la operación tras confirmar (banner visible 3,5 s) y resetea todo tras 60 s sin interacción mediante [useInactivityTimer.js](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/composables/useInactivityTimer.js) y `resetearKiosco()` en [KioskDeliveryView.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/views/entregas/KioskDeliveryView.vue#L55-L74). |
| **Uso continuo / detección de pérdida de conexión y mantenimiento** (págs. 47, 52) | ✅ | Polling cada 30 s a `GET /api/health` + eventos `online`/`offline` + pantalla bloqueante ante `503` (sin perder sesión) en [KioskDeliveryView.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/views/entregas/KioskDeliveryView.vue#L287-L349). |
| **3 modos de búsqueda (DNI, Nombre/Apellido, Grupo por Sector)** (pág. 48) | ✅ | Implementados en [KioskSearchPanel.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/components/entregas/KioskSearchPanel.vue#L71-L216), permitiendo agregar varias personas a una misma operación. |
| **Operación de retiro, responsable (1 vs varias personas o tercero) y resumen previo** (págs. 49, 58–59) | ✅ | Si hay 1 persona la asigna por defecto; si hay más de 1 exige elegir a una incluida o cargar un tercero con nombre + DNI validado en cliente y servidor; muestra resumen completo y deshabilita el botón mientras envía en [DeliverySummaryModal.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/components/entregas/DeliverySummaryModal.vue) y [entregas.py](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/admin/src/web/controllers/entregas.py#L149-L167). |
| **Reglas backend de entrega (nominal, fecha actual, sin doble entrega con 409 detallado, datos del servidor)** (págs. 32–33, 49, 58–59) | ✅ | Fecha/hora y operador desde servidor, menú tomado de la solicitud, validación de fecha actual (`hoy`), y error `409` con `persona` y `comida` que el front muestra y usa para recargar la lista en [entregas.py](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/admin/src/web/controllers/entregas.py#L118-L183) y [KioskDeliveryView.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/views/entregas/KioskDeliveryView.vue#L136-L143). |
| **Consultas del turno y totales del día** (pág. 49) | ✅ | [TurnDeliveriesModal.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/components/entregas/TurnDeliveriesModal.vue) (`GET /api/deliveries` paginado + `GET /api/deliveries/{id}`) y [DailySummaryModal.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/components/entregas/DailySummaryModal.vue) (`GET /api/deliveries/summary` por comida y menú). |
| **Mínima exposición de datos personales** (pág. 49) | ✅ | Solo muestra nombre, apellido, DNI y sector. |
| **Responsive para tablets vertical y horizontal + móvil y desktop** (pág. 52) | ✅ | Media queries explícitas para `<767px`, `768px–1024px` (portrait y landscape) y `>=1025px` en [KioskDeliveryView.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/views/entregas/KioskDeliveryView.vue#L503-L533). |

---

### 1.2 Puntos a corregir o pulir DENTRO de tu módulo

1. **`POST /api/deliveries` rechaza el campo menú en vez de ignorarlo (incumplimiento puntual de pág. 58):**
   - La consigna (Ampliaciones de la API, pág. 58) dice textualmente: *"El servidor lo toma de la solicitud autorizada de cada persona; **si el cliente lo envía, se ignora**."*
   - En [schemas/entregas.py](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/admin/src/web/schemas/entregas.py#L5-L14), `RegistrarEntrega` tiene `unknown = RAISE`. Si en la defensa prueban el endpoint directamente (ej. Postman) enviando `menu_id` o `tipo_menu`, devolverá `400` en lugar de ignorarlo. Conviene usar `unknown = EXCLUDE` en ese esquema (o aceptar/descartar campos de menú).
2. **Permiso `entregas_new` otorgado al rol `Administrador` (pág. 50):**
   - El PDF indica: *"Solamente los usuarios con rol Sector Comedor podrán registrar entregas."*
   - En [seeds.py](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/admin/src/core/seeds.py#L81-L93), el rol `Administrador` tiene asignado `entregas_new` (y otros permisos operativos de la API). Debería revisarse junto al equipo.
3. **Paginación incompleta en "Grupo por Sector" ([KioskSearchPanel.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/components/entregas/KioskSearchPanel.vue#L137-L167)):**
   - `listarSectores({ pag: 1 })` solo trae la página 1 (máximo 25 sectores por defecto), cuando el hospital tiene hasta 35 sectores listados en la pág. 18 del PDF. Como `GET /api/sectors` ya devuelve `"paginas"`, podrías traer todas las páginas si `paginas > 1`.
   - Lo mismo ocurre al listar pendientes del sector con `GET /api/authorized-meals`: si un sector tuviera más de 25 personas pendientes en un mismo turno, hoy solo mostraría la página 1.
4. **Error silencioso al agregar una persona al retiro ([KioskDeliveryView.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/views/entregas/KioskDeliveryView.vue#L201-L203)):**
   - En `agregarPersonaAlRetiro`, si falla `GET /api/authorized-meals/{person_id}` (por ejemplo, corte de red o `503`), solo hace `console.error(...)` y no muestra ningún mensaje visible en pantalla al operador.
5. **Truncado de `nombre_retirador` para terceros ([pedidos.py](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/admin/src/core/services/pedidos.py#L885-L886)):**
   - Se guarda `f"{nombre_retirador} ({dni_retirador})"[:40]` por el límite de 40 caracteres de la columna. Si un tercero tiene un nombre y apellido de más de 29 caracteres, se cortará parte del DNI al final.
6. **Botón `Siguiente →` en [TurnDeliveriesModal.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/components/entregas/TurnDeliveriesModal.vue#L227-L234):**
   - Solo se deshabilita cuando `entregas.length === 0`. Si la página 1 tiene 5 entregas (menor al límite de paginación), igual habilita "Siguiente →" y lleva a una página 2 vacía. Se puede mejorar comparando contra la paginación obtenida de `GET /api/configuration` (o agregando metadata de paginación).
7. **Detalles cosméticos menores:**
   - En [KioskHeader.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/components/entregas/KioskHeader.vue#L94), el botón dice `"Entregas del día"` en lugar de `"Totales del día"` (nombre que usa el PDF y el título del modal).
   - En [KioskDeliveryView.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/views/entregas/KioskDeliveryView.vue#L404) quedó un comentario viejo (`<!-- Vista previa temporal... -->`).
   - En [NumericKeypad.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/components/entregas/NumericKeypad.vue#L76) y [DeliverySummaryModal.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/components/entregas/DeliverySummaryModal.vue#L261) quedaron placeholders de ejemplo (`"Ej: 32123456"`, `"Ej: Carlos López"`).

---

## Parte 2: Auditoría de las Especificaciones Generales del Trabajo Integrador

### 2.1 Requisitos técnicos (págs. 35–36 y 60–61)

| Requisito | Estado | Situación actual |
| :--- | :---: | :--- |
| **Python 3.14 + Poetry (`pyproject.toml`) + Flask + Jinja + PostgreSQL + SQLAlchemy ORM + MVC** | ✅ | Correctamente estructurado en [admin/pyproject.toml](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/admin/pyproject.toml#L9-L22) y capas `models / services / controllers / schemas / templates`. |
| **Validaciones servidor en módulo separado** | ✅ | Centralizadas en `admin/src/core/validacion/` y complementadas con esquemas Marshmallow. |
| **Contraseñas con hash seguro + protección SQL injection** | ✅ | Usa `werkzeug.security.generate_password_hash` en [core/services/auth.py](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/admin/src/core/services/auth.py#L10-L17) y consultas ORM vía SQLAlchemy. |
| **Login manual (sin Flask-Login) + `Flask-Session` en servidor** | ✅ | Cumplido en [admin/src/web/controllers/auth.py](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/admin/src/web/controllers/auth.py) y [config.py](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/admin/src/web/config.py#L7-L10). *(Nota: en producción asegurar `SESSION_COOKIE_SECURE = True` bajo HTTPS).* |
| **Manejo de logs en backend** | ✅ | Configurado `dictConfig` a consola y archivo rotativo `app.log` en [admin/src/web/\_\_init\_\_.py](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/admin/src/web/__init__.py#L34-L63). |
| **Docstrings y estilo Python** | ⚠️ | Bien aplicado en la mayoría de controladores y en `entregas.py`, pero hay archivos como [core/services/ajustes.py](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/admin/src/core/services/ajustes.py#L12-L109) que escriben los comentarios `"""..."""` **arriba** del `def` en vez de adentro (Python no los reconoce como `__doc__`). |
| **Vue.js versión `3.5.X` y versiones definidas en `package.json`** (págs. 36, 60–61) | ❌ | **CRÍTICO:** En [portal/package.json](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/package.json#L22) figura **`"vue": "rc"`** y un bloque `"overrides"` ([líneas 44–58](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/package.json#L44-L58)) que fuerza la Release Candidate (Vue 3.6-rc) en lugar de una versión fijada `3.5.X` como exige expresamente la pág. 61. |
| **Versionado Semántico con Git (`v1.x.x`, `v2.x.x`)** (págs. 35, 60) | ⚠️ | Los únicos tags actuales en Git son `v0.1.0` y `v0.2.0`. Para las entregas de Etapa 1 y Etapa 2 conviene acordar y etiquetar correctamente con el equipo (`v1.x.x` / `v2.x.x`). |
| **Semántica HTML5 e idioma español** (págs. 35, 53, 60) | ⚠️ | Tu módulo usa muy buena semántica (`<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<footer>`, `<label>`, ARIA) y formato `dd/mm/aaaa`, pero [portal/index.html](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/index.html#L2-L7) sigue teniendo `<html lang="">` y `<title>Vite App</title>` (debería ser `lang="es"` y el título del sistema). |
| **Responsive en 3 resoluciones (`<767`, `768–1024`, `>1025`)** (págs. 36, 52, 61) | ⚠️ | Cumplido en tu módulo. Ojo en [portal/src/assets/main.css](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/assets/main.css#L3-L28): `#app { width: 100vw }` y `body { display: flex; place-items: center }` en `>=1024px` son restos del template inicial de Vite y pueden generar scroll horizontal o desalinear otras vistas. |

---

### 2.2 Arquitectura Frontend, Login/Sesión, Mantenimiento y Producción (págs. 37–41, 52–59)

Estos puntos son **responsabilidad transversal / de otros compañeros**, pero te conviene alertar al equipo porque impactan directamente en la defensa y en tu módulo:

1. **❌ CRÍTICO — URL de producción rota en [portal/.env.production](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/.env.production#L1):**
   - Actualmente tiene `VITE_API_URL=https://admin-grupo49.proyecto2026.linti.unlp.edu.ar/` (**le falta `/api` al final**).
   - Como [auth.js](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/services/auth.js), [configuration.js](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/services/configuration.js) y [deliveries.js](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/services/deliveries.js) invocan rutas relativas como `/auth/login`, `/configuration` o `/deliveries`, en producción todas las llamadas pegarían fuera de `/api/...` y fallarían.
2. **❌ Guards de roles y página de Acceso Denegado pendientes ([router/index.js](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/router/index.js#L44-L54)):**
   - Aunque `/kiosco` declara `meta.roles: ['Sector Comedor']`, el `router.beforeEach` todavía no controla `r.meta.roles` ni redirige a una vista de "Acceso denegado" (pág. 39). Hoy un usuario de otro rol puede entrar por URL a `/kiosco` (recién la API lo frena con `403`).
3. **❌ Login no bloquea al `Administrador`, no redirige por rol y no muestra errores ([LoginView.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/views/LoginView.vue#L11-L47) y [controllers/auth.py](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/admin/src/web/controllers/auth.py#L70-L169)):**
   - Las págs. 38–39 y 53 exigen impedir el acceso a la aplicación web al rol `Administrador` (con `403` en Google y mensaje claro en pantalla), redirigir a cada rol a su pantalla inicial (`Sector Comedor` → `/kiosco`) y mostrar feedback si falla el login o si el email de Google no está registrado. Ninguno de esos puntos está resuelto aún en `development`/`feature/modulo-entregas`.
4. **❌ Bug al cerrar sesión en [portal/src/stores/user.js](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/stores/user.js#L16) y [App.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/App.vue#L49-L52):**
   - `removeAuth()` hace `localStorage.setItem("token", null)`, lo cual guarda el texto `"null"` (truthy en JS) y hace que [api.js](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/services/api.js#L20-L24) envíe `Authorization: Bearer null`. Debe ser `localStorage.removeItem("token")`.
   - Además, `App.vue` no llama a `POST /api/auth/logout` al cerrar sesión (vos sí lo resolviste bien dentro de `KioskDeliveryView.vue`).
5. **⚠️ Expiración del token JWT para tablets del Sector Comedor ([utils/auth.py](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/admin/src/web/utils/auth.py#L60-L65)):**
   - El token vence en **4 horas** (`horas_validez=4`). Las págs. 39 y 47 remarcan que en la tablet del comedor la sesión puede permanecer abierta **todo un turno** (típicamente 6–8 hs). Conviene acordar con el grupo un tiempo mayor (al menos para `Sector Comedor`) y tener preparada en la defensa la **justificación del almacenamiento del token** en `localStorage` frente a memoria, `sessionStorage` y cookies (pág. 39).
6. **⚠️ Manejo global de `503` y CORS durante mantenimiento ([api.js](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/services/api.js#L33-L53) y [\_\_init\_\_.py](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/admin/src/web/__init__.py#L137-L158)):**
   - `api.js` solo intercepta `401` globalmente; no intercepta `503` para activar la pantalla de mantenimiento en toda la app (en tu Kiosco lo manejás aparte con `GET /api/health`).
   - En `controlar_mantenimiento()`, si no se deja pasar `request.method == "OPTIONS"`, los preflight CORS en producción pueden fallar al activarse mantenimiento.
7. **⚠️ Horario de inicio y fin de comidas ([models/ajustes/ajustes.py](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/admin/src/core/models/ajustes/ajustes.py#L30-L43)):**
   - La pág. 54 pide *"horario de inicio **y fin** de cada comida"*, mientras el modelo actual solo tiene un campo `horario` (inicio).
8. **⚠️ Integración pendiente de los otros módulos del equipo:**
   - En tu rama actual todavía no están mergeadas las ramas remotas del resto del equipo (`feature/layout`, `feature/login-sesion`, `feature/carga-pedidos`, `feature/autorizaciones`, `feature/reportesyestadisticas`), por lo que en `portal/` todavía conviven vistas default de Vite (`HomeView`, `AboutView`, `HelloWorld`), y en `admin/` aún faltan varias ampliaciones de la API correspondientes a esos otros módulos (ej. exportación `formato=csv` que está con `# ...` en [autorizaciones.py](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/admin/src/web/controllers/autorizaciones.py#L238-L242), `POST /api/resident-lunches`, `GET /api/authorization-dates`, horario de corte en `pedidos.py`, etc.).

---

## Parte 3: Qué te conviene hacer ahora (priorizado)

### A. Ajustes chicos propios de tu módulo (sin tocar el código de nadie más)
1. Cambiar `unknown = RAISE` por `unknown = EXCLUDE` en `RegistrarEntrega` ([schemas/entregas.py](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/admin/src/web/schemas/entregas.py#L7)) para cumplir *"si el cliente envía el menú, se ignora"*.
2. Mostrar un mensaje de error visible en pantalla si falla `agregarPersonaAlRetiro` en [KioskDeliveryView.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/views/entregas/KioskDeliveryView.vue#L201-L203).
3. En [KioskSearchPanel.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/components/entregas/KioskSearchPanel.vue#L137), recorrer todas las páginas de `GET /api/sectors` si `data.paginas > 1` para no perder sectores si hay más de 25 activos.
4. Opcional (pulido visual): cambiar `"Entregas del día"` → `"Totales del día"` en [KioskHeader.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/components/entregas/KioskHeader.vue#L94), quitar el comentario temporal de la línea 404 de `KioskDeliveryView.vue` y ajustar el deshabilitado del botón `Siguiente →` en [TurnDeliveriesModal.vue](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/src/components/entregas/TurnDeliveriesModal.vue#L230).

### B. Puntos críticos para avisar ya mismo a tu equipo
1. **Fijar Vue a `3.5.X` en [portal/package.json](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/package.json#L22)** (sacar `"vue": "rc"` y los `"overrides"`).
2. **Corregir [portal/.env.production](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/.env.production#L1)** agregando `/api` al final (y verificar que coincida con cómo está configurado Nginx en el servidor de la cátedra).
3. **Cerrar Login + Guards + Roles:** bloquear `Administrador` en `POST /api/auth/login`, `POST /api/auth/google` y `LoginView.vue`; sacar `entregas_new` del rol `Administrador` en `seeds.py`; corregir `localStorage.removeItem("token")` en `stores/user.js`; agregar el control de `meta.roles` + vista de acceso denegado en `router/index.js`; y redirigir a `/kiosco` cuando inicia sesión `Sector Comedor`.
4. **Actualizar [portal/index.html](file:///d:/Documentos/FACULTAD/4to-año/Proyecto%20de%20Software/code/portal/index.html)** (`lang="es"` y título real), revisar la duración del JWT para tablets de comedor y crear los tags de **Versionado Semántico** antes de la entrega.

Si querés, puedo pasarte el paso a paso puntual para resolver los 3–4 ajustes chicos de tu módulo (Punto A) sin tocar nada de tus compañeros.
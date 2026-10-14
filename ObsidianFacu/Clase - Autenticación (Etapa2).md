# Autenticación 
#### Identificación, Autenticación, Autorización
- Identificar: Dar cuentas. Quuién debo ser?
- autenticar: validar q la persona sea la dueña de la cuenta. Cómo lo valido?
- Autorizar: validar q la persona con la cuenta pueda hacer ciertas cosas. Qué puedo hacer adentro del sistema?

OpenID y Connect resuelve el inicio de sesión

JWT es otra cosa, es un formato(?) ver ppt. Es un formato que tiene una cabecera, un punto, un diccionario (payload) y una firma. Son muchas cosas 

Hoy en día, Google funciona como un proveedor de autenticación. Le da un token a la aplicación que el usuario quiere usar, y esta aplicación accede.
El código temporal que da el proveedor no es un token.
Browser -> Proveedor -> browser -> app -> API 
El browser nunca accede a la api directamente. 

---
#### Oauth
Tenemos roles OAuth:
- Resource owner es la persona q puede conceder acceso
- Client es nuestr app
- ...a
- nose

Los roles de OAuth no son permisos. A parte tenemos roles de servicio (no recuerdo bien si era el nombre) 

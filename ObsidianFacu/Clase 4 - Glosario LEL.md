#clase 
Tmb está propuesto por Julio Leite. En este, cada término tiene 2 atributos que lo describen, noción e impacto.
==Noción==: Similar a las definiciones de diccionario
==Impacto==: Cómo se relacionan los distintos términos entre ellos

Es la forma de filtrar elementos fundamentales de un dominio. En el diccionario tenemos 4 categorías: Sujeto(elementos activos q hacen cosas), Objeto(elementos pasivos q se usan para hacer cosas), Verbo(Actividades q se hacen) y Estado (Situaciones en las q pueden estar las actividades, objetos y sujetos).
Debemos definir una lista de palabras para englobar todo el dominio, y después las ordenamos y asignamos a donde deban

|                          | Noción                  | Impacto<br>                 |     |
| ------------------------ | ----------------------- | --------------------------- | --- |
| **Sujeto**<br>Activo     | (Quién es?)             | (Qué hace?)                 |     |
| **Objeto**<br>Pasivo     | (Qué es?)               | (Qué se busca?)             |     |
| **Verbo**<br>Actividades | (Para qué?<br>Por qué?) | (Cómo?)                     |     |
| **Estado**               | (Qué significa?)        | (Cómo pasar a otro estado?) |     |
Usuario, Sistema, credenciales de acceso, iniciar sesión

|                          | Noción | Impacto |     |
| ------------------------ | ------ | ------- | --- |
| **Sujeto**<br>Activo     |        |         |     |
| **Objeto**<br>Pasivo     |        |         |     |
| **Verbo**<br>Actividades |        |         |     |
| **Estado**               |        |         |     |
Relacionando los elementos de la lista, armamos oraciones donde tenemos Sujeto, Objeto y Verbo. 
Obtenemos un término que es el Sujeto usuario =
Sujeto : Usuario
Noción: Persona fisica registrada en la plataforma
Impacto: (Pueden ser varios)
	El usuario inicia sesión con las credenciales de acceso
	El usuario cierra  sesión

Verbo: iniciar sesión
Noción: Obtener autorización e identifiarse con el sistema
Impacto: 
	El usuario ingresa con éxito
	El sistema valida las credenciales

Seguro aparezcan muchos verbos, pero no podemmos preestablecer cantidades límites de los elementos que van a aparecer en el glosario. Seguro sean pocos sujetos. 

---
Pasos de primera parte (Hacer Glosario):
1- Listar términos
2- Caracterizar
3- Describir noción / 

Debemos notar que según como escribamos las cosas, vamos a tener el mismo concepto como distintos elementos (por ejemplo sesión iniciada q es Estado, e iniciar sesión que es Verbo)

Pasos de segunda parte (Hacer escenarios con el glosario):
1- Para cada verbo hacer un scenario 
2- Un objetivo es la noción
3- Un episodio(o más supongo) es un impacto
4- Desglose del Glosario

A la hora de seguir desarrollando, vamos a ver que el sujeto son los roles, el objeto los datos, el verbo las funcionalidades, y el estado sería el Workflow

El workflow es complejo, es como una máquina de estados, ya que hay una gran variedad de cambios (estados) que pueden aparecer en el workflow, generando nuevos estados. 

Todo el esfuerzo q hagamos en el LEL, se capitaliza al hacer los MockUps (la tabla creo), y después más con los scenarios

---
Mismo ejercicio que con los escenarios, escribimos prompt, que escriba simbolos, los categorice y escriba los impactos. Jugar con los prompt para el lunes

Revisar cronograma porque el miercoles capaz es virtual. 

![[Pasted image 20260923185320.png]]Foto en grupo de Telegram
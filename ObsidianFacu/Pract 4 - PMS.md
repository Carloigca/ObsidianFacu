#### Ejercicio 1a
```
process examinador [id= 0..N-1]{
	texto URL,res
	int id
	while (true){
		URL= buscar
		analizador!infeccion (id, URL)
		analizador?resultado (res)
	}
}

process analizador {
	texto URL,res
	int id
	while (true)
		examinador[*]?infeccion (id, URL)
		res= analizar(URL)
		examinador[id]!resultado (res)
		
}
```

#### Ejercicio 1b
Ya está en el a
#### Ejercicio 1c 
```
process examinador [id= 0..N-1]{
	texto URL,res
	int id
	while (true){
		URL= buscar
		coordi!infeccion (id, URL)
		analizador?resultado (res)
	}
}

process coordi {
	do
		examinador[*]?infeccion (id,URL)->
			push (cola, (id,URL))
		□(not empty(cola));analizador?analizar ()->
			pop (cola, (id,URL))
			analizador!infeccion (id,URL)
	od
}

process analizador {
	texto URL,res
	int id
	while (true){
		coordi!analizar ()
		coordi?infeccion (id, URL)
		res= analizar(URL)
		examinador[id]!resultado (res)
	}
}
```
#### Ejercicio 1d
Solo saco el id y la espera y envío de confirmación
```
process examinador [id= 0..N-1]{
	texto URL,res
	int id
	while (true){
		URL= buscar
		coordi!infeccion (URL)
	}
}
process coordi {
	do
		examinador[*]?infeccion (id,URL)->
			push (cola, (URL))
		□(not empty(cola));analizador?analizar ()->
			pop (cola, (URL))
			analizador!infeccion (URL)
	od
}
process analizador {
	texto URL,res
	int id
	while (true){
		coordi!analizar ()
		coordi?infeccion (URL)
		res= analizar(URL)
	}
}
```
#### Ejercicio 2
```
process preparador{
	while (true){
		prepararMuestras (muestra)
		armador! envMues (muestra)
	}
}
process armador{
	do 
		(); preparador? envMues (muestra) -> 
			set= armarSet (muestra);
			push (cola, set)
		□(not empty (cola)); analizador? esperando() ->
			pop (cola, set)
			analizador! envSet (set)
		□(); analizador? envResu (resultado)
	od
}
process analizador{
	while (true)
		armador! esperando ();
		armador? envSet (set)
		resultado= analizarSet(set)
		armador! envResu (resultado)
}
```
---
#### Ejercicio 3a 
1 profesor. El alumno tiene q enviar su id así el profe se lo devuelve
```
procedure alumno [id:..N-1]{
	ex= resExam
	Profesor! envExam (id, ex);
	Profesor? envNota (nota)
}
procedure profesor {
	for int i=0..(N-1) {
		alumno[*]? envExam(idAlu,ex)
		resultado= corregir (ex)
		alumno[idAlu]! envResu (resultado)
	}
}
```
#### Ejercicio 3b
Hay más de un profesor, intermediario que asigne profesores? Así los profesores saben a qué alumno enviarle los resultados del examen
```
procedure alumno [id:0..N-1]{
	ex= resExam
	coordi! envExam (id,ex)
	Profesor[*]? envResu (nota)
}
procedure coordi {
	int recibidos=0;
	int entregados=0;
	do 
		(recibidos<N); alumno[*]? envExam(idAlu, ex) ->
			push (colaE, (idAlu,ex))
			recibidos++;
		
		□((entregados<N)and(not empty(colaE))); profesor[*]?libre(idProf) ->
			pop (colaE, (idAlu,ex))
			profesor[idProf]! envExam (idAlu,ex)
			entregados++;
	od
	for int i=0..P-1 {
		profesor[*]? libre (idProf)
		profesor[idProf]! envExam (-1, null)
	}
}
procedure profesor [id:0..P-1]{
	idAlu=0
	while (idAlu>-1) {
		coordi! libre (id)
		coordi? envExam(idAlu,ex)
		if (idAlu>-1) {
			resultado= corregir (ex)
			alumno[idAlu]! envResu (resultado)
		}
	}
}
```
#### Ejercicio 3c
El intermediario ahora tmb hace de barrera? Agrego un envío inicial entre alumnos y coordi
```
procedure alumno [id:0..N-1]{
	texto ex;
	int nota;
	coordi! llego()
	coordi? empezar()
	
	ex= resExam
	coordi! envExam (id,ex)
	Profesor[*]? envResu (nota)
}
procedure coordi {
	int recibidos=0;
	int entregados=0;
	int idAlu;
	texto ex;
	Queue colaE
	
	for int i=0..N-1 {
		alumno[*]? llego ()
	}
	for i=0..N-1 {
		alumno[i]! empezar()
	}
	
	do 
		(recibidos<N); alumno[*]? envExam(idAlu, ex) ->
			push (colaE, (idAlu,ex))
			recibidos++;
		
		□((entregados<N)and(not empty(colaE))); profesor[*]?libre(idProf) ->
			pop (colaE, (idAlu,ex))
			profesor[idProf]! envExam (idAlu,ex)
			entregados++;
	od
	for int i=0..P-1 {
		profesor[*]? libre (idProf)
		profesor[idProf]! envExam (-1, null)
	}
}
procedure profesor [id:0..P-1]{
	idAlu=0;
	Texto resultado, ex;
	while (idAlu>-1) {
		coordi! libre (id)
		coordi? envExam(idAlu,ex)
		if (idAlu>-1) {
			resultado= corregir (ex)
			alumno[idAlu]! envResu (resultado)
		}
	}
}
```
#### Ejercicio 4a
empleado recibe id, le avisa q es su turno y espera confirmación para dejar al siguiente
```
procedure persona [id:0..N-1]{
	empleado! llego (id)
	empleado? usarSim ()
	UsarSim;
	empleado! termino ()
}
procedure empleado {
	for int i=0..N-1 {
		persona[*]? llego (idP)
		persona[idP]! usarSim ()
		persona[idP]? termino ()
	}
}
```
#### Ejercicio 4b
El empleado va avisando por orden, cada persona solo espera q le avisen y avisa cuando termina
```
procedure persona [id:0..N-1] {
	empleado? usarSim ()
	usarSim;
	empleado! termino ()
}

procedure empleado {
	for int i=0..N-1 {
		persona[i]! usarSim ();
		persona[i]? termino ();
	}
}
```
#### Ejercicio 4c
El empleado recibe pedidos y los encola. Simulador q sea un proceso? 
Si el simulador es un proceso, este avisa cuando está libre así el empleado asigna una persona, pero no sé si puede ser un proceso. 

Capaz el empleado puede recibir pedidos de personas y si está libre (bool) lo asigna, y dsp puede recibir cuando se libere, sin poner libre si hay gente esperando (Cola)
```
procedure persona [id=0..N-1] {
	empleado! llego (id);
	empleado? usarSim ();
	usarSim;
	emplado! termino ();
}

procedure empleado {
	do 
		(llegaron<P); persona? llego (idP) ->
			llegaron++
			if (not libre) {
				push (colaP, idP)
			} else {
				libre=false;
				persona[idP]! usarSim();
			}
		□(not libre and atendidos<P); persona? termino (); ->
			if (not empty (colaP)) {
				pop (colaP, idP)
				persona[idP]! usarSim ();
				atendidos++;
			} else {
				libre= true;
			}
	od
}
```

#### Ejercicio 5
un proceso coordinador q reciba a los espectadores. Si la maquina esta libre (un boolean), le indica q la use y sino lo encola. 
Cuando otro espectador avisa q la terminó de usar, si hay cola, le avisa al siguiente q la use, sino, libera la maquina
```

```
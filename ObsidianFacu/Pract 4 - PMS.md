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
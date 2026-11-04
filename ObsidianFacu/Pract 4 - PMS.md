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
#### Ejercicio 1c TERMINAR
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

process coordi {
	while (true)
		do
			();examinador[*]?infeccion (id,URL)->
				push (cola, (id,URL))
			();analizador?analizar ()-> 
				pop (cola, (id,URL))
				analizador!infeccion (id,URL)
}

process analizador {
	texto URL,res
	int id
	while (true)
		coordi?infeccion (id, URL)
		res= analizar(URL)
		examinador[id]!resultado (res)
		
}
```
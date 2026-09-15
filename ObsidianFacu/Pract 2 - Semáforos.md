#### Ejercicio 1
a) 
b) free = 1;
```
Process Persona [i = 1 to n]::
	{P(free)
	detector()
	V(free)
	}
```

c) Sem cant = 3; Sem mutex = 1; Queue Int = cola[ 1,2,3 ]
```
Process Persona [i = 0 to n-1]::
	{int #detector = 0;
	P (cant)
	
	P (mutex)
	#detector = cola.dequeue ();
	V (mutex)
	
	detector (#detector);
	
	P (mutex)
	cola.enqueue (#detector)
	V (mutex)
	
	V (cant)
	}
```

d) Sem cant = 3; Sem mutex = 1; Queue int = cola [ 1, 2, 3 ]
```
Process Persona [i = 0 to n-1]::{
	int numDetector = 0;
	int numRandom Random ();
	for (i=0; i< numRandom){
		P (cant)
		P (mutex)
		#detector = cola.dequeue ();
		V (mutex)
		detector (#detector);
		P (mutex)
		cola.enqueue (#detector)
		V (mutex)
		V (cant)
	}
}
```

#### Ejercicio 2
a)  Int Historial [ 1..N ]; Int tamaño = N //int leo [1..N]; Sem leo = 1;
```
Process Detector [id: 1 .. 4]::{
	int inicio= tamaño * (id-1);
	int fin= (tamaño * id) -1;
	
	for (int i=inicio to fin){
		if (Historial[i].nivel = 3) print(Historial[i].ID)
	}
}
```

b)Int Historial [ 1..N ]; Int tamaño = N; int fallos [ 0..3 ]; Int acceso [ 0..3 ];
```
process Detector [id: 1..4]::{
	int inicio= tamaño * (id-1);
	int fin= (tamaño * id) -1;
	int fallosLocal ([4] 0);
	
	for (int i=inicio to fin){
		fallosLocal[Historial[i].nivel]++;
	}
	
	for (int i=0 to 3){
		P(acceso[i]);
		fallos[i]= fallos[i] + fallosLocal[i];
		V(acceso[i]);
	}
}
```

c) Int Historial [ 1..N ]; Int tamaño = N; int fallos [ 0..3 ]
```
process Detector [id: 1..4]::{
	cant=0;
	for (int i=0 to N){
		if (Historial[i].nivel = id) cant ++;
	}
	
	fallos[id] = cant;
}
```

#### Ejercicio 3
Recurso cola [5]; Sem cant = 5; Sem uso = 1;
```
Process proceso [id: 1..P]::{
	Recurso r;
	P(cant);
	
	P(uso);
	desencolar (r);
	V(uso)
	
	usarRecurso
	
	P(uso)
	encolar (r);
	V(uso)
	
	V(cant);
}
```

#### Ejercicio 4
Total Sem = 6; alta Sem = 4; baja Sem = 5;

```
Process Usuario-Alta [I:1..L]::{ 
	P (alta);
	P (total);
	//usa la BD
	V(total);
	V(alta);
}
```

```
Process Usuario-Baja [I:1..K]::{ 
	P (baja);
	P (Total);
	//usa la BD
	V(total);
	V(baja);
}
```

#### Ejercicio 5
a) Sem llenos=0; Sem vacíos = n;
```
process productor::
	while (true) {
		//produce
		P (vacíos)
		//llena contenedor
		v (llenos)
	}
```

```
process consumidor::
	while (true) {
		P (llenos)
		//vacía
		V (vacío)
	}
```


d) Int libre = 0; Int ocupado = 0; Contenedor buf[N]; Sem vacío = n;  Sem Lleno = 0; Sem mutexP = 1; Sem mutexC = 1; 
```
process Productor [i = 1..P]::{
	while (true){
		//Produzco Elemento E
		P (vacío);
		P (mutexP);
		buf[libre] = E;
		libre = (libre+1) mod n;
		V (mutexP);
		
		V (lleno);
	}
}
```

```
process Consumidor [i= 1..E]::{
	while (true){
		P(lleno);
		
		P(mutexC);
		Paquete p= vaciar(buf[ocupado]);
		ocupado = (ocupado+1) mod n;
		V(mutexC);
		V(vacío);
		//enviar paquete p
	}
}
```

#### Ejercicio 6
a) Sem Libre =1;
```
process Persona [id= 1..N]:: {
	P(libre);
	imprimir (doc);
	V(libre);
}
```

b) Bool Libre =1; Sem Mutex= 1; Int Queue cola[0]; 
```
process Persona [id= 1..N]:: {
	P (Mutex)
	if (Libre){
		Libre= false;
		Imprimir (doc);
		
		V(mutex);
	}else{
		cola.encolar(id);
		V(mutex);
		
		P(Libre);
	}
	
	Usar un booleano para saber si hay alguien viendo la cola o usando la impresora. 
	
}
```
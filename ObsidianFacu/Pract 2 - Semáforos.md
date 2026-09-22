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

b) Bool Libre =V; Sem Mutex= 1; Int Queue cola[0]; 
```
MAL
process Persona [id= 1..N]:: {
	P (Mutex)
	if (Libre){
		Libre= false;
		Imprimir (doc);
		V(mutex);
		
	}else{
		cola.encolar(id);
		V(mutex);
		Boolean imprimi=false;
		
		while (! imprimi){
			P(Mutex);
			if ((Libre) & (prox(cola) = id)){
				Libre = false;
				desencolar(id)
				Imprimir (doc);
				V(Mutex)
				imprimi = True;
			}
		}
	}
}
```
Bool free= true; Sem mutex =1; int Queue cola[0]; Sem espera[N] = ([N],0);
``` 
BIEN. Free se usa para indicar q la cola está vacía

process Persona [id= 1..N]:: {
	int aux= -1;
	
	P(Mutex) //para el Free y la Cola
	if (free){ //Si está libre, la ocupo e imprimo
		free = false;
		V(Mutex)
	} else { //Si no está libre, me encolo y espero aviso
		Push (cola, id);
		V(Mutex);
		P( espera[id] ); 
	}
	
	imprimir (doc); //imprimo (xq está libre, o xq me toca)
	P(Mutex);
	if ( cola.isEmpty() ) { //Si no hay nadie, solo libero
		free = true;
	} else { //Si hay alguien, le aviso q le toca
		Pop (cola, aux); //Saco siguiente
		V (espera[aux]); //Aviso que imprima
	}
	V(Mutex) //Libero para el siguiente 
}
```

c) Terminó = 0; Sem Mutex = 1;
```
MAL
process Persona [id= 1..N]:: {
	Boolean imprimi = False; 
	while (! imprimi){
		P (Mutex);
		if (Terminó == (id-1) ){
			Imprimir (doc);
			Terminó = id;
			V(Mutex);
			imprimi = True;
		} else {
			V (Mutex);
		}
	}
}
```
Sem espera [N] = ([N], 0); Sem mutex = 1; espera[0]=1;
```
BIEN
process Persona [id= 0..N-1]::{
	P(espera[id]);
	imprimir (doc);
	V(espera[id+1]);
}
```

d) Bool Libre=V; Prox= -1; Sem Mutex= 1; Int Queue cola[0]; 
```
MAL
process Coordinador:: {
	while (true){
		P(mutex)
		if ( !isEmpty(cola) & Libre){
			prox = desencolar(cola)
			V(mutex)
			
		}
	}
}
process Persona [id= 1..N]::{}
```
Sem espera[N] = ([N],0); Sem Mutex =1; Sem llegue=0; Sem termine=0; int Queue cola[0];
```
BIEN
process Persona [id= 0..N-1]::{
	P(Mutex);
	Push (cola, id);
	V(Mutex);
	V(llegue);
	
	P (espera[id]);
	imprimir (doc);
	V (termine)
}

process Coordinador ::{ 
	int aux=-1;
	for int i=0 ..N-1{ //espera aviso, se fija quién sigue, le avisa, espera aviso
		P(llegue);
		
		P(Mutex);
		Pop (cola, aux);
		V(Mutex);
		
		V(espera[aux]);
		
		P(Termine);
	}
}
```

e) 
Persona:
- llego, me encolo y aviso
- Espero q me avisen que puedo imprimir, reviso cuál impresora me asignaron y la uso
- Bloqueo la cola para devolverla, la desbloqueo y aviso
Coordinador:
- sé que todas las personas van a imprimir, espero que llegue una
- Bloqueo la cola y saco una persona
- Espero q haya impresora libre, la saco de la cola
- La asigno y aviso

Sem mutex=1(cola); Int Queue cola[0]; 
```
Process persona [id= 0..N-1]:: {
	P(mutex);
	push (cola, id);
	V(mutex);
	V(llegue);
	
	P(espera[id]);
	int miImpresora= impresoraAsignada[id];
	imprimir (doc, miImpresora);
	
	P(mutexImp);
	push (colaImp, miImpresora);
	V(mutexImp);
	V(cantImpresoras);
}

process Coordinador:: {
	int aux; int impAux;
	for (int i=0..N-1) {
		P(llegue);
		
		P(mutex);
		pop (cola, aux);
		V(mutex);
		
		P(cantImpresoras);
		P(mutexImp);
		pop (colaImp, auxImp);
		V(mutexImp);
		
		impresoraAsignada[aux] = auxImp;
		V(espera[aux]);
	}
}
```

#### Ejercicio 7
```
process Alumno [id= 1..N]::{
	int tarea=-1;
	while (tarea==-1){
		if (espera[id] != 0){
			tarea= espera[id];
		}
	}
	
	realizar tarea
	P(mutex1);
	Push (cola, tarea);
	espera[id]=0;
	V(mutex1)
	
	int puntaje=-1;
	while (puntaje==-1){
		if (espera[id] != 0){
			puntaje = espera[id]; 
		}
	}
	
}
```
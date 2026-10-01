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
MAL process Alumno [id= 1..N]::{
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

Sem P =0;Sem barrera=0; Sem terminé=0; Sem Mutex=1; Sem Mutexnota=1; Queue cola[0]; int alumnos=0; int alumno[1..50]; int nota [1..10]; Sem grupoTermino[10] = (10, 0); 

process alumno [id= 1..N]::{
	int tarea = elegir ();
	
	P(MutexContador)
	alumnos++;
	if (alumnos==50){
		for int i=1 to 50 V(barrera);
	}
	V(Mutexcontador);
	P(barrera);
	
	realizar tarea 
	P(Mutex)
	Push (cola, tarea) //Avisa q terminó mediante la cola
	V(Mutex)
	V(Terminé)
	
	int notaGrupal=0;
	P(grupoTermino[tarea]);
	notaGrupal= nota[tarea];

}

process profesor:: {
	int terminadas = 0;
	int tareaActual;
	int tareas[1..10] = (10, 0); 
	
	for int i=1 to 50{
		P(terminé)
		P(Mutex)
		Pop (cola, tareaActual);
		V(Mutex);
		
		tareas [tareaActual] ++;
		//Si el grupo terminó, les asigno su nota (q es el orden en el q terminaron), y les aviso que ya está asignada en el vector de notas para cada tarea
		if (tareas [tareaActual] == 5){ 
			terminadas ++;
			P(MutexNota)
			nota[tareaActual]= terminadas;
			V(MutexNota)
			for int i=1 to 5 {
				V(grupoTermino[tareaActual]);
			}
		}
	}
}
```

#### Ejercicio 8 a y b
```
process empleado [id= 1..E]:: {
	int piezas=0;
	while (piezasTotales < T) {
		P(mutex);
		pop (cola, pieza);
		V(mutex);
		produce la pieza
		piezas++;
		
		P(mutexPiezas);
		piezasTotales++;
		V(mutexPiezas);
	}
	
	piezasEmpleado[id]= piezas;
	P(mutexTerminados);
	terminados++;
	if (terminados == E){
		V(terminaron);
	}
	V(mutexTerminados);
}

process empleado [id= 1..E]:: {
	int piezas=0;
	P(Mutex)
	while (piezasHechas < T){
		piezasHechas ++;
		V(mutex);
		producir la pieza;
		piezas++;
		
		P(mutex);
	}
	V(mutex)
	
	P(mutexMax)
	if (piezas > max) {
		max = piezas;
	}
	V(mutexMax)
}
```

#### Ejercicio 9
carpinteros:
	constantemente
		hace marco
		disminuyo un espacio libre
		Bloqueo buffer para guardarlo
		guardo
		aumento el indice de libres (marcos hechos)
		libero el buffer (desbloqueo a los otros carpinteros)
		aumento la cant. de marcos hechos
vidriero
	constantemente
		hace vidrio
		disminuye un espacio vacío
		lo guarda en el buffer
		aumento el indice de libres (vidrios hechos)
		aumenta la cant. de vidrios hechos
armadores:
	constantemente
			toma marco
				descuento uno hecho
				le aviso al otro armador
				saco el marco del buffer
				muevo el indice de ocupados
				le aviso al otro armador (lo libero)
				aumento los espacios libres
			toma vidrio
				descuento uno hecho
				le aviso al otro armador
				saco el vidrio del buffer
				muevo el indice de ocupados
				le aviso al otro armador (lo libero)
				aumento los espacios libres
			arma la ventana
			entrega o deja la ventana

```
var
buffer depoM[] [1..30]; Sem mutexM=1; Sem llenoM=0; Sem vacíoM=30; Sem sacarM=1;
int libreM=0; int ocupadoM=0;

buffer depoV[] [1..50]; Sem llenoV=0; Sem vacíoV=50; Sem sacarV=1;
int libreV=0; int ocupadoV

process carpintero [id= 1..4]::{
	while (true){
		marco= hace marco
		P(vacíoM) // disminuyo espacio libre 
		
		P(mutexM) // bloqueo buffer
		depoM[libreM] = marco;
		libreM= (libreM++) mod 30;
		V(mutexM) // libero buffer
		V(llenoM) // incremento cant hechos
	}
}

process vidriero:: {
	while (true){
		vidrio= hace vidrio
		P(vacioV); // descuento espacio libre
		
		depoV[libre] = vidrio
		libreV= (libreV++) mod 50;
		V(llenoV) // aumento cant hechos
	}
}

process armadores [id= 1..2]:: {
	while (true){
		P(llenoM); //tomo marco
		P(sacarM); //aviso al otro armador
		
		marco= depoM[ocupadoM];
		ocupadoM= (ocupadoM++) mod 30;
		V(sacarM); //dejo sacar al otro
		V(vacioM); //aviso que vacié un espacio
		
		P(llenoV); //tomo vidrio
		P(sacarV); //aviso al otro armador
		vidrio= depoV[ocupadoV];
		ocupadoV= (ocupadoV++) mod 50;
		V(sacarV); //dejo sacar al otro
		V(vacíoV); //aviso q vacié un espacio
		
		Arma ventana (marco, vidrio)
	}
	
}
```

#### Ejercicio 10 a
Asumimos que se puede guardar un dato compuesto tipo registro con el id y el tipo de camión
camionesT:
	bloqueo el arreglo de camiones
	Me agrego al arreglo de camiones poniendo id y tipo de carga
	desbloqueo el arreglo
	aumento la cant. de camiones en fila
	---
	espero a que me habiliten mi semáforo de camiones de Trigo
	descargo
	bloqueo contador
	camionesTotales++
	libero contador 
	Aviso??
camionesM:
	bloqueo el arreglo de camiones
	Me agrego al arreglo de camiones poniendo mi id y tipo de carga
	desbloqueo el arreglo
	aumento la cant. de camiones en fila 
	---
	espero a que me habiliten mi semáforo de camiones de Maíz
	descargo
	bloqueo contador
	camionesTotales++
	libero contador
	Aviso??
Coordinador:
	Mientras camionesTotales < (T+M)
		disminuyo la cant. de camiones en fila
		disminuyo la cant. de espacios disponibles (de 7 inicialmente)
		saco camión del arreglo de camiones en la posición indice
		Si camion.tipo = T y la cantT <5
			disminuyo la cant de camionesT // descontamos de los 5 posibles
			aumento la cantT // contamos los camiones atendidos para evaluar si llegamos a 5
			aumento su semaforo particular en camiones de Trigo //le indico que empiece
			aumento el id
		---
		sino si (el camion.tipo = M y la cantM <5)
			disminuyo la cant de camionesM
			aumento su semaforo particular en camiones de Maíz
			disminuyo el semáforo terminé
		aumento en 1 el indice usado
		

```

procedure camionesT [id=0..T-1]:: {
	P(mutex)
	Push (cola, (id,tipo)) // me encolo
	V(mutex)
	V(hayCamion) //aviso que me encolé
	
	P(camionesT[id]) //Espero q me habiliten
	descargar ()
	V(semTotal) //sumo al contador total
	V(EsperarT) //sumo al contador de camiones de Trigo simultáneos
}

procedure camionesM [id=0..M-1]:: {
	P(mutex)
	push (cola, (id,tipo)) // me encolo
	V(mutex) 
	V(hayCamion) // aviso q me encolé
	
	P(camionesM[id]) // espero q me habiliten
	descargar()
	V(semTotal)
	V(esperarM)
}

procedure coordinador:: {
	for int i=0.. (T+M-1){
		P(hayCamion)
		P(mutex)
		pop (cola, (id,tipo))
		V(mutex)
		if (tipo = trigo){
			P(esperarT)
			P(semTotal)
			V(camionesT[id])
		} else {
			P(esperarM)
			P(semTotal)
			V(camionesM[id])
		}
		
	}
}
```

#### Ejercicio 10 b
camiones:
	me fijo si hay lugar para mi tipo
	me fijo si hay lugar en general
	descargo
	aumento en 1 el lugar de mi tipo
	aumento en 1 el lugar general
```
Sem semM=5; Sem semT=5; Sem semTotal=7
procedure camionT [id=0.. T-1]:: {
	P (semT)
	P (semTotal)
	descargar()
	V (semT)
	V (semTotal)
} 

procedure camionM [id=0..M-1]:: {
	P (semM)
	P (semTotal)
	descargar()
	V (semM)
	V (semTotal)
}
```

#### Ejercicio 11
Persona
	llego y me encolo (proteger cola)
	aviso aumentando cantPersonas
	espero confirmación (arreglo de semáforos)
Empleado
	for 10 i
		for 5 j
			disminuyo cantPersonas
			tomo a alguien de la cola 
			lo meto en un arreglo local
		for each Persona en arreglo local
			vacunarPersona()
			aumento uno en su semaforo personal (del arreglo de semáforos)

```
process Persona [id=1..50]:: {
	P(mutex);
	Push (cola, id);
	V(mutex);
	V(cantPersonas);
	P(semaforos[id]);
}

process Empleado:: {
	for int i=1..10{
		for int j=1..5{
			P(cantPersonas);
			P(mutex);
			Pop (cola, id);
			V(mutex);
			grupo[j]=id;
		}
		for (int id in grupo){
			vacunarPersona()
			V(semaforos[id])
		}
		
	}
}
```

#### Ejercicio 12 a
```
procedure Pasajero[id=1..150]:: {
	P(mutexColaR)
	Push (colaRecepcionista, id)
	V(mutexColaR)
	V(avisoRecep)
	
	P(pasajeros[id])
	
	idEnfermera= asignaciones[id]
	P(mutexColas[idEnfermera]);
	Push (colas[idEnfermera], id);
	V(mutexColas[idEnfermera]);
	V(aviso[idEnfermera]);

	P(pasajeros[id])
}

procedure Recepcionista:: {
	for int i=1..150 {
		P(avisoRecep);
		
		P(mutexColaR)
		Pop (colaRecepcionista, idPaciente)
		V(mutexColaR)
		
		idEnfermera= sacarMin (colas[1].lenght, colas[2].lenght, colas[3].lenght);
		asignaciones[idPaciente] = idEnfermera
		V(pasajeros[idPaciente])
	}
}

procedure Enfermera [id=1..3]:: {
	int atendidos=0;
	while (atendidos<150){
		P(aviso[id])
		
		if (atendidos<150){
			P(mutexColas[id])
			Pop (colas[id], idPersona)
			V(mutexColas[id])
			
			hisopar (idPersona)
			V(pasajeros[idPersona])
			P(mutexCont)
			atendidos++
			V(mutexCont)
			
			if (atendidos==150){
				// hay que terminar, entonces vuelvo a habilitar todas las 
				//enfermeras para q puedan pasar el "P(aviso[id])" y hagan el if
				V(aviso[1])
				V(aviso[2])
				V(aviso[3])
			}
		}
		
	}
}
```
notas
	if (idEnfermera=1){
		P(mutexCola1)
		Push (colas[1], id)
		V(mutexCola1)
		V(aviso[1])
		
	} else if (idEnfermera=2){
		P(mutexCola2)
		Push (colas[2], id)
		V(mutexCola2)
		V(aviso[2])
	} else {
		P(mutexColas[id])
		Push (colas[3], id)
		V(mutexCola3)
		V(aviso[3])
	}

#### Ejercicio 12 b
```
process Pasajero[id=1..150]:: {
	P(mutexGeneralCola);
	idEnfermera= min (cola[1..3].lenght);
	Push (cola[idEnfermera], id);
	V(mutexGeneralCola);
	
	V(aviso[idEnfermera]);
	P(pacientes[id]);
}

process Enfermera[id=1..3]::{
	while (atendidos <150){
		P(aviso[id])
		if (atendidos<150){
			P(semaforos[id]);
			Pop(cola[id], idPaciente);
			V(semaforos[id]);
			
			Hisopar();
			V(pacientes[idPaciente]);
			
			P(mutexAtendidos);
			atendidos++;
			V(mutexAtendidos);
			if (atendidos==150){
				V(aviso[1]);
				V(aviso[2]):
				V(aviso[3]);
			}
		}
	}
}
```
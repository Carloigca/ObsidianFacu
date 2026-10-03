### ejercicio 1
![[Pasted image 20261002121859.png]]![[Pasted image 20261002121913.png]]
a- Como no importa el orden de llegada, está bien. Pero si se debiera seguir el orden está mal, ya que en una pelea por acceder al procedimiento del monitor podría ganar cualquier auto (proceso).
b- 
```
Monitor Puente
	cond cola
	int cant=0;
	
	Procedure cruzarPuente ()
		//cruza el puente

Process Auto [id= 0..N-1]:: {
	Puente.cruzarPuente()
}
```

c) No, no respeta orden de llegada xq el que acceda al monitor será el auto q pase, indiferentemente del orden. 
No, mi resolución del B tampoco contempla orden de llegada xq con orden de llegada se hace más complejo todo. 

### Ejercicio 2
a) Necesito procesos Persona, un monitor para gestionar el acceso a la BD.
b) 
```
Monitor motorBD
	int consultas=5
	cond cv
	
	Procedure solicitarAcceso () 
		if (consultas=0) wait (cv);
		consultas--;
	
	Procedure devolverAcceso () 
		consultas++;
		signal (cv);
		
Process lector [id=1..N] {
	motorBD.solicitarAcceso()
	manda sus consultas
	motorBD.devolverAcceso()
} 
```
### Ejercicio 3 
Proceso persona, monitor para manejar acceso o monitor fotocopiadora
#### 3a
```
Monitor fotocopiadora 
	Procedure tomarFotocop (doc)
		Fotocopiar(doc)

Process Persona [id=1..N]:: {
	tomarImpresora (doc);
}
```
#### 3b
Ahora uso monitor para manejar el acceso
```
Monitor coordi
	int esperando=0;
	Cond cola;
	bool libre=True
	
	procedure pedirAcceso ()
		if (!libre) {
			//Si hay cola, me cuento como uno más y me encolo
			esperando++;
			wait (cola);
		} else {
			//sino la ocupo sin pasar por la cola
			libre= false;
		}
	
	procedure cortarAcceso ()
		if (esperando = 0){
			//si no hay cola, la libero
			libre=true;
		} else {
			//sino, descuento uno de la espera y lo saco de la cola
			esperando--
			signal (cola)
		}

Process persona [id=0..N-1]:: {
	coordi.pedirAcceso()
	Fotocopiar(doc)
	coordi.cortarAcceso()
}
```
#### 3c
```
Monitor coordi 
	queue cola;
	Cond[] conds (1..N);
	int esperando=0;
	bool libre=true;
	
	procedure pedirAcceso (id, edad) 
		if (!libre) {
			esperando++
			insertar(cola, (id,edad))
			wait (conds [id])
		} else {
			libre=false;
		}
	
	procedure cortarAcceso () {
		if (esperado>0) {
			esperando--;
			pop(cola, aux);
			Signal (conds[aux.id]); 
		} else {
			libre=true;
		}
	}
process Persona [id=1..N]:: {
	int id,edad;
	coordi.pedirAcceso (id, edad);
	Fotocopiar();
	coordi.cortarAcceso();
}
```
#### 3d
```
Monitor coordi 
	Cond[] conds(0..N-1)
	int turno=0;
	
	Procedure pedirAcceso (id)
		if (id!=turno){
			Wait (conds[id])
		}
			
	Procedure cortarAcceso (){
		turno++;
		Signal (conds[turno]);
	}

Process Persona [id=0..N-1] {
	coordi.pedirAcceso(id)
	Fotocopiar()
	coordi.cortarAcceso ()
}
```

#### 3e
```
Process persona [id=0..N-1]:: {
	coordi.llegue(id);
	Fotocopiar()
	coordi.sali()
}
Process Empleado:: {
	total=0;
	while (total<N){
			siguiente ();
		total++;
	}
}

Monitor coordi 
	Cond[] conds (0..N-1)
	Cond cvEmp
	Bool libre=True
	Queue cola;
	int esperando=0;
	
	procedure llegue (id)
		esperando++
		Push (cola, id)
		Signal (llego);
		wait (conds[id])
	
	procedure sali ()
		Signal (cvEmp)
	
	procedure siguiente ()
		if (esperando==0) {
			Wait (llego)
		}
		Pop (cola, auxID);
		esperando--;
		Signal (conds[auxID])
		Wait (cvEmp)
```
#### 3f
```
//Como el anterior, que el empleado asigne y contando las que da desde una cola. Cuando se queda sin impresoras, se queda esperando con un wait. 

Process Persona [id=0..N-1]:: {
	coordi.solicitoPermiso(id,idFotocopiadora);
	Fotocopiar(idFotocopiadora);
	coordi.sali (idFotocopiadora);
}
Process Empleado:: {
	for int i=0 ..N-1{
		coordi.siguiente()
	}
}

Monitor coordi
	Queue colaF[10] = (10, 1..10); //cola con ids de fotocopiadora
	Queue colaP; //cola para ids de personas
	int ocupadas=0;
	int esperando=0;
	Cond llegue, termino;
	Cond[] conds[0..N-1];
	
	procedure solicitoPermiso (in id, out idFot)
		push (colaP, id);
		esperando++;
		signal (llegue);
		wait (conds[id])
		
		Pop (colaF, idFot);
		ocupadas++;
	
	procedure siguiente ()
		si no hay gente esperando
			espero gente
		si no hay impresora libre
			espero que se libere Y la cuento
		asigno impresora a la persona
	
		if (esperando==0){
			wait (llegue);
		}
			if (ocupadas==10){
			wait (termino);
		}
		esperando--
		Pop (colaP, auxId);
		Signal (conds[auxId]);
		
		
	procedure sali (in idFot)
		ocupadas--;
		Push (colaF, idFot)
		Signal (termino)
```

### Ejercicio 4
# Rehacer, es más simple con variables para los turnos
```
process Auto [id=0..N-1] {
	puente.permiso(id, miPeso);
	//pasar
	puente.salida(miPeso);
}
monitor puente
	queue fila, queue
	double pesoActual=0;
	cond esperoCola;
	cond esperoPeso;
	int esperando=0;
	int pasando=0
	
	procedure permiso (int id, double peso){
		si hay gente esperando
			me pongo en la fila
			espero q me dejen pasar (subió un auto y salió de la fila)
		me pongo en fila de puente
		mientras mi peso supera el limite
			espero que se baje un auto (bajó un auto)
		me sumo a la cola de autos en puente
		sumo mi peso al total
		if (esperando>0)
			Push (fila, id)
			wait (conds[id])
		push (filaPeso, id)
		while (pesoActual+peso > 50000)
			wait (pesados[id])
			if (pesoActual+peso > 50000){
				insertarPrimero (filaPeso,id)
			}
		
		
		if (esperando>0){
			Push (colauto, id)
			wait (conds[id])
		}
		if (pesoActual+peso) > 50000 {
			Push (colautosPesados,id)
			wait (esperoPeso)
		} else {
			pesoActual+= peso;
			Push (colautos, id);
		}
	}
	
	procedure salida (double miPeso)
		pesoActual-= miPeso;
		Pop (colautos, id);
		signal (conds[id]);

```

### Ejercicio 5a con 2 monitores más simple (?)
```
cliente
	llego al mostrador
	doy lista y espero el comprobante del que me atiende
empleado
	espero los N clientes
		recibo cliente en mostrador
		lo atiendo recibiendo su comprobante y generando el comprobante(?)
		devuelvo el comprobante al cliente y termino la atención
monitor de mostrador
	proced llega cliente, espera
monitor de atención
	proced atender el cliente da su lista y espera su comprobante
	proced tomar lista y generar pedido/comprobante
	proced devolver comprobante y avisar al siguiente cliente. 
```

#### 5b
//hacerlo teniendo en cuenta lo q dijeron dolo y fio sobre la distribución de trabajos. Si sé que tardan lo mismo, lo distribuyo, sino que lo vayan tomando 
//si lo hago con while (true) no terminan de ejecutar
```
ahora el cliente pone su id en una cola, y los empleados toman al cliente q les toque (Capaz usando un vector de int para cada empleado), lo atienden, reciben pedido, devuelven comprobante.
Si no hay clientes deberían dormirse los empleados (?)
```
#### Ejercicio 1 A y B
```
Chan atender(Texto, int)
chan terminados[N](Texto)

Process Persona[id: 0..N-1] {
	pedido
	pedido = Generar pedido
	send atender(pedido, id);
	receive terminados[id] (respuesta)
}

Process Empleado {
	wihle (true)
		receive atender(pedido, id)
		respuesta= resolver(pedido)
		send terminados[id] (respuesta)
}
```

#### Ejercicio 1c
```
chan atender (Texto, int)
chan pedido (int) 
chan siguiente[2]
chan terminados[N](Texto)

Process Coordinador {
	texto atencion
	int idEmp
	int idPers
	while (true) {
		receive pedido (idEmp)
		if (empty (atender)) 
			atencion="vacio"
			idPers=0
		else 
			receive atender (atencion, idPers) 
		send siguiente[idEmp] (atencion, idPers)
	}
}

Process Empleado [id= 0..1]{
	texto atencion
	int id, idPers
	while (true){
		send pedido(id)
		receive siguiente[id] (atencion,idPers)
		if (atencion <> "vacio") {
			respuesta=resolver(atencion)
			chan terminados[idPers](Texto)
		} else {
			delay (900)
		}
	}
}

Process Persona[id: 0..N-1] {
	pedido
	pedido = Generar pedido
	send atender(pedido, id);
	receive terminados[id] (respuesta)
}

```

#### Ejercicio 2
```

process Persona [id: 0..P]{
	int id, idCaja
	texto pago, comprobante
	
	send aviso();
	send espera (id)
	receive aviso[id] (idCaja)
	send atender[idCaja] (pago, id)
	receive confirmacion[id] (comprobante)
}

process coordinador {
	int [5] colaCaja ([5], 0)
	int idPers, cajaAct
	
	while (true) {
		receive aviso () //así evitamos el busy waiting
		if 
			(not empty (espera)) ->
				receive espera (idPers)
				cajaAct= min(colaCaja)
				colaCaja[cajaAct] ++;
				send aviso [idPers] (cajaAct)
			(not empty (termino)) -> 
				recive termino (cajaAct)
				colaCaja[cajaAct] --
		fi
	}
	
}

process Caja [id: 0..4]{
	int id, idPers
	texto pago, comprobante
	while (true)
		receive atender[id] (pago, idPers)
		comprobante= procesarPago
		send confirmación[idPers] (comprobante)
		send aviso()
		send termino (id);
}
```

#### Ejercicio 3
```
VERSION FLOJA
process cliente [id: 0..C] {
	int id
	texto pedido
	texto plato
	
	send pedidos (pedido, id)
	receive entregas[id] (plato)
}

process vendedor [id: 0..2] {
	int idClient
	texto pedido
	while (true) {
		if (empty(pedidos)) {
			delay (60) //un minuto para bebidas
		} else {
			receive pedidos (pedido, idClient)
			send ordenes (pedido, idClient)
		}
	}
}

process cocineros [id: 0.. 1] {
	int idClient
	texto pedido, plato
	while (true) {
		receive ordenes (pedido, idClient)
		plato= cocinar (pedido)
		send entregas[idClient] (plato)
	}
}
```

```
VERSION COMPLETA
process cliente [id: 0..C] {
	int id
	texto pedido
	texto plato
	
	send llega (pedido, id)
	receive entregas[id] (plato)
}

process coordinador {
	while (true)
		receive termine (idVend)
		if (not empty(llega))
			receive llega (pedido, idClient)
			receive termine(idVend)
			send atender[idVend] (pedido, idClient)
		else {
			send atender ("", -1)
		}
		
}

process vendedor [id: 0..2] {
	int idClient
	texto pedido
	while (true) {
		send termine (id)
		receive atender[id] (pedido, idClient)
		if (idClient == -1) {
			delay (60) //un minuto para bebidas
		} else {
			send ordenes (pedido, idClient)
		}
		send termine (id)
	}
}

process cocineros [id: 0.. 1] {
	int idClient
	texto pedido, plato
	while (true) {
		receive ordenes (pedido, idClient)
		plato= cocinar (pedido)
		send entregas[idClient] (plato)
	}
}
```
Este es mejor porque el coordinador le envía al vendedor solo cuando está libre, no está todo el tiempo

#### Ejercicio 4A y B
```
process cliente [id= 0.. N-1]
	aviso q llego
	aviso q quiero cabina
	espero q me digan cabina
	uso la cabina indicada
	aviso al empleado
	pago al empleado 
	espero ticket
process empleado
	inicialmente tengo las 10 cabinas
	loop infinito
		recibo llamado del cliente y su id
		if no deterministico
			si es para pedir cabina
				desencolo cabina
				asigno cabina al id
			si es para pagar
				cobro al cliente 
				libero su cabina
				entrego ticket

process cliente[id=0.. N-1]{
	send pedidoCabina (id)
	send atencion ()
	receive clientes[id] (#cabina)
	usar cabina (#cabina)
	send dejoCabina (#cabina, pago, id)
	send atencion ()
	receive recibo[id] (ticket)
}
procedure empleado {
	cabinas =10
	while (true)
		receive atención ()
		if 
			 (empty(dejoCabina) and not empty(pedidoCabina) and cabinas>0) ->
				 receive pedidoCabina (id)
				 pop (idCabina)
				 cabinas --;
				 send clientes[idCliente] (idCabina)
				 
			 (empty(dejoCabina) and cabinas=0) ->
				 receive dejoCabina (idCabina, pago, idCliente)
				 ticket= Cobrar (pago)
				 send recibo[idCliente1] (ticket)
				 receive atencion()
				 receive pedidoCabina (idCliente)
				 send clientes[idCliente] (idCabina)
				 
			 (not empty(dejoCabina) or ) ->
				 receive dejoCabina (idCabina, pago, idCliente)
				 push (idCabina)
				 cabinas ++;
				 ticket= Cobrar (pago)
				 send recibo[idCliente] (ticket)
}

```

#### Ejercicio 5A
```
process impresora [id= 1..3] {
	while (true)
		receive pedido (#Admin, doc)
		aux = imprimir doc
}

process administrativo [id = 0.. N-1] {
	while (true)
		Trabajar
		send pedido (id, doc)
}
```

#### Ejercicio 5B
```
process impresora [id= 1..3] {
	while (true)
		receive pedidoGeneral ()
		if (Empty (pedidosD))
			receive pedido (#Admin, doc)
			aux = imprimir (doc)
		else
			receive pedidosD (doc)
			aux = imprimir (doc)
}

process buffer {
	while (true) 
		receive pedidoGeneral (doc)
		if (empty (libres))
		
		si hay alguna impresora libre, le mando que imprima el doc
		sino espero que alguno se libere
		
		Capaz debería hacer un if no deterministico (si) que reciba cuando una impresora se libera, cuando se recibe un doc y hay libres, y cuando se recibe un doc y no hay libres (como en el 4)
}

process administrativo [id = 0.. N-1] {
	while (true)
		Trabajar
		send pedido (id, doc)
		send pedidoGeneral ()
}

process director {
	while (true)
		Trabajar
		send pedidosD (doc)
		send pedidoGeneral ()
}
```

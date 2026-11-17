# Recordar
Cada task tiene una "declaración" 
```
Task nombre1; //Una sola linea si no tiene entrys, y el TYPE va si hay más de uno

Task TYPE persona;
personas: array [0..P-1] of persona;

Task nombre2 is
	Entry nombreEntry1;
	Entry nombreEntry2;
end nombre2
```
Después cada task tiene su body
```
Task body nombre1 is
begin
	...
end;
```
---
#### Ejercicio 1a
Supongo q el puente es un task, los tipos de vehiculos tmb. Cada vehiculo hace un entry call al puente, y el puente va a aceptar en base al peso que pueda soportar. Pero solo viendo el peso, va a elegir de forma no determinística
```
Procedure Puente1 is
Task acceso is
	Entry acceso_auto;
	Entry acceso_cam;
	Entry acceso_camion;
	Entry salgo (peso: IN float);
end acceso;
	
Task type auto; 
autos: Array [0..A-1] of auto
Task Body auto is
Begin
	acceso.acceso_Auto;
	cruzarPuente;
	acceso.salgo(1);
end;

Task type camioneta;
camionetas: Array[0..B-1] of camioneta
Task Body camioneta is
begin
	acceso.acceso_cam;
	cruzarPuente;
	acceso.salgo(2);
end;

Task type camion;
camiones: Array[0..C-1] of camioneta
Task body camion is
begin
	acceso.acceso_camion;
	cruzarPuente;
	acceso.salgo(3)
end;

Task Body acceso is 
	pesoTotal: integer;
begin
	pesoTotal:= 0;
	loop
		select 
			WHEN ((pesoTotal+1 <=5) and (salir'Count=0))-> 
				ACCEPT acceso_auto() DO 
					pesoTotal:= pesoTotal +1;
				END acceso_auto;
			
			WHEN ((pesoTotal+2 <=5) and (salir'Count=0))-> 
				ACCEPT acceso_cam() DO
					pesoTotal:= pesoTotal+2;
				END acceso_cam;
			
			WHEN ((pesoTotal+3 <=5) and (salir'Count=0)))-> 
				ACCEPT acceso_camion() DO
					pesoTotal:= pesoTotal+3;
				END acceso_camion;
			
			WHEN (salgo'count > 0)-> 
				ACCEPT salgo (pesoAct) DO
					pesoTotal:= pesoTotal-pesoAct;
				END salgo;
			
		END select
	end loop;
end acceso;

begin
	null;
end puente1;
```
Implicitamente le tengo q dar prioridad a la salida de vehiculos. 
#### Ejercicio 1b
Lo mismo q el anterior, pero ahora debo ver la forma de darle prioridad a los entry call de los camiones. Si el count de los accesos de camiones es =0 atiendo a otros, sino no 
```
Procedure Puente2 is
Task acceso is
	Entry acceso_auto;
	Entry acceso_cam;
	Entry acceso_camion;
	Entry salgo (peso: IN float);
end acceso;
	
Task type auto; 
autos: Array [0..A-1] of auto
Task Body auto is
Begin
	acceso.acceso_Auto;
	cruzarPuente;
	acceso.salgo(1);
end;

Task type camioneta;
camionetas: Array[0..B-1] of camioneta
Task Body camioneta is
begin
	acceso.acceso_cam;
	cruzarPuente;
	acceso.salgo(2);
end;

Task type camion;
camiones: Array[0..C-1] of camioneta
Task body camion is
begin
	acceso.acceso_camion;
	cruzarPuente;
	acceso.salgo(3)
end;

Task Body acceso is 
	pesoTotal: integer;
begin
	pesoTotal:= 0;
	loop
		select 
			WHEN ((pesoTotal+1 <=5) and (acceso_camion'count=0) and (salir'Count=0))-> 
				ACCEPT acceso_auto() DO 
					pesoTotal:= pesoTotal +1;
				END acceso_auto;
			
			WHEN ((pesoTotal+2 <=5) and (acceso_camion'count=0) and (salir'Count=0))-> 
				ACCEPT acceso_cam() DO
					pesoTotal:= pesoTotal+2;
				END acceso_cam;
			
			WHEN ((pesoTotal+3 <=5) and (salir'Count=0)))-> 
				ACCEPT acceso_camion() DO
					pesoTotal:= pesoTotal+3;
				END acceso_camion;
			
			WHEN (salgo'count > 0)-> 
				ACCEPT salgo (pesoAct) DO
					pesoTotal:= pesoTotal-pesoAct;
				END salgo;
			
		END select
	end loop;
end acceso;

begin
	null;
end puente2;
```

#### Ejercicio 2a
El cliente pide atención y el empleado da atención
```
procedure banco is
task type cliente;
clientes: Array[0..N-1]
task empleado is
	Entry atencion(ticket: OUT data);
end empleado;

task body cliente is
	ticket: data
begin
	empleado.atencion(ticket)
end;

task body empleado is
begin
	for int i=0..N-1 loop
		accept atencion(ticket: OUT data) do
			ticket = realizar_pago();
		end atencion
	end loop
end;

begin
	null
end banco;
```
#### Ejercicio 2b 
hacer select de entry calls para el cliente
```
procedure banco is
task type cliente;
clientes: Array[0..N-1]
task empleado is
	Entry atencion(ticket: OUT data);
end empleado;

task body cliente is
	ticket: data
begin
	select
		empleado.atencion(ticket)
	OR delay (600)
		null
	end select;
end;

task body empleado is
begin
	for int i=0..N-1 loop
		accept atencion(ticket: OUT data) do
			ticket = realizar_pago();
		end atencion
	end loop
end;

begin
	null
end banco;
```
#### Ejercicio 2c
Cambio el delay por un else 
```
procedure banco is
task type cliente;
clientes: Array[0..N-1]
task empleado is
	Entry atencion(ticket: OUT data);
end empleado;

task body cliente is
	ticket: data
begin
	SELECT
		empleado.atencion(ticket)
	ELSE
		null
	end select;
end;

task body empleado is
begin
	for int i=0..N-1 loop
		accept atencion(ticket: OUT data) do
			ticket = realizar_pago();
		end atencion
	end loop
end;

begin
	null
end banco;
```
#### Ejercicio 2d
Ahora hago una encadenación de selects. Primero uno con delay, y dsp otro con else
```
procedure banco is
task type cliente;
clientes: Array[0..N-1]
task empleado is
	Entry atencion(ticket: OUT data);
end empleado;

task body cliente is
	ticket: data
begin
	select
		empleado.atencion(ticket)
	OR delay (600)
		select
			empleado.atencion(ticket)
		ELSE
			null
	end select;
end;

task body empleado is
begin
	for int i=0..N-1 loop
		accept atencion(ticket: OUT data) do
			ticket = realizar_pago();
		end atencion
	end loop
end;

begin
	null
end banco;
```

#### Ejercicio 3
```
procedure sistema
task central is
	Entry proc1;
	Entry proc2; 
end central;

task proc1; 
task body proc1 is
	data señal
begin
	señal= generarSeñal;
	central.p1señal(señal);
	loop 
		señal= generarSeñal;
		select
			central.p1señal(señal);
		or delay (120)
			null //tecnicamente se deshecha xq dsp genera otra
		end select;
	end loop;
end proc1;

task proc2;
task body proc2 is
	boolean enviado
	data señal
begin
	enviado= true;
	loop 
		señal= generarSeñal;
		select
			central.p2señal(señal);
		or delay (60)
			central.p2señal(señal); //Se asegura q se envíe la señal
		end select;
end proc2;

task body central is
	señal: data
	termine: boolean
begin
	termine= False;
	ACCEPT proc1.señal(señal);
	loop
		select
			WHEN (termine)-> ACCEPT p1señal(señal) 
		or
			WHEN (termine)-> ACCEPT p2señal(señal)
				timer.empezar()
				termine= false;
		or 
			WHEN (not termine)-> ACCEPT p2señal(señal)
		or 
			ACCEPT terminoTimer();
				termine=true
	end loop;
end central;

begin
	null;
end sistema;
```

#### Ejercicio 4
Persona llega y mientras cant<3 solicita atencion, espera 5, y sino suma cant+1 y espera 10
Medico atiende personas, si no hay personas esperando, revisa su consultorio
Enfermera tiene pedido, si el médico no la atiende, lo deja en el consultorio
```
process clinica

task type persona;
personas = array[0..N-1] of persona;
task body persona is
	cant= integer;
begin
	cant= 0;
	while (cant<3) do begin
		select 
			medico.solicitud()
				cant=3
		or Delay (300)
			cant:= cant+1
			delay (600)
	end;
end persona;

task type enfermera;
enfermeras = array[0..E-1] of enfermera;
task body enfermera is
	nota: string
begin
	loop
		select 
			medico.pedido()
		else 
			nota= pedido;
			escritorio.dejarNota (nota)
		end select;
	end loop;
end enfermera; 

task escritorio is
	Entry dejarNota(nota: IN string)
	Entry verNotas(nota: OUT string)
end escritorio;
task body escritorio
	notas: queue;
begin
	loop
		select 
			accept(dejarNota (nota: IN String)); 
			push (cola, nota);
			nota:= notas+1;
		or
			accept verNotas(nota: OUT string) is begin
				if (notas>0)
					pop(cola, nota);
					notas:= notas-1;
				else
					nota=null;
				end if;
			end verNotas;
		end select;
	end loop;
end escritorio;

task medico is
	Entry solicitud ();
	Entry pedido ();	
end medico;
task body medico is
begin
	loop 
		select 
			accept solicitud() is begin
				tratarPaciente;
			end solicitud;
		or
			when (solicitud'count = 0); accept pedido();
		else
			escritorio.verNotas(nota);
			if (nota <> null) 
				resolver(nota);
			end if;
		end select
	end loop
end medico;

begin
	null
end clinica;
```

#### Ejercicio 5
Me dijo fio q es mejor hacer el 6,8 y 9
#### Ejercicio 6
el process informa id y grupoid a las personas
cada persona avisa (no bloqueante, con select) que llego, después avisa que empieza (bloqueante)
recoje las 15 monedas
Le avisa al admin la cant total y su equipoID
Espera ganador del admin
cada equipo espera que lleguen las personas. Después acepta los entry call para q empiecen.
El admin espera 20 cantidades y idEquipos que va registrando en un arreglo de 5 contadores
al final le avisa a los 20 ids el id del equipo ganador 
```
process Juego is

task persona is
	Entry enviarIDs (id, idEquipo: IN integer);
end persona;
personas= Array[0..N-1] of persona;
task body persona is
	id, idEquipo, total, equipoGanador: integer
begin
	accept enviarIDs (id, idEquipo: IN integer)
	equipos[idEquipo].llego();
	equipos[idEquipo].empezar();
	total=0;
	for int i=0..14 loop
		total:= total+ Moneda()
	end loop;
	admin.totalMonedas (total, idEquipo);
	accept equipoGanador (equipoGanador)
end;

task equipo is
	Entry llego ();
	Entry empezar ();
end equipo;
equipos= array[1..4] of equipo;
task body equipo is
begin
	for int i=0..3 loop
		accept llego();
	end loop;
	for int i=0..3 loop
		accept empezar();
end equipo;

task admin is
	Entry totalMonedas(total, idEquipo: IN integer);
end admin;
task body admin is
	equipos= Array[1..4] of integer
	ganador, total= integer
begin
	for int i=1..4 loop
		equipos[i]=0;
	end loop
	for int i=0..19 loop
		accept totalMonedas (total,idEquipo) is begin
			equipos[idEquipo]=equipos[idEquipo] + total;
		end totalMonedas;
	end loop;
	ganador= obtenerGanador (equipos);
	for int i=0..19 loop
		personas[i].equipoGanador(ganador);
	end loop;
end;

begin
	for int i:1..20 loop
		idEquipo= ((i-1) / 4) + 1;
		personas[i].enviarIDs(i, idEquipo)
	end loop
end juego;
```

#### Ejercicio 8
Cada servidor esta a la espera de q el especialista le envíe un test (está con un call bloqueante). Y cuando lo recibe analiza y devuelve los resultados. 
El especialista tiene q enviar 8 tests y recibir 8 resultados (Todo puede ser por accepts), entonces uso 16 veces el select. Puedo recibir un pedido de test o unos resultados. 
```
process reconocimientos is
task type servidor is
	Entry termine(termino= IN boolean)
Servidores= array[0..7] of servidor
task body servidor is
	termino: boolean
begin
	termino = false //Al final el termino siempre es false xq no sé cuántos TEST se hacen. Así q es lo mismo q poner un loop infinito. 
	while (termino = false) loop
		especialista.recibirTest(test)
		Buscar(test, codigo, valor);
		especialista.recibirResultados(codigo, valor)
		accept termine(termino);
	end loop;
end servidor;

task especialista is
	Entry recibirTest (test: OUT data)
	Entrt recibirResultados (codigo, valor: IN integer)
end especialista;
task body especialista is
	test: data;
	valor, valorMax, codigo, codigoMax = integer;
begin
	loop 
		test= tomarImagen;
		for int i=0..15 loop
			select
				accept recibirTest (test: OUT data);
			or
				accept recibirResultados (codigo, valor: IN integer);
				if (valor > valorMax) 
					valorMax= valor;
					codigoMax= codigo;
				fi
			end select;
		end loop;
		for int i=0..7 loop
			servidores[i].termine();
			//Si supiera cant. de muestras, acá marco el corte
		end loop;

begin
	null;
end reconocimientos;
```
#### Ejercicio 9
Una persona realiza un reclamo a la empresa. Espera 15 mins y si todavía no llegó el camión(no recibió un entry call), vuelve a empezar
La empresa está constantemente recibiendo llamados: 
- Personas reclamando, les suma uno en un arreglo de Personas.
- Camiones indicando que están libres. en el body del acept les indica a qué id de persona atender. Cuando toma el id con más llamadas, tiene q reiniciar el contador. 
El camion pide a la empresa a dónde ir, la empresa le indica(el id de persona) y va, avisándole a la persona (le manda msje al id )
```
Lo hago en papel
```
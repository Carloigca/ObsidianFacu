#### Reglas
Dependencia multivaluada trivial: 
X U Y = R o sino que Y C(o parcialmente) X

Dependencia funcional trivial: 
Y C(o parcialmente)  X

BCNF: 
x es superclave de R mínima  
o
X -> A es una Dependencia Funcional Trivial

4FN: 
Todas las Dependencias Multivaluadas son Triviales

---
#### Ejercicio 8
df1:#Suscripción -> email,#plan
==df2: email -> nombreUsuario==
df3: email_Adicional -> nombre_adicional
df4:#suscripción, email_adicional -> fecha_adicional
==df5:#Plan -> condiciones, nombre, precio==
df6:#Contenido -> título, sinopsis, duración

CC: (#suscripción,#Contenido, email_adicional)
El esquema cumple BCNF? No porque ya#Suscripción no es superclave de R

R1(==#plan==, condiciones, nombre, precio) 
R2(==#plan, email==, nombreUsuario, ==email_Adicional==, nombre_adicional,==#suscripción==, fecha_adicional, condiciones, nombre, precio,==#Contenido==, título, sinopsis, duración)
R1 no pierde información ya que R1 n R2 =#plan que es clave en R1
R2 no cumple con BCNF porque se da que el X de la df2 no es superclave de R2

Vuelvo a particionar a partir de R2, tomo la df3
R3(email, nombreUsuario). No perdemos info, ya que R3 n R4 me da email que es clavve en R1
R4 (==#plan, email==, ==email_Adicional==, nombre_adicional,==#suscripción==, fecha_adicional, condiciones, nombre, precio,==#Contenido==, título, sinopsis, duración)
R4 no cumple con BCNF ya que la X de la df1 no es superclave en R4

Vuelvo a particionar a partir de R4, tomo df1
R5 (#suscripción, email, plan). R5 no pierden info 
R6 (==email_Adicional==, nombre_adicional,==#suscripción==, fecha_adicional, condiciones, nombre, precio,==#Contenido==, título, sinopsis, duración)
R6 no cumple BCNF porque la X de df6 no es superclave de R6

---


Si con los atributos de la CC puedo obtener todos los demás atributos,
CC es X
X⁺ Es el resultado del algoritmo, representa a todos los atributos determinados por X en R (relación)
F es el conjunto de DFs

```
resul = X
while (hay cambios en resul) do
	for (cada dependencia funcional Y -> Z en F) do
		if (Y C resul) then 
			resul += Z
```

resul va a ser X⁺ y debería tener todos los atributos de la relación para ser CC. Sino hay q buscar otra
resul: sumar email,#Plan,nombre-adicional, fecha_adicional, título, sinopsis y duración
si resul tiene todos los atributos del esquema, significa que la CC está  bien

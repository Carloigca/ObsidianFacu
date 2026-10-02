#### Reglas de X -> Y
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
SUSCRIPCION (#suscripcion, email, nombre_usuario,#plan, nombre_plan, texto_condiciones, precio, email_adicional, nombre_adicional,#contenido, titulo, sinopsis, duracion, fecha_adicional)

df1:#Suscripción -> email,#plan
df2: email -> nombreUsuario
df3: email_Adicional -> nombre_adicional
df4:#suscripción, email_adicional -> fecha_adicional
df5:#Plan -> condiciones, nombre, precio
df6:#Contenido -> título, sinopsis, duración

CC: (#suscripción,#Contenido, email_adicional)
- El esquema cumple BCNF? No porque .#plan no es superclave de R, entonces particiono tomando la **df5**

R1(==#plan==, condiciones, nombre, precio) 
R2(==email==, nombreUsuario, ==email_Adicional==, nombre_adicional,==#suscripción==, fecha_adicional,==#Contenido==, título, sinopsis, duración)
-R1 no pierde información ya que con el .#suscripcion en la df1 recuperamos .#plan por transitividad, que es la clave en R1
-R2 no cumple con BCNF porque se da que el X de la df2 no es superclave de R2

- Vuelvo a particionar a partir de R2, tomo la **df2**
R3(email, nombreUsuario). 
R4 (email, ==email_Adicional==, nombre_adicional,==#suscripción==, fecha_adicional, ==#Contenido==, título, sinopsis, duración)
-R3 No pierde info, ya que con el .#suscripcion obtenemos mail por transitividad, y este es clave en R3
-R4 no cumple con BCNF ya que la X(#suscripcion) de la df1 no es superclave en R4

- Vuelvo a particionar a partir de R4, tomo **df1**
R5 (#suscripción, email, plan). R5 no pierden info 
R6 (==email_Adicional==, nombre_adicional,==#suscripción==, fecha_adicional,==#Contenido==, título, sinopsis, duración)
-R5 no pierde información ya que en la intersección entre R5 y R6 obtenemos .#suscripcion que es clave en R5
-R6 no cumple BCNF porque la X(emailk_adicional) de df3 no es superclave de R6

- Vuelvo a particionar a partir de R6, tomo **df3**
R7 (email_adicional, nombre_adicional)
R8 (==email_Adicional==,==#suscripción==, fecha_adicional,==#Contenido==, título, sinopsis, duración)
-R7 no pierde información ya q en la intersección entre R7 y R8 obtenemos email_adicional que es clave en R7
-R8 no cumple con BCNF ya que la X(#contenido) de df4 no es superclave en R8. 

- Vuelvo a particionar a partir de R8, tomo df4
R9 (#suscripcion, email_adicional, fecha_adicional)
R10 (==email_Adicional==,==#suscripción==, ==#Contenido==, título, sinopsis, duración)
-R9 no pierde información ya que la intersección entre R9 y R10 nos da email_adicional,#contenido que son clave en R9
-R10 no cumple BCNF ya que la X(#contenido) de df6 no es superclave en R10 (no puedo obtener email usando .#contenido como clave)

- Vuelvo a particionar a partir de R10, tomo df6
R11 (==#Contenido==, título, sinopsis, duración)
R12 (==email_Adicional==,==#suscripción==, ==#Contenido==)
-R11 no pierde información ya que en la intersección con R12, se obtiene .#contenido que es clave en R11
-R12 cumple BCNF ya que es igual a la cc, y todas las dependencias funcionales que existan en R12 serán triviales.


---
# Anotaciones mías para particionar
Hermano, la justificacion es la siguiente: 
-te fijas qué clave primaria de una df no es superclave, y en base a esa df particionas dsp
-al particionar, dejas la clave primaria de la 1er particion en la 2da partición, y todos los demás atributos de la 1er partición, desaparecen en la 2da partición
-dsp de particionar, cuando te quede algo igual a la CC, justificas como puse para R12, es mecánico

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

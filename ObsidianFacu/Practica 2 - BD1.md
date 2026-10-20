
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

---
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
[[Pract 2-Ejer 8]]
#### Ejercicio 7
[[Pract 2-Ejer 7]]
#### Ejercicio 9
[[Pract 2-Ejer 9]]
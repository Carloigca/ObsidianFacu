#clase 
#### Taller
![[Pasted image 20260916084013.png]]
df1(idGrupo->nombreGrupo,vocalista)
df2(idOrganizador->nombreOrganizador)
df3(idGrupo, idIntegranteGrupo, idRecital->marcaInstrumento)

No se cumple BCNF porque idGrupo no es superclave de RECITALES.
Particionamos considerando la df1
R1(idGrupo, nombreGrupo, vocalista)
R2(idGrupo, idRecital, Id IntegranteGrupo, IdOrganizador, marcaInstrumento, nombreO)

Perdimos información?  No, R1 Intersección R2 = {idGrupo} fue clave en R1
Perdimos dfs? No: 
df1 -> R1
df2 y df3 -> R2

R1 cumple BCNF? Sí, en R1 vale df1, tal que {idgrupo} superclave en R1
R2 cumple BCNF? No, en R2 vale por ejemplo df2, tal que {idRecital, idGrupo, idIntegranteGrupo} no es superclave en R2

Particiones en BCNF
R1 ()
R3 ()
R5 ()
R6 ()

Para analizar la 4ta forma, hay que ver las DM
R1, R3 y R5 no tienen DM, analizamos R6
DM válidas de R6:
DM1= idRecital,IdGrupo ->> idIntegranteGrupo
DM2= idRecital ->> idOrganizador
R6 no cumple 4FN, ya que al menos DM1,, tal que no es trivial en R6. Particiono R6 considerando DM1

En la explicación del ppt, DM1 es DM2 (están invertidos)
![[Pasted image 20260916092404.png|599]]
![[Pasted image 20260916092544.png|595]]

Dormí mal así que se me pasó mal la primera parte

---
### Álgebra relacional
Operaciones

#### Selección
Operación unaria que requiere una condición booleana
Si tengo R(a,b,c) puedo poner que solo se tomen las tuplas que cumplan la condición

#### Proyección
Operación unaria donde indicamos los atributos que queremos tener de la tabla

#### Producto Cartesiano
Se toman dos tablas y se juntan todos sus atributos, teniendo todas las tuplas posibles. 
Teniendo R1 y R2, a cada atributo de R1 le corresponderá una tupla con todos cada atributo de R2

#### Unión compatible
Se da cuando dos tablas tienen un atributo del mismo dominio

#### Unión


La profe comenzó a correr explicando todas (Ya me salté alguno creo) las operaciones del álgebra Relacional

#### División
A % B
Van a quedar todos los atributos de A que tengan al menos una tupla con cada atributo de B.![[Pasted image 20260916101435.png]]

![[Pasted image 20260916101957.png]]

![[Pasted image 20260916102158.png|689]]


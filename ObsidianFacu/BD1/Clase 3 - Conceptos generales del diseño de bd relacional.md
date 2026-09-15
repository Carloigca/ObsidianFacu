#clase
Clave de una relación: atributo o conjunto de atributos del que podemos determinar a todos los restantes atributos de la relación. La idea es que el atributo sea minimo (minimal). 

Ej: con Persona (DNI, nombre, apellido), DNI es clave candidata ya que desde el DNi podemos obtener el resto de información, y DNI es unívoco. 

Clave candidata: Es cuando hay más de una forma de nombrar a los atributos, entonces nombramos todas, y dependiendo de las claves candidatas, más adelante encontraremos todas las dependencias funconales. 

Ej: con Persona (DNI, Legajo, nombre, apellido), DNI y Legajo son 2 claves candidatas. 

Debe haber un conjunto minimo del lado del determinante (No sé si en alguna de las dos claves en particular). Siempre trabajamos con claves minimas

---
### Super clave
Una super clave es un conjunto de atributos que determina a todos los restantes del esquema, pero que no necesariamente es minimo. 
En Persona (DNI, NroLegajo, Carrera, Nombre), DNI es superclave. 

---
### Axiomas de Armstrong
Todas las df van a ser correctas anuque no sirvan pkara el proceso de normalización. 
Terminan encontrando df triviales 

Hay axiomas básicos y axiomas extendidos

Transitividad: Si X determina Y, e Y determina a Z, podemos decir que X determina a Z tmb

Union: Si X determina a Y, e Y determina a Z, entonces X determina a X,Z

Descomposición: si X determina a Y,Z, podemos decir que X determina a cada uno por separado

Pseudotransitividad: si X determina a Y, e Y,Z determina a W, entonces X,Z determina a W


Cuando trabajo con clausura, F como conjunto de df en un esquema R, y a X como subconjunto de R.


---
Normalización de esquemas de relación
Cuando ya tenemos las df  y las cc, debemos deshacernos de las anomalías
![[Pasted image 20260902095936.png]]

Debemos evitar perder información y dependencias funcionales. 

Validamos que no se pierda info, viendo las particiones y las intersecciones que se realizan entre los subconjunto (R1 y R2). En el ejemplo, una clave va a poder estar en R1 o en R2 (por la intersección que se da entre estos dos)

Otro concepto: Las formas normales
Son un criterio para el grado de vulnerabilidad e incosistencias y anomalías. Al llegar a un grado de forma normal, podemos asumir que se cumplen todos los grados anteriores. 
Tenemos primera, segunda, tercera, y Boyce Codd 
Primera: El esquema no debe tener atributos polivalentes o compuestos
2da, 3era y BC: Se determinan a partir de las DF
(((BCNF = Boyce Codd Normal Form)))
BCNF: Dado X->Y, X va a ser SuperClave o la df X->Y va a ser DFTrivial



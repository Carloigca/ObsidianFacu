#clase
Cambiando la semántica de los atributos, las dependencias funcionales cambian. 

Un esquema de relación está normalizado en BCNF si, siempre que una dependedncia funcional de la forma X-> A es válida en R, entonces siempre se cumple que:
- a
- a

X->y trivial si y solo si YcX

---
Como llevar un esquema R a BCNF (Cuando se puede):
- Hallar DF y CC
- 1- Analizar si en el equema R 
- .
![[Pasted image 20260909091806.png]]
---
Ejercicio FIESTAS:

(#Salón) -> dirección, Capacidad
Fecha_Fiesta,#Salón, DNI_invitado -> mesa_invitado
dni_Invitado -> nombre_invitado

CC: (#Salón, fecha_fiesta, dni_invitado, servicio_contratado)
Como puede haber más de un servicio contratado por fiesta, lo debemos incluir como clave porque sino no podemos identificarlo. 

Vamos a ver si cumple BCNF, Particiono

F1(==#Salón==, direccion, capacidad)
F1 cumple BCNF ya que en ella vale df1, tal que {#salón} es clave en R1

F2(==#salón, fecha_fiesta, dni_Invitado==, nombre_invitado, ==Servicio_contratado==, Mesa_invitado)
En F2, valen las df2 y df3, pero DNI_invitado no es superclave de F2, entonces F2 no cumple, y debemos volver a particionar, donde aparece F3 y F4

F3(DNI_invitado, nombre_invitado)
F4(#salón, fecha_fiesta, mesa_invitado, servicio_contratado, dni_invitado)
F4 no cumple con la definición de BCNF, ya que (salón, fecha y DNi) no son Superclave de F4. 

Entonces debemos volver a particionar.

F5 (==#Salón, fecha, dni==, mesa)
F6 (==#salón, fecha, servicio, dni==)

Ahora si ambas cumplen con BCNF, entonces quedan F1, F3, F5 y F6

---
Ejercicio ATENCIONEs de hospital
![[Pasted image 20260909092140.png]]df1= codHospital -> nombreHospital
df2= codHospital, legajoPaciente -> dniPaciente
df3= dniPaciente, codHospital -> LegajoPaciente

cc1) codHospital, LegajoPaciente, dniMédico
cc2) codHospital, dniPaciente, dniMédico

Analizamos si cumple para llevarlo a BCNF, pero no cumple, entonces hay que particionar. La profe usa A1, A2, etc., pero yo sigo con F1, F2, etc

F1(codHospital, nombreHospital)
F2(==codHospital, dniPaciente==, LegajoPaciente, ==dniMédico==)

F2 no cumple con la regla de la suiperclave (Yo la llamé así), así que hay que separar en F3 

F3(==codHospital, legajoPaciente==, dniPaciente)
Cumple BCNF ya que no perdemos dependencias funcionales, tenemos 2 pero no hay problema 

F4 (==LegajoPaciente, codHospital, dniMédico==)
Cumple xq cualquier dependencia q busque va a ser trivial

---
Vamos  aver un algoritmo que nos ayude a validar las DF cuando no abarcan todos los atributos (?).

el elevado a + (chiquito) indica la clausura.
R sub i puede ser la primer relación y se reemplaza por sus atributos en el "cálculo". 

Repasar clase anterior (Algoritmos de Armstrong) para poder entender mejor la clausura y las uniones e intersecciones con conjuntos que realiza


---
En el ejercicio de LIBROS se pierden la 2da dependencia funcional, entonces no se cumple BCNF. 


la 3era Forma Normal pide que X es suerclave, y que A puede ser primo, o sea que forma parte de alguna clave candidata. 

Para cada una de las df no tratadas, genero una nueva partición,  debo ver si la cc quedó en algun esquema 

Llamamos normalizar hasta bcnf o 3fn al proceso que involucra:
1. Ver df
2. Ver cc
3. Particionar y ver is cumple las formas normales. Si se pierden df, puede ser 3FN, pero si no se pierden, continuo el proceso de confirmar BCNF

Las redundancias no se eliminan por trabajar con las dependencias funcionales, entonces comenzamos a ver las ==dependencias multivaluables.== Al igual que las funcionales, estas se evalúan a partir de la semántica de los atributos (su contexto)

---
#### DM (Dependencias Multivaluadas)
Se puede decir que X -->> Y si

dado un valor de X, hay un conjunto de valores Y asociados, y este conjunto de Y NO se relaciona con el conjunto de R-X-Y (R es el esquema). 
Entonces Y es independiente de los atributos R-X-Y
Decimos que X multidetermina a Y
En criollo, con un X obtenemos múltiples Y que son independientes del resto de atributos en la tabla. 

En el ejemplo de la clase, falta la doble flecha. 
idProfesor -->> idEstudiante
idProfesor -->> idCurso

##### Trivialidad en DM
Una DF trivial era una donde X->Y con Y válido en R, y YcX

La Dep Multivaluada X-->> Y, con Y válido en R, va a ser trivial si X U Y = R

Caso especial: 
Sea R un esquema de relación, podemos definir la siguiente DM 
vacío (la O tachada) -->> Y, y la leemos como que vacío multidetermina a Y

Si en una tabla (Esquema) no hay DM, podemos decir que está en la 4ta Forma Normal (4FN)

---
E (idProducto,#sucursal, idGerente)

dm1 (#Sucursal) -->> idProducto
dm2 Vacío -->> idGerente

---
En el ejemplo de las fiestas, visto antes, en la F6 puedo sacar la siguiente DM:
.#salon, fechaFiesta -->> dni_invitado. 

sigue, ver cómo se desarrolló, hizo 2

La prox clase vamos a hacer clase taller, hay q repasar todo, las DF, DM

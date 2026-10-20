MEDICION_AMBIENTAL (#medicion, #pozo, valor_medicion, #parametro, fecha_medicion, cuil_operario, #instrumento, nombre_parametro, valor_ref, descripcion_pozo, fecha_perforacion, apellido_operario, nombre_operario, fecha_nacimiento, marca_instrumento, modelo_instrumento, dominio_vehiculo, fecha_adquisicion)

df1: (#medicion -> cuil_operario,#pozo, fecha_medición)
df2: (#parametro,#medicion -> valor-medición)
df3: (#parametro -> nombre_parametro, valor_ref)
df4: (#instrumento -> marca_instrumento, modelo)
df5: (cuil_operario -> nombre, apellido, fecha_nacimiento)
df6: (dominio_vehiculo -> fecha_adquisición)
df7: (#pozo -> descripcion, fecha_perforación)

cc:#parametro,#medición,#instrumento,dominio_vehiculo


- El esquema R cumple BCNF? No ya que el X de la df7 (#pozo) no es superclave de R
Particiono a partir de R y tomo df7
R1 (#pozo, descripcion, fecha perforación)
R2 (#medicion, #pozo, valor_medicion, #parametro, fecha_medicion, cuil_operario, #instrumento, nombre_parametro, valor_ref, apellido_operario, nombre_operario, fecha_nacimiento, marca_instrumento, modelo_instrumento, dominio_vehiculo, fecha_adquisicion)
-R1 no pierde información ya que al hacer la intersección con R2 obtengo .#pozo que es clave de R1 
-No se pierden dependencias funcionales ya que por validación simple en R1 vale df7, y en R2 valen df1, df2, df3, df4, df5 y df6
-R2 no cumple con BCNF ya que el X de la df5 no es superclave en el esquema R2

- Particiono R2 tomando df5
R3 (cuil_operario, nombre, apellido, fecha_nacimiento)
R4 (#medicion, #pozo, valor_medicion, #parametro, fecha_medicion, cuil_operario, #instrumento, nombre_parametro, valor_ref, marca_instrumento, modelo_instrumento, dominio_vehiculo, fecha_adquisicion)
-R3 no pierde información ya que en la intersección con R4 obtenemos cuil_operario que es clave de R3
-No se pierden DFs ya que por validación simple en R3 vale df5, y en R4 valen df1, df2, df3, df4 y df6
-R4 no cumple con BCNF ya que el X de la df1 no es superclave en el esquema R4 

- Particion R4 tomando df1
R5 (#medición, cuil_operario,#pozo, fecha_medición)
R6 (#medicion, valor_medicion, #parametro, #instrumento, nombre_parametro, valor_ref, marca_instrumento, modelo_instrumento, dominio_vehiculo, fecha_adquisicion)
-R5 no pierde información ya que en la intersección con R4 obtengo .#medicion que es clave en R5
-No se pierden DFs ya que por validación simple, en R5 vale df1, y en R6 valen df2, df3, df4 y df6
-R6 no cumple BCNF ya que el X de la df2 (#medicion,#parametro) no es superclave en el esquema R6

- Particiono R6 tomando df2
R7 (#medicion,#parametro, valor_medicion)
R8 (#medicion,#parametro,#instrumento, nombre_parametro, valor_ref, marca_instrumento, modelo_instrumento, dominio_vehiculo, fecha_adquisicion)
-R7 no pierde informacion ya que en la interseccion con R8 obtengo .#medicion,#parametro que son las claves de R7. 
-No se pierden dependencias funcionales xq por validación simple, en R7 vale df2 y en R8 valen df3, df4 y df6
-R8 no cumple con BCNF ya que con la X de la df3 (#parametro) no es superclave en el esquema.

- Particiono R8 tomando df3
R9 (#parametro, nombre_parametro, valor_ref)
R10 (#medicion,#parametro,#instrumento, marca_instrumento, modelo_instrumento, dominio_vehiculo, fecha_adquisicion)
-R9 no pierde información ya que en la intersección con R10 obtengo .#parametro que es la clave en R9
-No se pierden dependencias funcionales xq por validación simple, en R9 vale df3, y en R10 valen df4 y df6
-R10 no cumple con BCNF ya que la X de la df4 (#instrumento) no es superclave del esquema 

- Particiono R10 tomando df4
R11 (#instrumento, marca_instrumento, modelo_instrumento)
R12 (#medicion,#parametro,#instrumento, dominio_vehiculo, fecha_adquisicion)
-R11 no pierde información ya que en la interseccion con R12 obtengo .#instrumento que es la clave en R11
-No se pierde DFs ya que por validación simple, en R11 vale df4, y en R12 vale df6
-R12 no cumple con BCNF ya que la X de la df6 (dominio_vehiculo) no es superclave en el esquema 

- Particiono R12 tomando df6
R13 (dominio_vehiculo, fecha_adquisicion)
R14 (#medicion,#parametro,#instrumento, dominio_vehiculo)
-R13 no pierde información ya que en la intersección con R14 obtengo dominio_vehiculo que es clave en R13
-No se pierden DFs ya que por validación simple, en R13 vale df6
-R14 cumple con BCNF ya que como los atributos son los mismos que la cc, todas las dependencias funcionales que pueda obtener de R14 serán triviales

Resultado:
R1 (#pozo, descripcion, fecha perforación)
R3 (cuil_operario, nombre, apellido, fecha_nacimiento)
R5 (#medición, cuil_operario,#pozo, fecha_medición)
R7 (#medicion,#parametro, valor_medicion)
R9 (#parametro, nombre_parametro, valor_ref)
R11 (#instrumento, marca_instrumento, modelo_instrumento)
R13 (dominio_vehiculo, fecha_adquisicion)
R14 (#medicion,#parametro,#instrumento, dominio_vehiculo)

DM1: (#medicion->->#parámetro)
DM2: (#medicion->->#instrumento)
DM3: ({ }->->dominio_vehiculo) 
//como dominio Vehículo no está multideterminado por ninguna otra clave, lo multidetermina el vacío
- R14 no está en 4FN ya que la DM3 es válida en el esquema y no es trivial. Particiono R14 tomando la DM3

R15 (dominio_vehiculo)
R16 (#medicion,#parametro,#instrumento)
-R15 está en 4FN porque todas las DMs válidas en el equema son triviales 
-R16 no está en 4FN ya que la DM1 es válida en el esquema y no es trivial 

- Particiono R16 tomando DM1
R17 (#medicion,#instrumento)
R18 (#medicion,#parametro)
-R17 está en 4FN ya que en el esquema es válida la DM1, y es trivial
-R18 está en 4FN ya que en el esquema es válida la DM2, y es trivial

Resultado:
Los esquemas R1, R3, R5, R7, R9, R11 y R13 no tienen DMs, entonces están en 4FN. R15 y  R18 son proyecciones. 
R1 (#pozo, descripcion, fecha perforación)
R3 (cuil_operario, nombre, apellido, fecha_nacimiento)
R5 (#medición, cuil_operario,#pozo, fecha_medición)
R7 (#medicion,#parametro, valor_medicion)
R9 (#parametro, nombre_parametro, valor_ref)
R11 (#instrumento, marca_instrumento, modelo_instrumento)
R13 (dominio_vehiculo, fecha_adquisicion)
R17 (#medicion,#instrumento)


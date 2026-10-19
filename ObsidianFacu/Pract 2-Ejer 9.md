FESTIVALES (#festival, denominacion_festival, localidad, cuil_musico, nombre_musico,
fecha_nacimiento, #banda, nombre_banda, estilo_musical, #tema, nombre_tema, duracion,
instrumento, cuil_auspiciante, url_plataforma_entradas, #sponsor)

df1: .#festival -> denominación_festival, localidad
df2: .#banda-> nombre_banda, estilo_musical
df3: cuil_musico-> nombre_musico, fecha_nacimiento
df4: .#tema,#banda,#festival -> nombre_tema, duracion
df5: .#tema,#banda,#festival , cuil_musico -> instrumento

cc: .#festival,#banda, cuil_musico,#tema, cuil_auspiciante, url_plataforma_entradas,#sponsor

- El esquema R cumple BCNF? No ya que la X (#festival) de la df1 no es superclave de R
Particiono a partir de R y tomo df1
R1 (#festival, denominación_festival, localidad)
R2 (#festival, cuil_musico, nombre_musico,
fecha_nacimiento, #banda, nombre_banda, estilo_musical, #tema, nombre_tema, duracion, instrumento, cuil_auspiciante, url_plataforma_entradas, #sponsor)
-R1 no pierde información ya que al hacer la intersección con R2 obtengo .#festival que es clave de de R1
-No se pierden dependencias funcionales ya que por validación simple en R1 vale df1, y en R2 valen df2, df3, df4 y df5
-R2 no cumple BCNF ya que el X de la df2 (#banda) no es superclave en el esquema

- Particiono R2 tomando df2
R3 (#banda, nombre_banda, estilo_musical)
R4 (#festival, cuil_musico, nombre_musico,
fecha_nacimiento,#banda,#tema, nombre_tema, duracion, instrumento, cuil_auspiciante, url_plataforma_entradas,#sponsor)
-R3 no pierde información ya que en la intersección con R4 obtengo .#banda que es clave de R3
-No se pierden DFs ya que por validación simple, en R3 vale df2, y en R4 vale df3, df4 y df5
-R4 no cumple BCNF ya que la X de la df3 (cuil_musico) no es superclave en el esquema

- Particiono R4 tomando df3
R5 (cuil_musico, nombre_musico, fecha_nacimiento)
R6 (#festival, cuil_musico,#banda,#tema, nombre_tema, duracion, instrumento, cuil_auspiciante, url_plataforma_entradas,#sponsor)
-R5 no pierde información ya que en la intersección con R6 obtengo cuil_musico que es clave en R5
-No se pierden DFs ya que por validación simple, en R5 vale df3, y en R6 valen df4 y df5
-R6 no cumple BCNF ya que la X de la df4 (#tema,#banda) no es superclave ne el esquema 

- Particiono R6 tomando df4
R7 (#tema,#banda,#festival,nombre_tema,duración)
R8 (#festival, cuil_musico,#banda,#tema, instrumento, cuil_auspiciante, url_plataforma_entradas,#sponsor)
-R7 no pierde información ya que en la intersección con R8 obtengo (#tema,#banda,#festival) que es clave en R7
-No se pierden DFs ya que por validación simple, en R7 vale df4, y en R8 vale df5
-R8 no cumple con BCNF ya que la X de la df5 (#tema, cuil_musico) no es superclave del esquema

- Particiono R8 tomando df5
R9 (#tema,#banda,#festival, cuil_musico, instrumento)
R10 (#festival, cuil_musico,#banda,#tema, cuil_auspiciante, url_plataforma_entradas,#sponsor)
-R9 no pierde información ya que en la intersección con R10 obtengo (#tema,#banda,#festival, cuil_musico) que es clave en el esquema
-No se pierden DFs ya que por validación simple, en R9 vale df5
-R10 cumple con BCNF ya que los atributos son los mismos que la cc, por lo que todas las DFs que pueda obtener de este esquema, van a ser triviales

Resultado: 
R1 (#festival, denominación_festival, localidad)
R3 (#banda, nombre_banda, estilo_musical)
R5 (cuil_musico, nombre_musico, fecha_nacimiento)
R7 (#tema,#banda,#festival,nombre_tema,duración)
R9 (#tema,#banda,#festival, cuil_musico, instrumento)
R10 (#festival, cuil_musico,#banda,#tema, cuil_auspiciante, url_plataforma_entradas,#sponsor)

---
DM1:#festival ->> cuil_auspiciantes
DM2:#festival ->> url_plataforma_entradas
DM3: { } ->>#sponsor //Me hablan de un registro, no de cómo obtener los sponsor
DM4:#tema,#banda,#festival ->-> cuil_musico

-R10 no está en 4FN ya que la DM3 es válida en el esquema y no es trivial.

- Particiono R10 tomando la DM3
R11 (#sponsor)
R12 (#festival, cuil_musico,#banda,#tema, cuil_auspiciante, url_plataforma_entradas)
-R11 está en 4FN porque todas las DMs válidas en el esquema son triviales
-R12 no está en 4FN ya que la DM1 es válida en le esquema y no es trivial

- Particiono R12 tomando la DM1
R13 (#festival, cuil_auspiciantes) 
R14 (#festival, cuil_musico,#banda,#tema, url_plataforma_entradas)
-R13 está en 4FN ya que todas las DMs válidas en el esquema, son triviales
-R14 no está en 4FN ya que la DM2 es válida en el esquema y no es trivial

- Particiono R14 tomando la DM2
R15 (#festival, url_plataforma_entradas)
R16 (#festival, cuil_musico,#banda,#tema)
-R15 está en 4FN ya que todas las DMs válidas en el esquema, son triviales
-R16 está en 4FN ya que la DM4 es válida  en el esquema y es trivial

R1 (#festival, denominación_festival, localidad)
R3 (#banda, nombre_banda, estilo_musical)
R5 (cuil_musico, nombre_musico, fecha_nacimiento)
R7 (#tema,#banda,#festival,nombre_tema,duración)
R9 (#tema,#banda,#festival, cuil_musico, instrumento)
R11 (#sponsor)
R13 (#festival, cuil_auspiciantes) 
R15 (#festival, url_plataforma_entradas)
R16 (#festival, cuil_musico,#banda,#tema)
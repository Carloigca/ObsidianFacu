#clase 

# Nuevos requerimientos

1. Familiograma: Al dar de alta un paciente, se puede dar de alta familiares. Si ese familiar se hace un estudio o ya se lo hizo, tiene q quedar linkeado con la tabla de familiares del paciente original. Se pueden linkear con algun nro ya sea dni o algun id. 
2. Mililitros de la sangre: Puede pasar q cuando la muestra llega a corea, por alguna razón esta muestra tenga baja calidad. En ese caso el administrativo puede indicar que se rehaga el estudio. Asumimos q el administrativo recibe un mail donde le dicen y este lo indica en el sistema. 
Ignoramos el hecho de las 3 muestras
3. Por temas de auditoría médica, se deben registrar todas las modificaciones en las entidades principales. Estudios con fecha y usuario de los cambios de estado. Lo mismo para los cambios en paciente si se cambia un familiar, dirección, cambios en su información, fecha de alta. Las entidades principales son estudio, muestra y paciente.
El historial es clave xq cuando hagamos el tablero, vamos a querer saber los pasos que más se demoran en el proceso 

---
# Recomendaciones
Tener en cuenta la charla sobre ia, pero ir probando, no nos detengamos tanto en la parte del prompt. El uso de ias no debe ser perfecto necesariamente. Debe ser lo más parecido a un proyecto real. 
No nos van a preguntar cosas puntuales de código, pero deberíamos tener noción de todos los conceptos, arquitecturas y tecnologías usadas. 
Capaz nos pidan ejemplos de correcciones que le hicimos a la ia, o sea momentos donde no nos pareció bien lo q hizo la ia.

Una diapo general, y dsp una diapo para cada endpoint. El funcionamiento interno no hay q explicarlo, lo explicamos como una caja negra. damos parámetros de entrada y parámetros de salida, y manejos de errores, cuáles son los errores de cada endpoint y cómo los representan. Hay q pensarlo como usuarios de la API

Le preguntamos a Braian por Telegram cosas de Supabase

---
Las variabtes pueden ser benignas, malignas o bus. Bus quiere devir q no hay evidencia, entonces no se puede hacer diagnostico. Si hay bus, se le pone una fecha en el año de q hay que revaluarlo. Se debe agregar una fecha para el paciente. Se volvería a la parte de evaluacióon de las variantes (bioinformáticos) dentro de un año. Podría darse que en ese año aparezca más informacióon para la base de datos q usan los bioinformáticos. 

Deno es typescript/Javascript


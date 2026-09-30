#clase 
Actividades:
- Transcripción, glosario LEL, Escenarios 
  Para el 11/10 hasta 11:59
- Prototipo de solución (Las pantallas de la aplicación)
  Domingo 11/10 hasta las 11:59. 
  Hay que mandar video y reflexionar sobre la entrevista

Mañana nos sube el audio de la grabación y un google form para reflexionar y ponernos en esa situación. 

---
### Entrevista
Con puntos las preguntas, y abajo las respuestas
- Nombre del lab: No está definido, podemos definirlo
- Logo q identifique? No está definido, debe contener la molécula de ADN
- Paleta de colores? No, proponerla
- Horarios? Depende de la actividad que se esté haciendo. Para las extracciones de muestras son de 9 a 18. La toma de muestras se pueden hacer en 100 laboratorios a lo largo del país con el mismo horario
- Quiénes interactúan? Varios roles, dsp suma más. Médicos q derivan estudios (médicos tratantes) y tienen la consulta inicial, sospechan una enfermedad genética y derivan al paciente
- Qué estudios realizna? Se concentran en el exoma clínico. Nos permite analizar el ADN del paciente. El ADN es una palabra larga de 3 mil millones de letras donde cada letra puede tomar 4 valores, A,C,T,G. Dentro de esa gran palabra, tenemos regiones llamadas Genes. Un Gen es una subsecuencia de la gran palabra usando esas mismas letras. Están configuradas las secuencias para generar proteínas. Ante una mutación, la instrucción es correcta y la proteína generada es incorrecta y no funcional. Hay q poder leer todos los genes del ADN desde la muestra de sangre. 
- Los estudios llegan a través de los médicos tratantes. Estos dan de alta el estudio siempre
- Los estudios son por orden de llegada
- Los estudios tienen las etapas: Lo da el médico, llega al estudio, y se hace el procesamiento
- Este procesamiento incluye los datos: Cuando se da de alta, los datos q completa el médico son: Paciente (para el q se pide el estudio), si no existe se tiene q dar de alta, y la lista de genes q quiere evaluar, y si quiere evaluar o no, hallazgos secundarios. La lista de genes depende de la patología q se esté sospechando, ya q cada patología se da por el malfuncionamiento de una proteína específica. 
- Cómo sigue el proceso? Una vez q se da de alta el estudio, hay q emitir el presupuesto para el paciente. eso lo hace el ==administrativo del laboratorio==. 
- El paciente se vincula? Si, ya q cuando el médico derivante lo da de alta, l ellega un mail con usuario y contraseña para iniciar sesión y cambiar la contraseña. 
- Se puede hacer más de un estudio al paciente.
- Cuando el médico selecciona el Gen, y puede marcar secundarios, hay un delay entre las entregas de resultados o son todos de una? La info del ADN llega toda junta. El análisis posterior se hace todo jjunto. Se analizan los genes de la patología sospechada, y si se marcaron hallazgos secundarios, se analizan los genes secundarios. Para esto hay un módulo al que le podemos consultar a la API, se llama ==Bio informática API==. Asumimos q existe y dsp lo usamos
- Exoma: Se analiza y se informa al médico, enviando el resultado que queda disponible para q el paciente lo pueda bajar. en la misma página. Se debe informar mediante un mail
- El formato del resultado es PDF sigueindo una plantilla que nos van a dar. Informando positivo o negativo e informando la variante genética. Una variante genética es cualquier letra del  ADN del paciente q sea diferente al genoma de referencia (Genoma Humano). No todas las variantes son patogénicas (malas), pueden ser benignas, patogénicas, otra más, y VUS(Variante de significado incierto). 
- El médico es el q da de alta y se le manda un presupuesto al paciente, y dsp? El paciente paga. Lo puede cubrir la obra social o lo puede pagar de forma particular. cada obra social cubre de forma distinta. Cada obra social y sus porcentajes de cobertura están en una API que nos van  a proveer. En caso de q elija pagar de forma particular, el usuario accede a la página para realizar el pago. Este pago va a ser simulado, y debe quedar un registro financiero de todas las transacciones (por obra social y por pago particular). 
  Después de pagar, el paciente tiene q elegir un turno en alguno de los centros de extracción
- El sistema de turnos? Se dan cada 15 minutos, cada centro de extracción tiene su propio turnero. Una vez q se elige  un centro de extracción, se elige el turno. El horario es de 9 a 18. 
- Un médico da de alta a un paciente, qué datos da? DNI, nombre, apellido, telefono, mail, diagnóstico presuntivo (alguna de las 5 patologías manejadas), el resumen de la historia clínica (hay una tecnología de transcripción q debería implementarse en el sistema para q el médico grabe el historial del paciente), la lista de síntomas del paciente (No se deben escribir a mano, debe usarse un estándar de tecnología médica que se llama ==Snomed==, para que todos codifiquen los síntomas de igual manera. Hay un equipo trabajando en Snomed). 
- 5 patologías, tienen distintas formas de tratarse? Las 5 patologías son distintas y cada una evalúa un gen distitno. En una célula tneemos un nucleo y los lisosomas, que degradan y recilan deshechos internos de la célula, si andan mal, los deshechos se acumulan y se produce una patología, entonces en base a la proteína q no anda, sabemos la patología. Las patologías Lisosomales son: Fabry, Pompe, Mucopolisacaridosis 1, Gaucher, ASMD. Cada una provoca disfunción en uno o más órganos. Un paciente podría tener más de una, por eso se sospechan más de una. Si el médico no lo pidió no se dan resultados de más. 
  
  Hay una funcionalidad en la q se hace solo un reanálisis de su estudio, así no se vuelve a sacar sangre y se vuelve a estudiar el ADN. Entonces cada paciente podría tener más de un análisis y este/estos pueden usarse para otro análisis posterior
- Además de los sńtomas y el diagnóstico presuntivo, tneemos el familiograma (Genograma o algo así), ya que como es genético, hay un patrón q puede heredarse en la familia. Cuando damos de alta al paciente, hay que indicar familiares con: Parentesco, nombre, lista de síntomas, y si tiene un diagnóstico hecho. El médico carga los familiares, estos se mandan a la API, y esta devvuelve un PDF con toda la información todo se da en la misma consulta, info del paciente y de su familia. El inicio de todo esto es una consulta médica completa, no hay q asumir q el paciente vuelva. 
- Tmb registramos del paciente si la sospecha de la patología es una sospecha clínica (Viendo sus sintomas), o si es una sospecha familiar (Viendo su herencia en familiares (Padres, primo, madre, hermanos, tios, etc))
- Fmiliograma: solo se sube si el paciente da la información, pero puede ser opcional, porque no den información los pacientes. 
- Otro dato del paciente? No, familiograma, info del paciente, patologías sospechadas, etc
- La lista de genes se determina en la API, le informamos patología y nos da los genes asociados. Pero el mpedio ==Puede agregar== genes a analizar. 
- Una vez q el paciente paga y asiste al turno, ante sdel informe, hay un paso intermedio? Hay un tema de logística. Ejemplo: Cuando el paciente José eligió el turno, acude al laboratorio. En el sistema no tenemos un control de la "previa" a que el paciente acuda al laboratorio por la muestra, pero deberíamos agregar un recordatorio para el paciente por mail. Una vez q se tomó la muestra, dbee haber un num de laboratorio, entonces cuando se registra q el paciente tomó la muestra, le debe llegar un msje al numero y que el estado del número pase a ser "Listo para retirar". Entiendo q se le avisa al que analiza la muestra.
- Hay otro tipo de estados para el estudio? Siempre está en un estado puntual "Creado", "Turno sacado", "Listo para retirar"
- Área central: Si bien estamos distribuidos en el país, hay un lab central donde llegan todas las muestras, xq la lectura del ADN la hacemos en Corea, entonces se unifican todas las muestras en un lugar y salen de ahí. Salen de a lotes, y eso lo maneja el sistema. Cuando llegan al laboratorio, el sistema arma lotes de 10 muestras y los envía. Si no se llegan a 10, no se envían, se quedan esperando. VER AL FINAL PARA CUANDO TIENEN MENOS DE 4 MESES (el paciente)
- La respuesta de Corea cómo llega? Llega directamente un PDF por el mail (que lo recibe la secretaria por fuera del sistema), para cada paciente, donde se indica el paciente y el resultado presuntivo (?). El ==administrativo== del laboratorio carga los resultados de cada paciente, de forma manual, en el sistema. Los lotes se envían de a 10, pero cada estudio se recibe individual, ya que cada estudio puede tardar más. No necesitamos manejar los resultados en Lotes
- Cuando se cargan los resultados se avisa al médico y al paciente. 
- Se vuelve a algun estadío previo? Si, hay casos donde la muestra (por alguna razón de mal mantneimiento), el ADN se desnaturaliza o es insuficiente. En ese caso se vuelve a sacar la muestra al paciente, pero sin costo para el paciente. Se informa al paciente y médico por mail. El paciente vuelve a sacar turno, y no se crea un nuevo estudio, es el mismo
- El costo del turno de la extracción se incluye en el costo del estudio. Cuando el cliente paga recién es habilitado a sacar el turno. Cuando es por obra social, el trámite debe ser autorizado
- La autorización de la obra social la hace el administrativo, lo gestiona por teléfono y este carga si se autorizó o rechazó. Después el paciente puede cargar la demanda del estudio y pedir que se vuelva a intentar autorizar (puede darse q el paciente tramite con la obra social (por juicio xd) entonces dsp se debería poder). El paciente debería poder subir documentación de la decisión favorable de la justicia en formato PDF. 
- Se hace un único pago, no hay cuotas
- A los médicos y administrativos. Los médicos se dan de alta solos, los administrativos también. 
	- El médico debe cargar matrícula, especialidades, nombre, apellido, telefono y mail. Las especialidades son estandarizadas, buscar
	- Los administrativos usan nombre, apellido, telefono y mail
- No hay restricción para el almacenamiento de datos
- Cuando llegan los resultados de Corea, el administrativo en el área central los carga, y? Ahora los datos crudos del equipo biotecnológico se almacenan, y un equipo de bioinformática los analiza. Corea manda los datos crudos en archivo de texto con ACTTG, y este pasa por un análisis de un ==bioinformático==, que va a informar los resultados q encontró. Este lo carga en el sistema
- Un bioinformático tiene Nombre, apellido y mail. No hay que simular su trabajo, solo hay q informarle mediante un mail a todos los bioinformáticos cuando llega un resultado. Estos ingresan y registran las variantes q encontraron para los genes para los q se pidió el análisis. 

- Habló de un tablero de costos y ganancias, qué datos debería tener? Lo van a definir bien después, pero la gerencia debe poder ver tiempos, cuellos de botella del proceso, ver caja, ver retrasos en pagos y las deudas, pero todavía se tiene q trbaajr y desarrollar. En otra reunión nos explica
- La gerencia solo accede, pero el tablero todavía no se pide en el sistema, capaz se hace en un sistema a parte
- Rol de administrativo: Hay solo administrativos en la Central. Los laboratorios de extracción no operan en el sistema, solo envían msje al numero que nos dijo (no). 
- La muestra de sangre queda asociada al paciente o cómo se guardan los datos? Se registra la cantidad de mililitros que tiene el tubo de la muestra. De hecho, si tiene menos de 2ml, hay que remuestrear sin volver a costear. 
- Las restricciones del paciente si no asiste muchas veces o si inicia mal? Ya pagó, así q si sacó turno es q pagó. Hay que registrar la asistencia, recordar el nuevo turno, y contar las faltas. Si a la 5ta vez no asiste, pierde el pago. 

- Para cuando saque el turno, en la plataforma el usuario va a recibir dos cosas: La orden de extracción con fecha, hora y datos determinados (si necesita preparación especial), y además le llega el consentimiento informado, que es una plantilla para que el paciente imprima y llene para llevar al turno

- Tmb hay q registrar fecha de nacimiento para el paciente tmb. Solo el familiograma puede estar vacío, el resto de los datos son ==obligatorios==
- El bioinformático tmb está obligado a completar sus 4 datos (nom, ape, mail y teléfono). Cuando registra los datos lo hace sobre el mismo estudio
- Roles? Médico q se registra solo, da de alta al paciente. Y el administrativo del laboratorio en el central. Pero los laboratorios de extracción son exteriores al sistema, no van a acceder. 
- Las muestras llegan a la central mediante un nuevo rol. Este es el ==transportista==. 
- El transportista tiene q recibir la orden del día (qué laboratorios de su provincia debe pasar a buscar las muestras) todas las mañanas. Hay un transportista por provincia. Cuando sale de cada laboratorio, debe poder marcar las muestras tomadas del laboratorio del q salió, y las muestras de ese laboratorio deben cambiar de estado
- Debería poder verse un mapa para ver por dónde vienen las muestras. Hay un modelo q hace optimización de recorridos, pero de momento, para armar la orden del día, le pasamos las direcciones para el transportista y el módulo arma el recorrido. Una vez generado el recorrido no se modifica, pero el transportista va indicando en cada parada que pasó. 

- Los lotes son Sample Sets. los estudios son Samples.
- Los lugares de extracción informan q tienen las muestras al telegram del laboratorio.

- cuando un bioinformático toma una muestra, la muestra pasa a un estado "En análisis". Distinto del estado "En bioinformática" (que sería como esperando un analisis del bioinformático)
- Cuando se pide por obra social, se espera la autorización. El pago de las obras sociales es a parte, entonces cuando se autoriza un estudio, ya se considera pagado (aunque el pago de la obra social llegue un día dsp)
- Un bioinformático solo carga información si haía variantes en los genes analizados, y cuál es la variante q se encontró relacionada a cada gen
- Precios para cada estudio? El estudio sale 500USD hasta 5 genes. Cada gen adicional son 30USD adicionales. el precio es fijo
- Seguridad de los datos? Es clave la seguridad, si se filtran nos podrían hacer un juicio. Máxima atención a la seguridad de datos y autenticación. Estaría bueno un 2do factor de autenticación
- Transcripción de audio, por celu? Si, el sistema debe ser responsive. 
- El paciente q recibe contraseña inicia sesión y cambia contraseña, algo más? No, con el cambio de contraseña ya puede acceder. Desde otra red (otra IP), se debe poder autenticar o validar q sea el mismo usuario (Se me ocurre a mi l enviar al mail registrado)
- Cuando las muestras salen a Corea, debemos tener un permiso del Ministerio de salud. Antes de q la muestra viaje, hay q emitir un CCV (archivo Excel) con los pacientes q se mandan, esto s emanda al ministerio y este devuelve un PDF con los pacientes q se pueden enviar. Hay q asumir otra API a la q le pasamos el CCV y nos devuelve el PDF. Los dos archivos se asociann al estudio. La información de los pacientes en el CCV es la misma q debería devolver el PDF
- Cuando el médico se registra. se debe validar la matrícula con una API del ministerio de salud? NO, asumimos q entró una matrícula válida}
- Si hay un percanse con el envío del transportista? Está tercerizado, así q asumamos q llegan y listo.
- Se puede pagar de forma simulada, solo debe quedar registrado financieramente para saber el crédito q tiene el administrativo. El paciente solo presiona pagar. 
- Hay cupones de descuento. El paciente cuando va a pagar, puede poner "Aplicar cupón". 
- Circuito: Gerente médico (Genera cupones y los asigna a APM (que son los comerciales o valijas) que visitan a los médicos derivantes), APM, médicos derivantes, administrativo
- Corrección: El gerente médico asigna cupones para los q define un desucento determinado, y se los asigna a cada médico derivante. tiene una cantidad limitada y define a quién se lo pasa. Un equipo va a tener para generar los cupones con un QR en PDF. Entonces para saber si el cupón es válido o si fue usado, es el módulo externo el que genera los cupones con QR (Cuando el médico lo pide), y después es el mismo módulo el que lo valida dsp
- Los pacientes NO pueden eliminar su información
- Cuando lemédico carga los síntomas, marca los q quiere y mediante la API? No, el médico pone las 3 letras que le interesan, y la API informa los patologías asociadas a las letras (Si escribe CEF ya debería mostrarle Cefalea y otras. )
- Las patologías están asociadas a un gen puntual. el módulo bioinformática las relaciona. En un gen puede haber distintas variables patológicas/patogénicas. En cualquier caso es disfuncional. Al elegir un Gen, el bioinformático va a poder ver la lista de variantes para ese gen puntual. 
- El gerente médico tmb se da de alta por si solo. En realidad o él elige el rol, o sino un administrativo los da de alta, cualquier opción viene bien.
- Los médicos no manejan los turnos. Los médicos no son del laboratorio, solo reciben al paciente e informan si sospechan patologías, etc. No tienen otra funcionalidad.
- Los gerentes médicos dan de alta las patologías, y para cada una se indica el síntoma principal/cardinal, y los sintomas secundarios. Lo puede modificar luego. 
- Médico, admin de lab, paciente, transportista, gerente médico, bioinformático. Si queremos gestionar usuarios podemos tener un Admin general. 
- Para dar los resultados se analiza que los rsultados tengan el sintoma cardinal o al menos 2 sintomas carinales de 2 sistemas distintos (corazón, riñones, etc). Pero esto no lo vamos a saber, entonces alcanza con q tenga 2 sintomas secundarios. Si no se cumple, el diagnóstico presuntivo no corresponde. Hay que validar q la patología sospechada se esté dando
- Cuando el bioinformático analiza la secuencia de ADN y mira el genoma de referencia. Las variantes genéticas de un Gen ya son conocidas y se van actualizando (pero de eso se encarga la API de bioinformática). Al cargar, elige un gen y aparece una lista de las variantes para cada gen. 
- La gravedad si es Patogénica, si es benigna, inocua, o si es "bus?" lo informa la API. 
- La patología Pompe es urgente en pacientes recién nacidos porque puede provocar la muerte.
- Si el estudio es de Pompe, en un niño o niña menor a 4 meses, se puede mandar solo, sin esperar el Lote de 10. El coste es el mismo. Es lo único q cambia. 
- Si hay un lote esperando a los 10, y llega una muestra q puede ser pompe y tienne 4 meses o menos, se envía el lote con esa muestra al momento (Si eran 5 esperando, llega el de pompe y se envían los 6)
- FedEx tiene un módulo al q le mandamos una hora de retiro así lo pasa a retirar (con día y hora). Le debemos pasar una URL donde impacte que esas muestras pasan al próximo estado. Le decimos día, hora, URL del Sample set, e impacta en el estado de los Samples y del sample Set. El que programa este horario es un administrativo. No es por turnos ni nada. Y hay un nuevo estado donde el lote espera q se programe su retiro. El horario de los retiros es el mismo de antes, lunes a viernes y de 9 a 18 (Creo, lo dijo al inicio). Capaz se habilita un sábado
- Los pacientes pueden ser de todas las edades. 
- Va a haber un chatbot para pacientes que lo está haciendo otro equipo.  Está preparado para responder preguntas sobre las 5 patologías. Le reenviamos la pregunta y le mostramos la respuesta al paciente (?)
- Trabajan de lunes a viernes, y en los feriados no deben aparecer turnos de extracción ni recorrido para el transportista
- Los recordatorios y avisos son siempre via Mail? Estaría bueno que aparezcan avisos en el mismo sistema al paciente. Una lista de avisos en el sistema cuando se logea.  
- Si hay q tener en cuenta la automatización del Telegram del laboratorio central. Si se nos ocurre otra manera, bienvenida. 


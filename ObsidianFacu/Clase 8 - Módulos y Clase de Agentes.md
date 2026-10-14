
---
# Prox semana es virtual

1. Familiograma: Recibe lista de familiares, nombre, apellido, parentesco, y  si tiene algo. Reciben la data y devuelven una imagen o PDF 
2. Módulo de términos médicos estandarizados:
Usamos tecnología SNOMED. Hay que investigarlo, capaz podemos tener base de datos propia. Recibe caracteres, y devuelve el término. Si se pudiera agregar q se escriba de forma informal (dolor de cabeza o temperatura > 39 (fiebre)) mejor. 
3. Sistema de Vouchers. El gerente médico genera los vouchers. Genera un PDF con un QR, pero además debe poder confirmar su uso. Los voucher deberían tener un identificador único (para todos los grupos)
4. CDSS: Sistema de Soporte en la toma de decisiones (?). dada una lista de síntomas, infiere la patología asociada. Primero se debe cumplir que esté presente el síntoma cardinal (de la patología), o sea q si este no está, se descarta la patología. Después el médico podría cambiar la patología 
- Patología con mayor cantidad de síntomas
- Patologgía con mayor cantidad de sintomas cardinales
- Diferencia entre sintomas totales de la patología (entre cardinales y no cardinales), y sintomas presentes en el paciente, sea la menor. 
Vamos a necesitar tener datos separados por grupo. Primero se setea todo vacío para cada grupo. 

5. APIgen: Devuelve los genes a estudiar para una patología determinada. Los hallazgos secundarios son rasgos genéticos q no estaban asociados originalmente pero que se asocian. Una variante en un gen es una letra distinto en comparación al genoma de referencia. Hay q usar una base de datos llamada "clinvar". Recibe patología, lista de variabtes, y confirma si están las variantes debidas para la patología, y además devuelve lista de hallazgos secundarios. Tiene varios endpoints: Confirrmar diagnostico, hallazgos secundarios, dame genes, dame genes para hallazgos secundarios

6. Cuando se elige obra social, hay q elegir una obra social de la lista. Se debe poder dar de alta un estudio para la obra social. Deberíamos poder hacer una web donde el grupo vaya y simule una autorización o rechazo. Se puede charlar otras opciones. Como q nos devuelvan una URL para simular la autorización o la negación

7. Módulo de avisos. Se enfrenta a las API de télegram y envío de mails, capaz de wsp también. Cuando un grupo deba hacer un aviso, se usa este módulo. 

8. Chatbot para los pacientes. Deberíamos personalizar lo más posible al chatbot para q funcione en base a los estudios genéticos. Cada vez q el paciente hace una duda, cada grupo hace el front, pero en el back le envía la pregunta a la API del chatbot

9. Optimización del recorrido. 

10. API de FedEx. Se programa un retiro usando dia, hora y lugar. Cuando el sistema se conecta a FedEx, este debe devolver una URL para pasar el pedido a en viaje a Corea. Otra alternativa es q cuando se de de alta, se devuelva una URL y que se pueda ver el viaje, para q cuando llegue el dia, hora y lugar cambie el estado.

# Nos tocó  OS6 = Obra social (6)

---
# Ingeniería de Agentes de IA

#### Qué hace un agente de ia?
- LLM: El q razona
- Instrucciones: quien es y que no puede hacer
- Tools: Suss manos
- Memoria: Lo que sabe del contexto
- Loop Repetir hasta terminar




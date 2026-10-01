#extra

---
### Buffers/Arreglos cíclicos

Como tengo una cantidad fija, debo tener un **semáforo de espacios libres**, y un **semáforo de espacios ocupados**. 

En la situación Productor/Consumidor, debo tener ==2 índices== (variables compartidas) **para indicar posición libre y posición vacía**
Para hacer que un índice avance:
```
(Teniendo un buffer/arreglo de N elementos) 
indice= indice++ mod N; 
//Va a devolver 0 cuando la siguiente posición sea igual a N 
``` 
![[Pasted image 20260930201252.png|187]] Rear=Libre /// Front=ocupado![[Pasted image 20260930201437.png|521]]

---
## Semáforos
#### Lectura de datos compartidos
Cuando tenga ==más de un consumidor==, necesito un semáforo para que no accedan al mismo recurso a la vez

#### Escritura de datos compartidos
Cuando tenga ==más de un productor==, necesito un semáforo para que no sobre-escriban la misma posición del arreglo/buffer

#### Maximizar la concurrencia
Hay que estar atento a minimizar el tiempo en el q los procesos se bloquean entre sí. 

#### Coordinación de múltiples procesos
Cuando hay que coordinar el acceso a un recurso entre varios procesos, conviene ==usar un arreglo de semáforos== donde cada proceso tenga su semáforo propio para saber cuándo acceder al recurso. 
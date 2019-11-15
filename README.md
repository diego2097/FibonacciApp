# Escalamiento en Azure con Maquinas Virtuales, Sacale Sets y Service Plans

## 1. ¿Cuántos y cuáles recursos crea Azure junto con la VM?

Cuando se crea una maquina una virtual en azure, se crean en total 7 recursos incluyendo la maquina virtual. los cuales son: 

- Una red virtual 
- Cuenta de almacenamiento 
- Direccion IP publica 
- Grupo de seguridad de red 
- Interfaz de red 
- Disco 
- Maquina virtual 

## 2. ¿Brevemente describa para qué sirve cada recurso?

### Red virtual (vnet) 

Es el bloque de creacion fundamental de una red privada en azure, permite a muchos tipos de recursos, tal como una maquina virtual, comunicarse entre 
usuarios, con redes locales e internet de forma segura. Es similar a una red tradicional pero le aporta las ventajas adicionales de la infraestructura cloud 
como escalabilidad, disponibilidad y aislamiento. 

### Cuenta de almacenamiento (storage account)

Se encarga de contener todos los objetos de almacenamiento ofrecidos por azure: archivos, colas, blobs, discos etc. la cuenta de almacenamiento provee un 
espacio de nombre unico para la informacion alojada, la cual es accesible desde cualquier parte del mundo a traves de HTTP o HTTPS.  

### Direccion IP publica

La direccion IP que tendra la maquina, la cual la hace accesible desde cualquier parte del mundo. 

### Grupo de seguridad de red

Un grupo de seguridad de red contiene reglas de seguridad que permiten o deniegan el tráfico de red entrante 
o el tráfico de red saliente de varios tipos de recursos de Azure.

### Interfaz de red 

Una interfaz de red permite que una máquina virtual de Azure se comunique con los recursos de Internet, Azure y locales. 
Al crear una máquina virtual desde Azure Portal, este crea una interfaz de red con la configuración predeterminada.

### Disco 

Se puede considerar como un disco físico en un servidor en el entorno local, pero virtualizado. Los discos administrados de Azure se 
almacenan como blobs en páginas, que son un objeto de almacenamiento de E/S aleatorio en Azure. Llamamos a administrados a estos discos
porque es una abstracción sobre los blobs en páginas, los contenedores de blobs y las cuentas de almacenamiento de Azure. Con los discos
administrados, lo único que debe hacer es aprovisionar el disco y Azure se encarga del resto.

## 3. ¿Al cerrar la conexión ssh con la VM, por qué se cae la aplicación que ejecutamos con el comando npm FibonacciApp.js? ¿Por qué debemos crear un Inbound port rule antes de acceder al servicio?

- La aplicacion se cae debido a que el proceso termina su ejecucion cuando la conexion ssh se cierra. 
- Esta normas son las encargadas de permitir o denegar el trafico de red, por defecto el puerto de la aplicacion no esta definido en estas normas por lo tanto el acceso es denegado, para esto se debe crear la regla, que permita el acceso desde el exterior a el puerto. 

## 4. Adjunte tabla de tiempos e interprete por qué la función tarda tando tiempo.

Se tomaron los tiempos de respuesta con un tamano Standard B1ls y un tamano B2ms. 

### tabla 

![Imágen 1](img/tabla_tiempos.PNG)

La funcion tarda debido a la forma en la que esta implementada internamente, ya que al cambiar el tamano los tiempos no cambian de manera 
drastica pero si cambia el consumo de CPU, de lo cual se concluye que no importa el tamano que se elija el tiempo que tarde no va a variar considerablemente.

## 5. Adjunte imágen del consumo de CPU de la VM e interprete por qué la función consume esa cantidad de CPU


### Consumo de CPU con tamano Standard B1ls

![Imágen 1](img/consumo_cpu.PNG)

### Consumo de CPU con tamano B2ms

![Imágen 1](img/consumo_cpu2.PNG)

En las dos imagenes se puede observar una diferencia considerable y es que con el tamano standar b1ls el consumo de cpu llega casi al 70% su maximo pico, en 
cambio en el tamano B2ms el consumo de cpu no alcanza a llegar al 40%.

Este consumo se debe a la cantidad de operaciones que realiza el programa para calcular el numero. 

## 6. Adjunte la imagen del resumen de la ejecución de Postman. Interprete

### tamano Standard B1ls

#### Tiempos de ejecución de cada petición

La primera iteracion tuvo un tiempo de 56.3 segundos, las demas iteraciones que no fallaron, tuvieron un tiempo de 28 segundos. 

#### Si hubo fallos documentelos y explique

Hubieron 5 peticiones que fallaron, con un resultado como este: 

![Imágen 1](img/Eror1.PNG)

Este error se debe porque el cliente, nuestra aplicacion, cierra la conexion debido a que esta sobrecargada de peticiones. 

### tamano B2ms

#### Tiempos de ejecución de cada petición

La primera iteracion tuvo un tiempo de 53.5 segundos, las demas iteraciones que no fallaron, tuvieron un tiempo que oscilo entre 26 y 27 segundos. 
Esto concuerda con el tiempo que se tomo anteriormente con una sola peticion. 

#### Si hubo fallos documentelos y explique

Hubieron 5 peticiones que fallaron, con un resultado como este: 

![Imágen 1](img/Eror1.PNG)

Este error se debe porque el cliente, nuestra aplicacion, cierra la conexion debido a que esta sobrecargada de peticiones. 

## 7. ¿Cuál es la diferencia entre los tamaños B2ms y B1ls (no solo busque especificaciones de infraestructura)?

### B1ls 

![Imágen 1](img/B1ls.PNG)

### B2ms 

![Imágen 1](img/B2ms.PNG)

## 8. ¿Aumentar el tamaño de la VM es una buena solución en este escenario?, ¿Qué pasa con la FibonacciApp cuando cambiamos el tamaño de la VM?

Soluciona el consumo de CPU de la maquina virtual pero no mejora el tiempo de ejecucion, ya que este se mantiene casi constante. La aplicacion 
se comporta de la misma manera en cuanto a tiempo de ejecucion.

## 9. ¿Qué pasa con la infraestructura cuando cambia el tamaño de la VM? ¿Qué efectos negativos implica?

Cuando la maquina cambia de tamano, se debe reiniciar pero esto implica que los servicios se terminan y deben volver a ser iniciados o en su caso 
configurados para que inicien cuando se inicie la maquina. 

## 10. ¿Hubo mejora en el consumo de CPU o en los tiempos de respuesta? Si/No ¿Por qué?

Hubo mejora en el consumo de CPU pero no en los tiempos de respuesta, esto se debe a que con mas nucleos de procesamiento se pueden procesar mas solicitudes 
evitando el gasto de procesamiento pero la eficiencia depende de la aplicacion en si. 

## 11. Aumente la cantidad de ejecuciones paralelas del comando de postman a 4. ¿El comportamiento del sistema es porcentualmente mejor?

El porcentaje porcentual de la CPU se mantiene como se puede ver a continuacion, primero se realizo el comando con 4 ejecuciones y despues con 2 ejecuciones. 


![Imágen 1](porcentual.PNG)

# Autores 
- Diego Alejandro Corredor Tolosa https://github.com/diego2097

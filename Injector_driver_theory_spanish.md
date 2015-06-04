# introducción #

Estoy seguro de que muchas personas han visto el desairado o "rueda libre" diodos utilizados para controlar los picos inyectores de tensión. Estos diodos trabajo en situaciones básicas, pero no son óptimos, y que requieren trabajo adicional para instalar y mantener. Por el mismo precio como cualquier otro IC, se puede obtener OVP MOSFET como VNS14NV04 (SMT) o VNP28N04 (a través del agujero), que son mucho mejor que un MOSFET amortiguador de opinión y se recomiendan para la mayoría de los fines. En esta página se habla y explicar muchos aspectos de la utilización de dispositivos de OVP como conductor inductivo (s). Las simulaciones también pueden encontrarse en este enlace https://github.com/jharvey/Cinch_enclosure_template/tree/master/simulation, se hacen con Qucs, una herramienta de simulación libre.

# Diodo amortiguador de la topología #

Aquí hay una captura de pantalla de una simulación Qucs que muestra detalles de la topología de diodo amortiguador.

Lo sentimos, las imágenes son en Inglés

[http://wiki.open5xxxecu.googlecode.com/git/Snubbed\_HighZ\_injector\_simulation.PNG](http://wiki.open5xxxecu.googlecode.com/git/Snubbed_HighZ_injector_simulation.PNG)

Algunos elementos clave para la nota, el lado de baja tensión del inyector se queda en o debajo de 16V, lo cual es bastante agradable, y el campo de colapso magnético (que genera la corriente de inyección después de que se apaga) es completamente disipada por la resistencia de la bobina de la inyector. Sumándose al calor previo del combustible, lo que te empuja más cerca de los límites de la detonación. Un calco de la energía para el enfoque de IC OVP (se indica a continuación) indica el promedio de los inyectores de energía almacenada en 9kRPM es del orden de 3 vatios por inyector. Ahora ve a tocar sus tres vatios de luz de la noche, y obtener una sensación de la tripa de lo mucho que se pre-calentar el combustible. También tenga en cuenta el tiempo desde que el MOSFET se apaga el inyector, al tiempo en que el inyector se detiene la entrega de combustible es de 6 ms. El perno queda a unos amplificadores de 1.3 sorteo de un inyector de alta Z. Aviso de la tasa de desintegración en este punto, es en la parte inferior de la cara del decaimiento exponencial, lo que significa que pequeñas variaciones en la decadencia causada por variaciones en la inductancia debido a la calefacción, la corrosión en un mazo de cables, etc causará una mayor tolerancia en el control de cuando el combustible deja de fluir. En este caso no creo que + / -. 5mS es razonable esperar y + / - 0,25 mS es probable. Con un pulso de 6 ms y 0,5 ms variación del error en la entrega de combustible es de aproximadamente 4%.

Este tiempo de caída de 6 ms, y la rampa de 6 ms tiempo de preparación, simplemente significa que el encendido y apagado del inyector tomará alrededor de 12 ms. Si la entrega de una carga de combustible de 6 ms de largo una vez por vuelta, entonces su min completa el ciclo de trabajo largo, se 18ms. Lo que equivale a cerca de un máximo de 3.300 RPM entrega exacta ((1rev/.018S) x60S/min). Si usted comienza a rev más rápido que esto, el inyector o perderá la ventana de 6 ms para una dispensación exacta de combustible, causando una condición pobre. O tendrá que empezar a solapar el despegue y en las rampas, perdiendo la precisión de su cargo de entrega, que también puede dar lugar a una condición pobre. Por lo general los algoritmos simplemente volcado en más de combustible a lo seguro, como condición pobre a plena carga y RPM hace que por un corto tiempo antes de fundir las piezas del motor. Combustible Lean es malo, porque no va a tomar el calor del cilindro de distancia lo suficientemente rápido, y las causas de los fallos de cámara. Una solución es simplemente dejar de entregar combustible sobre una base por la revolución, y tratar de inyectar sobre la base de un consumo medio, no un consumo per rev. Si se ejecuta un motor de 4 tiempos, puede inyectarse en ese otro ciclo, lo que permite un pulso de combustible mucho más tiempo. Sin embargo, usted ya no se sabe exactamente qué cargo tiene en el cilindro y se están haciendo algunas suposiciones acerca de lo que en realidad lo hizo en la cámara. También tenga en cuenta, que si usted está cumpliendo con una base por revoluciones como en una carrera 2, y 12 ms para apagar y encender el inyector, en 9kRPM que podría tener apenas seca disparó dos revoluciones, o despedido lean 2 revoluciones y seco disparó un rev. Esta ventana de 12 ms no es una buena cosa. Así que echemos un vistazo a cómo ayudar a IC OVP con ese problema.

# Información general que desee de un inyector #

Por lo tanto la cantidad de energía que en un inyector? En primer lugar vamos a necesitar a la bola del parque la cantidad de inductancia que se encuentra en una típica inyector. Echa un vistazo a esta hoja spead para ver la lista que ha sido compilado a partir de varios inyectores diferentes.
https://github.com/jharvey/Cinch_enclosure_template/blob/master/docs/injector_mH_averages.xls?raw=true

Cuando esta página se creó esta hoja de cálculo señalar un máximo de un inyector de alta impedancia en 15.9ohms 32.8mH, una baja de 11.8ohms 7.9mH. Parece que será común para 12mH y 12 ohmios.

El mH más, más energía va a almacenar en un campo magnético. El ohms también es importante, ya que controla la curva de carga.

Ahora vamos a tomar un momento para recordar el tiempo que le toma a una rotación en los diferentes RPM.

12kRPM = 0.00500 segundos / rotación

9kRPM = 0.00666 segundos / rotación

6kRPM = 0.01000 segundos / rotación

3kRPM = 0.02000 segundos / rotación

Así que cuando el tamaño de su inyectores, es probable que desee algo en torno a un rev 9kRPM máximo, el pulso de flujo de carga total de hasta alrededor de 6.6mS. Esa es una cifra aproximadamente, a los efectos de hablar. No es para el dimensionamiento de los inyectores, como usted tiene que considerar muchos elementos como 2 o accidente cerebrovascular, secuencial o no 4, abrió la válvula de tiempo, etc. Para los propósitos de hablar voy a asumir el pulso 6.6mS se encuentra en el parque de pelota, sin embargo de forma no secuencial de 4 tiempos, que la entrega máxima del pulso puede ser 0.00333S. De modo que el pulso puede variar por un poco.

# Un MOSFET 70V OVP #

Así que ahora que hemos repasado algunos defectos del enfoque de diodo amortiguador, y tenemos un conjunto básico de valores que se establece un rango de tiempos de pulso, y las propiedades de inyección eléctrica, ¿cómo podemos controlarlos mejor? He aquí una aproximación.

Aquí hay una foto de un Qucs simulando el MOSFET 70V OVP la conducción de un inyector de la misma el modelo anterior. Esta simulación utiliza un inyector peor de los casos, que es un poco peor que el mayor mH se encuentran en la hoja de cálculo anterior, por lo que debe disipar más calor, y tomar más tiempo que la mayoría de los inyectores para abrir / cerrar. Se utiliza como punto de partida para mostrar las variaciones de un diseño a otro.

![![](http://wiki.open5xxxecu.googlecode.com/git/70v-ovp_highz_injector_simulation_4.2-watts.png)](http://wiki.open5xxxecu.googlecode.com/git/70v-ovp_highz_injector_simulation_4.2-watts.png)

Tenga en cuenta la decadencia de inyección es lineal, y toma alrededor de 0.0005 S al colapso. Si varios de la tensión y la corriente, usted descubrirá que el MOSFET se está disipando un promedio de 35 vatios de 0.0005 S cada vez que el inyector está pulsada. La región OVP del pulso no está determinada por la inductancia de la inyección o la resistencia, por lo que la precisión es mayor del 4% de probabilidades de bajo 0,1%. Que debe ser verdad para casi cualquier MOSFET OVP. Una cosa negativa es que 70 V puede exceder las especificaciones de los inyectores, o se puede producir tensiones internas, ya que estos están diseñados como dispositivos de 12V. Por lo que es el rendimiento general es mucho mejor, pero la alta tensión puede ser un problema, y que técnicamente viola el código eléctrico típico. Cuando por debajo de aprox 50V mayoría de los códigos eléctricos como NFPA70 consideramos que es un voltaje seguro. Por lo tanto una tensión más baja sería bueno para reducir el estrés del inyector, y aumentar la seguridad un poco.

# El MOSFET de 40V OVP #

Introducir el MOSFET 40V OVP. Aquí hay una foto de la simulación de la misma Qucs **40V** IC OVP conducir una simulación del inyector.

![![](http://wiki.open5xxxecu.googlecode.com/git/40v-ovp_highz_injector_simulation_4-watts.png)](http://wiki.open5xxxecu.googlecode.com/git/40v-ovp_highz_injector_simulation_4-watts.png)

Tenga en cuenta este deterioro inyector es lineal, y toma alrededor de 0.001 S a derrumbarse. Si varios de la tensión y la corriente, usted descubrirá que el MOSFET se está disipando un promedio de 20 vatios para .001 S cada vez que el inyector está pulsada. Es constante en 40V, y se inicia el curso en 1A y en forma lineal se reduce a 0A. Así que un promedio de 0,5 A. Por lo tanto, 40V x 0,5 A = 20W de calor OVP. Una vez más la precisión en el rango de 0,1%.

Vamos a ver esto de otra manera que vuelva a comprobar que la decadencia parece correcto. A partir de aquí http://en.wikipedia.org/wiki/Inductor # Stored\_energy tenemos 0,5 (0.035 H) (1 A ^ 2) = 0.0175 j. Entonces, desde aquí http://en.wikipedia.org/wiki/Watt # Definición y suponiendo que se disipan en una constante de 20 vatios, el tiempo de caída sería 0.0175 j / 20W = 0,0008 S. ¿Qué es lo que esperaba y ver en la simulación.

En 9kRPM que decir que si los inyectores están inyectando en una base por rev, que giraría / pulso 150 veces en un segundo. Por lo que sería en decadencia OVP para 150cycle/second x .001seconds/cycle =. 15 segundos por un segundo de la rotación. Así que el 15% del calor se disipa OVP en promedio. 15% de 20W es de 3 vatios de calor totales causados ​​por la desintegración MOSFET OVP por inyector de canal. Por Fred notas en los foros DIYEFI, no es descabellado ver 257 inyecciones por segundo, que se convierte en un 25,7% en el tiempo, que es 5,14 vatios por canal de inyección.

El OVP MOSFET 40V se señaló anteriormente, los movimientos que 6ms reducido a menos de 1 ms, y el OVP 70V mueve el tiempo libre a 0,5 ms, siendo el diodo amortiguador fue de 6 ms. Esto cambia el ciclo completo servicio de longitud de 18ms a 13ms bajo, y los cambios de su RPM máxima de entrega precisa de alrededor de 4.600 RPM (el OVP 70V está a 4800rpm). Esto aumenta la precisión de su carga de combustible entregado. Esta mayor precisión significa que usted no pierda de combustible que, no te arriesgues a fallos del sistema que causa oculta las condiciones de inclinación, y usted no tiene que adivinar lo mismo cuando a plena carga y RPM completa.

La compensación, puede manejar su inyector de 40 o 70 V potenciales? No he oído hablar de un embargo que no se puede. Tan seguro de que puede manejar sin romper con inmediatamente. Sin embargo, con mi falta de fiabilidad de los datos a largo plazo, me gustaría recomendar un 40V en lugar de 70V para aumentar la fiabilidad a largo plazo. A 70V es, probablemente, muy bien, pero no tengo datos empíricos que lo respalde con. Si usted consigue la precisión de un OVP 40V, eso es lo que realmente importa, y la 200RPM adicional de un control preciso es una especie de menor de edad, cuando muchos se basa el éxito se puede hacer con diodos amortiguador.

El circuito MOSFET OVP disipa el campo juntando actual en la ECU no, el inyector, lo que reduce el combustible de calefacción antes de los problemas causados ​​con el enfoque de amortiguador.

¿Entonces por qué la nota hoja de datos unidos y NS para el MOSFET y apagado, cuando medimos mS en el dispositivo real? Eso es porque cuando el inyector empuja el voltaje de hasta 40V (o lo que **O** ver **V** oltage **P** rotección (OVP) está programada para) El OVP se escape un poco de voltaje a la puerta, convirtiéndolo en algo, haciendo que el MOSFET de mirar como una resistencia, no un circuito abierto o cortocircuito. Esta resistencia temporal variará, haciendo que la corriente de la corrupción de forma lineal en lugar de un decaimiento exponencial. El límite de OVP se establecerá la tarifa de la decadencia, no el tiempo de conmutación de los MOSFET. Un tiempo de conmutación rápida disminución de los retrasos de la señal y demás. Ton los MOSFET y los tiempos de Toff siguen siendo importantes, ya que determina el sistema de demoras cuando el inyector se le ordena que cuando reacciona. Los dispositivos de forma más rápida son preferibles.

¿Por qué no usar un IGBT, después de todo lo que tienen una tensión de aislamiento más alto? No he visto un IGBT con resistencias bajas como un MOSFET. Esto aumentará la resistencia disminuye la cantidad de energía que el IGBT puede disipar, y disminuye los tiempos de apagado, se acerca el diodo amortiguador en términos de tiempos de retardo. Parece un lugar común para que los IGBT para dejar caer sobre 1V a 5 amperios. Que se convierte en una carga de calor en estado estacionario resistencia de 5 vatios, frente a la IC antes mencionada está en 0.035 vatios. Mientras que el OVP IGBT tendrá tolerancias similares en la decadencia, las resistencias internas se reducirá el tiempo de subida, que se extiende a que el ascenso de 6 ms, y limitar el deterioro, de tal manera que el MOSFET 1mS sería más como 1.1mS en un IGBT.

Así que, en general, MOSFET OVP son más rápidos, como un sistema global, que eliminar el calor del inyector, y en la ECU, y ofrecen menor resistencia características de calor, disminuyendo las preocupaciones de gestión térmica.
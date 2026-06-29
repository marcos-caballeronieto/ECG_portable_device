### 1. El Origen del Dipolo Cardíaco

Físicamente, un **dipolo eléctrico** es un sistema compuesto por dos cargas eléctricas de igual magnitud pero de signo opuesto ($+q$ y $-q$), separadas por una distancia microscópica. 

¿Por qué el corazón se comporta así? En estado de reposo, las células musculares del corazón (miocitos) están polarizadas: su interior es negativo respecto al exterior. Cuando se inicia un latido, ocurre la **despolarización**: los canales iónicos se abren, el sodio entra y el exterior de la célula se vuelve transitoriamente negativo.

Este proceso no ocurre en todo el corazón al mismo tiempo, sino que viaja como una ola. En el límite exacto donde avanza esta ola (el frente de onda de despolarización), tenemos tejido en reposo (exterior positivo) inmediatamente adyacente a tejido despolarizado (exterior negativo). 

> **Analogía:** Imagina "la ola" en un estadio de fútbol. Las personas sentadas son el tejido en reposo y las personas de pie con los brazos alzados son el tejido despolarizado. La línea exacta donde las personas se están levantando es el frente de onda. 

Ese frente de onda actúa, a efectos electromagnéticos, como una lámina continua de cargas positivas y negativas apareadas; es decir, una multitud de micro-dipolos que avanzan juntos.

### 2. El Vector Cardíaco (Momento Dipolar)

En física, la fuerza y orientación de un dipolo se mide mediante una magnitud vectorial llamada **momento dipolar** ($\vec{p}$). Como el frente de onda cardíaco está formado por millones de micro-dipolos celulares, estos se suman espacialmente para formar un único dipolo resultante (el macro-dipolo cardíaco).

Este macro-dipolo genera un vector tridimensional que posee:
* **Magnitud:** Directamente proporcional a la cantidad de masa muscular que se está despolarizando en ese milisegundo (por eso el vector ventricular es mucho mayor que el auricular).
* **Dirección y Sentido:** Apunta siempre desde la zona despolarizada (negativa) hacia la zona en reposo (positiva).

Dado que la onda de activación viaja a través de una anatomía compleja (nódulo sinusal, aurículas, tabique interventricular, ventrículos), este vector resultante es extremadamente dinámico. Su origen se ancla en el centro eléctrico del corazón, pero la punta de la flecha crece, se encoge, gira y cambia de dirección constantemente durante el ciclo cardíaco (dibujando en el espacio 3D una figura en forma de lazo conocida como *vectocardiograma*).

### 3. El Medio Conductor (Conductor Volumétrico)

El corazón no flota en el vacío. Está sumergido en el torso humano, el cual está lleno de sangre, líquido intersticial y tejidos ricos en iones. Desde el punto de vista biofísico, el torso actúa como un **conductor volumétrico tridimensional**.

Las cargas eléctricas del dipolo cardíaco generan un campo eléctrico que se propaga instantáneamente a través de este volumen hasta llegar a la piel. Aunque la realidad es que el torso es "inhomogéneo" (los pulmones conducen peor que la sangre) y "anisotrópico" (el músculo esquelético conduce mejor en unas direcciones que en otras), para la teoría clásica del ECG se modela como un medio homogéneo e isotrópico. 

Este medio conductor actúa como el "cableado" natural que permite que la actividad eléctrica del centro del pecho se disipe y pueda ser detectada a varios centímetros de distancia.

### 4. Proyección y Medición (La conexión con el ECG)

Aquí es donde la magia vectorial se convierte en la clínica médica. Los electrodos que pegamos en la piel (brazos, piernas, pecho) no miden el vector 3D directamente. Miden la **diferencia de potencial** entre dos puntos. 

Una derivación del ECG (por ejemplo, la Derivación I, que va del brazo derecho al brazo izquierdo) actúa como un eje unidimensional. Lo que dibuja la aguja del ECG es la **proyección matemática (producto escalar)** del vector cardíaco 3D sobre el eje de esa derivación específica.

> **Analogía espacial:** Imagina que el vector cardíaco es un avión haciendo acrobacias (subiendo, bajando, girando) y los electrodos son linternas que proyectan la sombra de ese avión contra la pared. El ECG solo dibuja la longitud de la sombra a lo largo del tiempo. 

* Si el vector cardíaco apunta *directamente* hacia el polo positivo del electrodo, el ECG registra una onda positiva máxima.
* Si el vector apunta *en dirección opuesta*, el ECG registra una onda negativa profunda.
* Si el vector viaja de forma *perpendicular* a la línea entre los electrodos, el ECG no registra nada (línea isoeléctrica), porque la "sombra" proyectada sobre ese eje es cero.

### 5. Fundamento Matemático

Para formalizar esto, utilizamos las ecuaciones del electromagnetismo para un medio conductor. El potencial eléctrico absoluto $\phi$ que genera nuestro dipolo cardíaco en un punto $P$ (por ejemplo, un electrodo en la piel) dentro de un conductor volumétrico infinito y homogéneo se describe así:

$$\phi = \frac{1}{4\pi\sigma} \frac{\vec{p} \cdot \hat{r}}{r^2}$$

Donde:
* $\phi$ es el potencial eléctrico medido en el punto de observación.
* $\sigma$ es la conductividad eléctrica del medio (el torso).
* $\vec{p}$ es el vector momento dipolar del corazón en un instante $t$.
* $r$ es la distancia escalar desde el centro del dipolo hasta el punto $P$.
* $\hat{r}$ es el vector unitario direccional que apunta desde el dipolo hacia el punto $P$.

Dado que el producto escalar $\vec{p} \cdot \hat{r}$ es igual a $p \cos(\theta)$, podemos reescribir la ecuación de la siguiente manera:

$$\phi = \frac{1}{4\pi\sigma} \frac{p \cos(\theta)}{r^2}$$

Donde $\theta$ es el ángulo entre la dirección de propagación del vector cardíaco y la línea de visión del electrodo. Esta simple función coseno es la razón matemática exacta por la cual los electrodos ven ondas positivas o negativas dependiendo del ángulo con el que la "flecha eléctrica" se acerca o se aleja de ellos. En la práctica clínica, la diferencia de potencial $V$ medida por una derivación (con su propio vector director $\vec{L}$) se simplifica frecuentemente mediante la ecuación:

$$V = \vec{p} \cdot \vec{L}$$

Esta es la ecuación fundamental de la electrocardiografía vectorial. Cada deflexión que ves en un trazado de papel milimetrado es simplemente la resolución geométrica de esta fórmula a lo largo del tiempo.
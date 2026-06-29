## 1. Introducción: Electrofisiología cardíaca y su relación con el ECG

La **electrofisiología cardíaca** es la rama de la fisiología que estudia los procesos bioeléctricos responsables de la generación, propagación y modulación de los impulsos eléctricos en el corazón. Este sistema eléctrico intrínseco es el que dicta el ritmo cardíaco y coordina la contracción mecánica (acoplamiento excitación-contracción) necesaria para bombear la sangre.

Desde la perspectiva de la ingeniería biomédica, comprender esta disciplina es fundamental porque representa la **fuente de la señal** de un electrocardiógrafo. El electrocardiograma (ECG) no mide la contracción muscular en sí, sino los cambios de voltaje extracelulares que resultan de los flujos iónicos a través de las membranas celulares del miocardio. Para diseñar los filtros, amplificadores y algoritmos de un ECG portátil, primero se debe comprender el origen bioeléctrico y las frecuencias asociadas a estos eventos celulares.

---

## 2. El Potencial de Reposo Transmembrana

En estado de reposo, la célula miocárdica está polarizada eléctricamente. Existe una diferencia de potencial eléctrico entre el medio intracelular y el extracelular, conocida como **potencial de reposo transmembrana**, que en los miocitos ventriculares es de aproximadamente **-90 mV** (el interior es negativo respecto al exterior).

Este estado se mantiene gracias a dos factores principales:
* **Permeabilidad selectiva:** La membrana celular en reposo es muy permeable al potasio ($K^+$) y casi impermeable al sodio ($Na^+$) y al calcio ($Ca^{2+}$).
* **La bomba $Na^+/K^+$ ATPasa:** Es una proteína transmembrana electrogénica activa. Utiliza energía (ATP) para expulsar **3 iones de $Na^+$** hacia el espacio extracelular y bombear **2 iones de $K^+$** hacia el espacio intracelular en contra de sus gradientes de concentración. Al sacar más cargas positivas de las que introduce, contribuye directamente a mantener la electronegatividad neta en el interior de la célula.

---

## 3. Tipos de Células Cardíacas

A nivel electrofisiológico, el corazón posee dos poblaciones celulares distintas con perfiles de potencial de acción diferentes:

* **Células marcapasos (Autorrítmicas o Nodales):** Localizadas principalmente en el nódulo sinoauricular (SA) y el nódulo auriculoventricular (AV). Su característica principal es el **automatismo**: no tienen un potencial de reposo estable. Sufren una despolarización diastólica espontánea (fase 4 inestable) que les permite generar sus propios impulsos eléctricos rítmicamente. Sus potenciales de acción son de "respuesta lenta" (dependientes de canales de $Ca^{2+}$ lentos).
* **Células contráctiles (Miocitos de trabajo):** Componen la inmensa mayoría del tejido auricular y ventricular. Son responsables de la generación de fuerza (contracción). Tienen un potencial de reposo estable y requieren un estímulo eléctrico de una célula vecina para despolarizarse. Generan potenciales de acción de "respuesta rápida" caracterizados por una entrada masiva y abrupta de $Na^+$.

---

## 4. Fases del Potencial de Acción de Respuesta Rápida (Células contráctiles)

Cuando un miocito de trabajo recibe un estímulo que eleva su voltaje por encima del umbral (aprox. -70 mV), se desencadena un **potencial de acción**. Este proceso se divide en cinco fases clínicas marcadas por flujos iónicos específicos.

### Fase 0: Despolarización rápida
* **Mecanismo iónico:** El estímulo provoca la apertura abrupta de los **canales rápidos de $Na^+$** dependientes de voltaje.
* **Resultado:** Se produce una entrada masiva y en "avalancha" de $Na^+$ al interior celular a favor de su gradiente electroquímico. El potencial transmembrana se invierte rápidamente, pasando de -90 mV a aproximadamente **+20 o +30 mV**.

### Fase 1: Repolarización inicial y temprana
* **Mecanismo iónico:** Los canales rápidos de $Na^+$ se inactivan rápidamente (cierran). Al mismo tiempo, se abren canales transitorios de $K^+$ (corriente $I_{to}$).
* **Resultado:** Ocurre una breve y pequeña salida de $K^+$ hacia el exterior, lo que provoca una ligera caída del potencial de acción, creando una "muesca" (notch) en la gráfica del potencial, situando el voltaje cerca de los 0 mV.

### Fase 2: Fase de meseta (Plateau)
* **Mecanismo iónico:** Esta fase es el equilibrio entre dos corrientes opuestas. Por un lado, se abren los **canales lentos de $Ca^{2+}$** (tipo L), permitiendo la entrada de calcio a la célula. Por otro lado, continúa la salida lenta de $K^+$.
* **Importancia crucial para el músculo cardíaco:** 
1.  **Mecánica:** La entrada de $Ca^{2+}$ extracelular desencadena la liberación de más calcio desde el retículo sarcoplásmico (liberación de calcio inducida por calcio), lo cual es el paso fundamental para iniciar la contracción muscular.
2.  **Protección:** La meseta prolonga la duración del potencial de acción (y por tanto el período refractario absoluto) a unos 200-300 ms. Esto evita que los impulsos eléctricos rápidos se sumen, **imposibilitando la contracción tetánica** (un espasmo sostenido) del corazón. Garantiza que el ventrículo tenga tiempo para contraerse y relajarse por completo, permitiendo el llenado de sangre para el siguiente latido.

### Fase 3: Repolarización rápida final
* **Mecanismo iónico:** Los canales lentos de $Ca^{2+}$ se cierran progresivamente, deteniendo el flujo de cargas positivas hacia adentro. Simultáneamente, los canales de $K^+$ (rectificadores tardíos) se abren por completo.
* **Resultado:** Hay una salida masiva de $K^+$ hacia el exterior celular, lo que restaura rápidamente la electronegatividad en el interior. El potencial de membrana cae en picado de vuelta a sus valores negativos.

### Fase 4: Potencial de reposo
* **Mecanismo iónico:** La célula ha recuperado su voltaje de reposo (-90 mV), pero la concentración de iones está invertida (hay exceso de $Na^+$ dentro y de $K^+$ fuera).
* **Resultado:** La **bomba $Na^+/K^+$ ATPasa** entra a trabajar a máxima capacidad para restablecer los gradientes iónicos originales, mientras que los canales rectificadores de entrada de potasio ($I_{K1}$) mantienen estabilizado el potencial de membrana a la espera del siguiente estímulo.

---

## 5. Propagación del Impulso: El Sincitio Funcional

Para que el corazón actúe como una bomba eficiente, el impulso eléctrico debe viajar de manera rápida y ordenada. La electricidad no se transfiere por nervios entre las células musculares, sino que pasa de miocito a miocito gracias a su estructura anatómica.

* **Discos Intercalares:** Las células musculares cardíacas están conectadas en sus extremos por estructuras especializadas llamadas discos intercalares.
* **Uniones Gap (Uniones en hendidura o comunicantes):** Dentro de estos discos existen canales proteicos (conexones) de muy baja resistencia eléctrica que conectan directamente los citoplasmas de células adyacentes.
* **Sincitio Funcional:** Gracias a las uniones gap, cuando una célula se despolariza (Fase 0), los iones de $Na^+$ y $Ca^{2+}$ fluyen libremente a través de los canales hacia la célula vecina, llevándola a su umbral de disparo. Esto hace que el tejido cardíaco se comporte bioeléctricamente como si fuera una única célula gigante continua (un sincitio). El corazón está dividido en dos sincitios funcionales separados (auricular y ventricular), aislados eléctricamente por el esqueleto fibroso del corazón y conectados solo por el Haz de His.

---

## 6. Conexión con el ECG: Del nivel celular al nivel macroscópico

La actividad eléctrica de una sola célula (unos pocos milivoltios) es indetectable desde la superficie corporal. Sin embargo, debido al comportamiento de **sincitio funcional**, millones de células cardíacas se despolarizan y repolarizan de manera sincronizada y en una secuencia geométrica bien definida (nódulo SA $\rightarrow$ aurículas $\rightarrow$ nódulo AV $\rightarrow$ ventrículos). Este movimiento masivo de frentes de onda iónicos genera un **dipolo eléctrico macroscópico** cambiante en el tiempo, cuyo campo electromagnético irradia a través de los fluidos corporales (que son excelentes conductores) hasta llegar a la piel. Los electrodos del dispositivo ECG que vas a construir capturan, amplifican y grafican la suma espacial y temporal de estos millones de potenciales de acción, traduciendo la Fase 0 auricular en la onda P, la Fase 0 ventricular masiva en el complejo QRS, y la Fase 3 ventricular en la onda T.
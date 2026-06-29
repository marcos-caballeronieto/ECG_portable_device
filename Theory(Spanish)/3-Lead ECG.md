## Análisis Diferencial: Sistema ECG Portátil de 3 Derivaciones vs. Estándar Clínico de 12 Derivaciones

### 1. Proyección Espacial y Análisis Vectorial

La diferencia matemática central radica en la dimensionalidad del vector dipolo cardíaco capturado.

* **Matriz de Proyección y el Triángulo de Einthoven:** En un sistema de 3 derivaciones, te limitas estrictamente al plano frontal bidimensional. Matemáticamente, mides las diferencias de potencial bipolares: $I = \Phi_L - \Phi_R$, $II = \Phi_F - \Phi_R$ y $III = \Phi_F - \Phi_L$. Por la Ley de Kirchhoff (Ley de Einthoven: $II = I + III$), solo obtienes **dos grados de libertad espacial**. Toda la actividad eléctrica se colapsa en este único plano bidimensional.
* **Pérdida del Terminal Central de Wilson (WCT):** El sistema de 12 derivaciones construye un nodo de referencia virtual, el WCT, definido como $\Phi_{WCT} = \frac{\Phi_R + \Phi_L + \Phi_F}{3}$. Al carecer de esta referencia estable y cercana a cero (el centro eléctrico teórico del corazón), el sistema de 3 derivaciones no puede registrar potenciales unipolares verdaderos.
* **Pérdida del Eje Z (Transversal):** La ausencia de las derivaciones precordiales (V1-V6) significa una pérdida absoluta de la proyección en el plano horizontal (eje anteroposterior). Matemáticamente, el vector de información tridimensional $\vec{V}(x,y,z)$ se trunca a $\vec{V}(x,y)$. Cualquier vector de fuerza electromotriz ortogonal al plano frontal tiene una proyección nula en las derivaciones I, II y III, volviéndose invisible al hardware.

---

### 2. Hardware, AFE (Front-End Analógico) y Sensores

La reducción física de electrodos altera la arquitectura del AFE y el comportamiento de la cadena de adquisición frente al ruido.

* **Arquitectura de Adquisición y Conversión:** Un sistema de 12 derivaciones (10 electrodos) requiere una red de resistencias de precisión (red WCT) y típicamente 8 canales ADC paralelos simultáneos de 24 bits (I, II y V1-V6 son independientes; el resto se calcula). Un AFE para 3 derivaciones (3 o 4 electrodos) simplifica drásticamente el silicio: requiere solo 2 canales ADC independientes (capturando I y II, derivando III digitalmente). Esto reduce el consumo de potencia de miliwatios a microwatios, fundamental para baterías portátiles.
* **Retos Críticos en la Pierna Derecha Dirigida (DRL):** * *Sistema de 12 derivaciones:* El DRL extrae el voltaje de modo común ($V_{cm}$) de la red WCT (múltiples puntos de contacto, gran masa corporal) y lo invierte con alta ganancia, proporcionando un excelente factor de rechazo en modo común (CMRR > 100 dB).
    * *Sistema portátil:* La extracción del $V_{cm}$ se basa en solo 2 nodos activos. Además, los electrodos suelen estar físicamente más juntos (menor base espacial). Si la impedancia de contacto de los electrodos varía asimétricamente (común en portables), la ganancia del lazo DRL puede volverse inestable (oscilar) o saturar el amplificador, reduciendo el CMRR efectivo drásticamente a < 70 dB en condiciones dinámicas.
* **Susceptibilidad al Ruido y Procesamiento de Señal (DSP):**
    * *Impedancia de contacto:* Los portátiles suelen usar electrodos secos o poliméricos, resultando en impedancias de contacto en la interfaz piel-electrodo que oscilan entre $100\ k\Omega$ y $1\ M\Omega$, frente a los $< 5\ k\Omega$ de los electrodos de Ag/AgCl húmedos clínicos. Este desajuste de impedancia ($Z_{mismatch}$) convierte el ruido de modo común directamente en ruido diferencial.
    * *Artefactos de movimiento:* La variación del potencial de semicelda por deformación mecánica del estrato córneo es severa. Esto obliga a tu diseño portátil a implementar un DSP agresivo: filtros paso alto con frecuencias de corte mayores (ej. 0.5 Hz a 0.67 Hz) comparados con los 0.05 Hz del estándar de diagnóstico clínico. **Consecuencia técnica:** Esta fase de filtrado no lineal introduce distorsión de fase en las frecuencias bajas, deformando artificialmente el segmento ST.

---

### 3. Limitaciones Diagnósticas (El Delta Clínico)

El hardware define los límites de lo que el algoritmo puede diagnosticar.

#### Patologías detectables con alta fiabilidad (Conservadas)
* **Análisis de ritmo y frecuencias:** Taquicardia y bradicardia sinusal.
* **Arritmias supraventriculares:** Fibrilación Auricular (FA), aleteo auricular (Flutter).
* **Arritmias ventriculares:** Extrasístoles ventriculares (PVC), Taquicardia Ventricular (TV), Fibrilación Ventricular (FV).
* **Trastornos de conducción AV:** Bloqueos auriculoventriculares de 1º, 2º (Mobitz I y II) y 3º grado.
* **Variabilidad de la Frecuencia Cardíaca (HRV):** Mediciones temporales de los intervalos R-R.

#### Diagnósticos técnica y médicamente imposibles o poco fiables (Perdidos)
* **Localización de Isquemia e Infarto Agudo de Miocardio (IAM):** Imposible detectar infartos en la pared anterior, septal, lateral o posterior. La elevación del segmento ST (STEMI) en estas regiones requiere el vector transversal (V1-V6). Aunque detectes isquemia inferior en II y III, el estándar clínico exige la confirmación regional cruzada que solo brindan las 12 derivaciones para evitar falsos positivos por la distorsión del filtro paso alto antes mencionada.
* **Bloqueos de Rama (Tóxicos espacialmente):** Diferenciar entre un Bloqueo de Rama Izquierda (BRI) y un Bloqueo de Rama Derecha (BRD) depende críticamente de la morfología del complejo QRS en V1 y V6 (ej., morfología rSR' o "orejas de conejo" en V1 para BRD). El plano frontal no proporciona esta diferenciación de fase.
* **Hipertrofias Ventriculares:** La Hipertrofia Ventricular Izquierda (HVI) se diagnostica usando criterios de amplitud espacial dependientes de voltaje, como el índice de Sokolow-Lyon ($S_{V1} + R_{V5/V6} \ge 35\ mm$). La ausencia del terminal central de Wilson y las derivadas precordiales anula este cálculo.
* **Progresión de la Onda R:** La evaluación de la rotación del corazón sobre su eje longitudinal (progresión de R de V1 a V6) está matemáticamente fuera del alcance del dispositivo.
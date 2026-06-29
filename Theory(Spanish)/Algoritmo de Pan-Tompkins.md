## 1. Fundamentos Teóricos

El algoritmo de Pan-Tompkins, desarrollado en 1985, sigue siendo el estándar de oro de la industria para la detección del complejo QRS en señales de ECG. Su perdurabilidad en dispositivos médicos biomédicos se debe a su excepcional eficiencia: ofrece una altísima sensibilidad y especificidad computando únicamente operaciones aritméticas básicas. Esto lo hace ideal para sistemas embebidos de bajos recursos, como microcontroladores STM32, ESP32 o la familia AVR de Arduino, donde un análisis de Fourier o redes neuronales agotarían la batería y la CPU.

El algoritmo está diseñado para aislar la energía específica del QRS superando los siguientes desafíos morfológicos del ECG:
* Deriva de la línea base: El movimiento respiratorio del paciente causa fluctuaciones lentas en la señal.
* Ruido muscular (EMG): Los músculos del paciente generan interferencia eléctrica de alta frecuencia.
* Ondas P y T altas: Variaciones fisiológicas que pueden ser tan amplias como el pico R y provocar falsas detecciones.
* Interferencia de red: Ruido eléctrico a 50 Hz o 60 Hz inducido por el entorno.

## 2. El Flujo de Procesamiento de la Señal

Asumiendo que la señal ya ha pasado por un filtro pasabanda (que elimina la deriva de línea base y el ruido de alta frecuencia), el núcleo de este algoritmo procesa la señal en tres etapas secuenciales:

* Fase de Derivación: Calcula la pendiente (la tasa de cambio) de la señal. Como el complejo QRS representa la despolarización ventricular rápida, tiene las pendientes más abruptas del ciclo cardíaco. Esta etapa resalta el QRS mientras suprime las ondas P y T, que son más lentas y de menor frecuencia.
* Función de Cuadratura: Eleva la señal derivada al cuadrado punto por punto. Esto logra dos objetivos: primero, convierte todos los puntos de datos a valores positivos; segundo, actúa como un amplificador no lineal. Las altas frecuencias (el QRS) se amplifican exponencialmente, mientras que el ruido de fondo pequeño y las ondas remanentes se atenúan severamente.
* Integración de Ventana Móvil: Actúa como un filtro de suavizado o acumulador de energía. Al integrar (sumar) los valores en una ventana de tiempo específica (típicamente de 150 ms, el ancho máximo de un QRS normal), fusiona los múltiples picos dentados generados por la cuadratura en un único pulso en forma de campana. Esto asegura que obtengamos un solo pico detectado por cada latido real.

## 3. Matemáticas Subyacentes

Para implementar estos conceptos matemáticos en un microcontrolador que procesa datos muestreados discretamente, usamos ecuaciones de diferencia finita.

Filtro Derivador (Aproximación ideal de 5 puntos para evitar la amplificación excesiva de ruido):
$$y[n] = \frac{1}{8} \left( -x[n-2] - 2x[n-1] + 2x[n+1] + x[n+2] \right)$$

Función de Cuadratura:
$$y[n] = (x[n])^2$$

Integrador de Ventana Móvil:
$$y[n] = \frac{1}{N} \sum_{i=0}^{N-1} x[n-i]$$
(Donde $N$ es el tamaño de la ventana en muestras, calibrado según la frecuencia de muestreo $F_s$ para abarcar aproximadamente 150 milisegundos).

## 4. Implementación Práctica (Código)

El siguiente código en C/C++ está estructurado para ejecutarse dentro de la interrupción del ADC (o en un bucle principal procesando un buffer circular). Se utiliza una ventana deslizante optimizada ($O(1)$) para no sumar todo el arreglo en cada iteración.

``` c

    #include <stdint.h>

    // Parámetros de configuración (Ejemplo asumiendo una Fs = 200 Hz)
    #define WINDOW_SIZE 30 // 30 muestras a 200 Hz = 150 ms

    // Variables de estado del sistema estáticas/globales
    float deriv_buffer[5] = {0.0f};
    float window_buffer[WINDOW_SIZE] = {0.0f};
    int window_index = 0;
    float window_sum = 0.0f;

    // Función principal a llamar cada vez que se lee una nueva muestra del ADC
    // ecg_sample debe estar ya filtrada por el filtro pasabanda
    float process_pan_tompkins_core(float ecg_sample) {
        
        // 1. FASE DE DERIVACIÓN: Actualizar buffer (Shift)
        for(int i = 0; i < 4; i++) {
            deriv_buffer[i] = deriv_buffer[i+1];
        }
        deriv_buffer[4] = ecg_sample;

        // Calcular derivada de 5 puntos
        float derivative = (1.0f / 8.0f) * (-deriv_buffer[0] - 2.0f * deriv_buffer[1] + 
                                            2.0f * deriv_buffer[3] + deriv_buffer[4]);

        // 2. FUNCIÓN DE CUADRATURA
        float squared = derivative * derivative;

        // 3. INTEGRACIÓN DE VENTANA MÓVIL
        // Restar el valor más antiguo de la ventana y añadir el nuevo (cuadrado)
        float old_sample = window_buffer[window_index];
        window_buffer[window_index] = squared;
        
        window_sum = window_sum - old_sample + squared;
        
        // Actualizar el índice circular del buffer
        window_index = (window_index + 1) % WINDOW_SIZE;

        // Retornar la energía del pulso suavizado
        return window_sum / (float)WINDOW_SIZE;
    }
```

## 5. Umbrales Adaptativos (Adaptive Thresholding)

Una vez que procesamos la señal con el código anterior, obtenemos una serie de "montículos" suaves. Para determinar si un montículo es realmente un latido cardíaco, empleamos umbrales adaptativos. No se puede usar un umbral estático porque la amplitud de la señal de ECG cambia continuamente por el movimiento del paciente o la degradación del hidrogel del electrodo.

La lógica de decisión de Pan-Tompkins opera de la siguiente manera:
* Seguimiento de Picos: El sistema detecta continuamente picos locales en la señal integrada (cuando la pendiente cambia de positiva a negativa).
* Clasificación Señal/Ruido: El algoritmo mantiene un seguimiento del nivel de señal estimado (SPK - Signal Peak) y del nivel de ruido estimado (NPK - Noise Peak).
* Cálculo del Umbral Dinámico: El umbral para decidir si un pico es un QRS se calcula dinámicamente, ubicándose generalmente entre el nivel de ruido y el de señal. Por ejemplo: $$Threshold = NPK + 0.25 \times (SPK - NPK)$$
* Actualización Continua: Si un pico local supera el umbral, se marca como un pico R verdadero y se actualiza el valor de $SPK$. Si el pico cae por debajo del umbral, se considera ruido y se usa para actualizar el $NPK$.
* Período Refractario Blanking: Después de detectar un QRS válido, el algoritmo impone una ventana de "ceguera" temporal (típicamente 200 ms) donde ignora cualquier nuevo pico. Es anatómicamente imposible que ocurra otro latido tan rápido, lo que ayuda a evitar clasificar ondas T muy altas (que ocurren justo después del QRS) como nuevos latidos.
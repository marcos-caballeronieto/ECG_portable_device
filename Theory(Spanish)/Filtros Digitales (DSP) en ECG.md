## 1. Fundamentos Teóricos del DSP en el ECG

Los filtros analógicos son excelentes para el acondicionamiento inmediato de la señal previo a la amplificación, como el antialiasing y la eliminación de ruido a gran escala. Sin embargo, depender únicamente del hardware analógico para un Electrocardiograma (ECG) es insuficiente. Los componentes analógicos sufren de tolerancias físicas, deriva térmica y envejecimiento, lo que puede desplazar las frecuencias de corte. El Procesamiento Digital de Señales (DSP) nos permite realizar una limpieza fina con una precisión matemática que es perfectamente reproducible en millones de microcontroladores (como el ESP32 o Arduino) sin variación de componentes. Los filtros digitales pueden lograr fácilmente cortes abruptos para eliminar la interferencia de la red eléctrica de 50/60 Hz o corregir la desviación de la línea base sin el volumen físico de los circuitos analógicos de alto orden.

Un filtro digital fundamental utilizado en el DSP biomédico es el filtro de Media Móvil (Moving Average). Funciona tomando un número fijo de muestras recientes, sumándolas y dividiendo por ese número para encontrar la media. A medida que llegan nuevas muestras, la "ventana" avanza. Este tipo de filtro actúa como un filtro paso bajo y es altamente eficaz para suavizar el ruido aleatorio de alta frecuencia, como los artefactos musculares electromiográficos (EMG) y el ruido térmico aleatorio del convertidor analógico-digital (ADC), produciendo una línea base mucho más limpia.

## 2. Filtros FIR vs. IIR

Al diseñar filtros digitales más complejos para ECG, elegimos principalmente entre dos arquitecturas: Respuesta al Impulso Finita (FIR) y Respuesta al Impulso Infinita (IIR).

Un filtro FIR calcula su salida basándose completamente en un número finito de muestras de entrada actuales y pasadas. Dado que no utiliza retroalimentación, su respuesta a un impulso eventualmente se reduce a cero.

Un filtro IIR calcula su salida utilizando tanto las entradas pasadas como las salidas pasadas. Este bucle de retroalimentación significa que la respuesta del filtro a un solo impulso puede, teóricamente, continuar de forma infinita.

| Característica | FIR (Respuesta al Impulso Finita) | IIR (Respuesta al Impulso Infinita) |
| :--- | :--- | :--- |
| **Estabilidad** | Siempre estable (sin bucle de retroalimentación). | Puede volverse inestable si está mal diseñado. |
| **Distorsión de Fase** | Fase lineal (preserva la morfología exacta del QRS). | Fase no lineal (puede distorsionar los picos y la sincronización del QRS). |
| **Coste Computacional** | Alto (requiere más memoria y ciclos de CPU). | Bajo (se necesitan menos operaciones por muestra). |
| **Orden del Filtro Requerido** | Se necesita un orden alto para cortes abruptos. | Se necesita un orden bajo para la misma nitidez de corte. |

Para aplicaciones de ECG de diagnóstico donde es fundamental preservar la forma exacta del complejo QRS y la sincronización del segmento ST, los filtros FIR son generalmente preferidos debido a su respuesta de fase lineal. Sin embargo, para un sistema embebido con recursos limitados donde solo se necesita una monitorización básica de la frecuencia cardíaca, se podría utilizar un filtro IIR para ahorrar ciclos de CPU.

## 3. Matemáticas Subyacentes

El comportamiento de estos filtros en el dominio del tiempo discreto se define mediante sus ecuaciones en diferencias.

Para un filtro FIR, la salida $y[n]$ es la convolución de la señal de entrada $x[n]$ con los coeficientes del filtro $b_k$:

$$y[n] = \sum_{k=0}^{M} b_k x[n-k]$$

Para un filtro IIR, la salida $y[n]$ depende de los coeficientes de entrada $b_k$ así como de los coeficientes de retroalimentación $a_j$ aplicados a las salidas anteriores $y[n-j]$:

$$y[n] = \sum_{k=0}^{M} b_k x[n-k] - \sum_{j=1}^{N} a_j y[n-j]$$

## 4. Implementación Práctica (Código)

La implementación de un filtro de Media Móvil en un dispositivo embebido como un ESP32 o Arduino requiere optimizar la memoria y la velocidad. En lugar de recalcular la suma completa cada vez que llega una nueva muestra del ADC, utilizamos un búfer circular para restar el valor más antiguo y agregar el valor más nuevo.

Aquí se presenta una implementación en C/C++ altamente eficiente y adecuada para el procesamiento del ADC en tiempo real:

``` c
#include <stdint.h>

// Definir el tamaño de la ventana para la media móvil
#define FILTER_TAP_NUM 8

// Estructura del filtro para mantener el estado
typedef struct {
    float history[FILTER_TAP_NUM];
    uint8_t head;
    float sum;
} MovingAverageFilter;

// Inicializar el estado del filtro
void MAF_Init(MovingAverageFilter* filter) {
    filter->head = 0;
    filter->sum = 0.0f;
    for(int i = 0; i < FILTER_TAP_NUM; i++) {
        filter->history[i] = 0.0f;
    }
}

// Actualizar el filtro con una nueva muestra cruda del ADC
float MAF_Update(MovingAverageFilter* filter, float new_sample) {
    // Restar el valor más antiguo de la suma actual
    filter->sum -= filter->history[filter->head];
    
    // Sobrescribir el valor más antiguo con la nueva muestra
    filter->history[filter->head] = new_sample;
    
    // Añadir la nueva muestra a la suma actual
    filter->sum += new_sample;
    
    // Avanzar el índice (head), reiniciando si es necesario (búfer circular)
    filter->head = (filter->head + 1) % FILTER_TAP_NUM;
    
    // Devolver el valor promediado
    return filter->sum / (float)FILTER_TAP_NUM;
}

// --- Ejemplo de Uso en un Bucle de Arduino/ESP32 ---
// MovingAverageFilter ecg_filter;
// void setup() { MAF_Init(&ecg_filter); }
// void loop() { 
//     float raw_adc = analogRead(A0);
//     float clean_ecg = MAF_Update(&ecg_filter, raw_adc);
// }
```
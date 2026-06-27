# Fundamentos Teóricos - Dispositivo ECG Portátil

> **⚠️ Disclaimer (English):** This section is **exclusively in Spanish** because it was easier for me to learn the concepts in my native language. If you need the information in English, please refer to other sections of the project.

> **⚠️ Disclaimer (Español):** Esta sección está **exclusivamente en español** porque fue más fácil para mí aprender los conceptos en mi idioma nativo. Si necesitas la información en inglés, consulta otras secciones del proyecto.

---

## 📝 Nota sobre el Contenido

El contenido de esta sección ha sido **generado con asistencia de IA** para proporcionar una guía estructurada y comprensiva de los fundamentos teóricos necesarios para diseñar, desarrollar e implementar un dispositivo ECG portátil. Estos materiales están diseñados para **aprender y revisar** conceptos fundamentales del proyecto.

Se recomienda:
- Revisar críticamente la información presentada
- Complementar con fuentes académicas y documentación oficial
- Validar los conceptos prácticos con implementaciones reales

---

## 🎯 Objetivos de Aprendizaje

Al completar esta sección teórica, deberías comprender:

- **Fundamentos médicos:** Cómo funciona el corazón a nivel electrofisiológico y cómo se representa gráficamente en un ECG
- **Principios físicos:** La naturaleza vectorial de la señal cardíaca y cómo se captura desde la superficie corporal
- **Ingeniería de hardware:** Los circuitos y componentes necesarios para adquirir la señal ECG con calidad médica
- **Procesamiento digital:** Técnicas de procesamiento de señales y algoritmos para extraer información clínica del ECG
- **Normativa y seguridad:** Los requisitos de seguridad que debe cumplir un dispositivo médico portátil
- **Integración del sistema:** Cómo todos estos componentes trabajan juntos en un dispositivo funcional

---

## 📚 Contenido

### 1. 🫀 Anatomía, Fisiología y Medicina (El Origen de la Señal)

Comprende los fundamentos del corazón como órgano electromecánico y cómo se genera la señal ECG que adquirimos.

- [Electrofisiología Cardíaca](./1-Electrofisiologia-Cardiaca.md) - Potenciales de acción, despolarización y repolarización
- [Sistema de Conducción Eléctrica](./2-Sistema-Conduccion-Electrica.md) - Ruta del impulso eléctrico en el corazón
- [Morfología del ECG Normal](./3-Morfologia-ECG-Normal.md) - Análisis de ondas, complejos e intervalos
- [Patologías Básicas y Arritmias](./4-Patologias-Arritmias.md) - Anomalías comunes en el ECG

### 2. 📐 Física y Matemáticas (La Naturaleza de la Señal)

Modela matemáticamente la señal cardíaca y establece el fundamento geométrico de las derivaciones.

- [Teoría de Dipolos y Vectores Cardíacos](./5-Teoria-Dipolos-Vectores.md) - El corazón como dipolo eléctrico en un medio conductor
- [Triángulo de Einthoven y Derivaciones](./6-Triangulo-Einthoven.md) - Geométrica de las derivaciones estándar (I, II, III)
- [Análisis de Frecuencia y Procesamiento de Señales](./7-Analisis-Frecuencia-DSP.md) - Dominio del tiempo y frecuencia, Transformada de Fourier
- [Algoritmos de Cálculo de Ritmo Cardíaco (HR)](./8-Algoritmos-Calculo-HR.md) - Métodos para calcular BPM

### 3. ⚡ Ingeniería de Hardware y Electrónica (La Captura)

Diseña el sistema de adquisición de la señal ECG: desde los electrodos hasta la conversión analógico-digital.

- [Interfaz Piel-Electrodo e Impedancia](./9-Interfaz-Piel-Electrodo.md) - Física de los electrodos Ag/AgCl y contacto de piel
- [Amplificador de Instrumentación (InAmp)](./10-Amplificador-Instrumentacion.md) - Circuitos de adquisición y CMRR
- [Filtros Analógicos y Acondicionamiento](./11-Filtros-Analogicos.md) - Diseño de pasa-altos, pasa-bajos y filtro notch
- [Conversión Analógico-Digital (ADC)](./12-Conversion-ADC.md) - Teorema de Nyquist y digitalización

### 4. 💻 Ingeniería de Software (Procesamiento y Visualización)

Implementa el procesamiento digital de la señal y la interfaz de usuario en el microcontrolador.

- [Arquitectura en Sistemas Embebidos](./13-Arquitectura-Sistemas-Embebidos.md) - Selección de microcontrolador y gestión de ADC
- [Filtros Digitales (DSP)](./14-Filtros-Digitales-DSP.md) - Implementación de filtros FIR e IIR
- [Algoritmo de Pan-Tompkins](./15-Algoritmo-Pan-Tompkins.md) - Detección de picos R en tiempo real
- [Transmisión y Protocolos de Datos](./16-Transmision-Protocolos.md) - BLE, Wi-Fi (MQTT), comunicación Serial/UART
- [Interfaz de Usuario y Visualización](./17-Interfaz-Usuario-Visualizacion.md) - Desarrollo del software receptor (Python, JavaScript, Flutter)

### 5. 🔒 Seguridad, Normativa y Aislamiento (Protección del Paciente)

Garantiza que el dispositivo sea seguro para su uso clínico en humanos.

- [Aislamiento Galvánico y Seguridad Eléctrica](./18-Aislamiento-Galvanico.md) - Protección contra electrocución
- [Estándares de Dispositivos Médicos (IEC 60601)](./19-Estandares-IEC-60601.md) - Normativas internacionales de seguridad

### 📊 Extra: ECG de 3 Derivaciones (Configuración del Proyecto)

Este proyecto utiliza un sistema de **3 derivaciones** en lugar del estándar de 12 derivaciones. Esta sección complementaria explica cómo se adaptan los conceptos teóricos al sistema simplificado de 3 canales.

- [ECG de 3 Derivaciones](./20-ECG-3-Derivaciones.md) - Configuración, ventajas y limitaciones del sistema de 3 canales para dispositivos portátiles

---

## 📖 Cómo Usar Esta Sección

1. **Para principiantes:** Lee los temas en orden (1 → 5) para construir una comprensión progresiva
2. **Para consultas específicas:** Accede directamente al topic que necesites refrescar
3. **Para implementación:** Los temas de software (sección 4) son críticos cuando comiences a codificar
4. **Para validación:** Los temas de seguridad (sección 5) deben revisarse antes de usar el dispositivo con pacientes

---

## 🔗 Más Información

- [← Volver al README Principal](../README.md)

---

**Última actualización:** 2026-06-28

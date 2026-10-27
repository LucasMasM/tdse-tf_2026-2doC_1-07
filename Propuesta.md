# Sistema de Detección y Monitoreo de Objetos por Ultrasonido

 

**Autores: Lucas Lautaro Maselli Miño, Ian Gabriel Pereiras**

**Padrones: 113627, 113567**

**Fecha: 2do cuatrimestre 2026**




## 1. Selección del proyecto a implementar

### 1.1 Objetivo del proyecto y resultados esperados
El objetivo de este proyecto es diseñar e implementar un Producto Mínimo Viable (PMV) para un sistema de detección y monitoreo de objetos por ultrasonido sobre la plataforma de desarrollo NUCLEO-F103RB (STM32F103RB), utilizando un sensor de distancia montado sobre un servomotor para realizar exploraciones angulares del entorno. El sistema integrará señales de alerta sonoras proporcionales a la cercanía de los obstáculos y una interfaz gráfica de usuario en tiempo real.

Como objetivos adicionales, el sistema incluirá:

*   **Mecanismo de alerta sonora proporcional:** Controlado mediante un buzzer que se activará al detectar un objeto a partir de un umbral de distancia configurable, incrementando progresivamente la frecuencia del sonido mediante PWM conforme el objeto se aproxime al sensor.
*   **Dos modos de funcionamiento seleccionables:**
    *   **Modo Manual:** Permite al usuario controlar la orientación del servomotor y direccionar el sensor en tiempo real mediante un joystick analógico (lectura por ADC del STM32).
    *   **Modo Automático:** Ejecuta un paneo constante y automatizado de $180^{\circ}$ a un paso determinado mediante generación de señales PWM para la exploración continua del área.
*   **Pantalla LCD/Display con interfaz visual organizada en tres renglones:**
    1.  Primer renglón: Modo de funcionamiento seleccionado (Manual o Automático).
    2.  Segundo renglón: Confirmación de detección de objeto (Detección: SI / NO).
    3.  Tercer renglón: Distancia calculada entre el sensor y el objeto detectado (en cm).
*   **Navegación por Menú:** Controlado mediante un botón físico y un joystick (selección y navegación).
*   **Robustez y Arquitectura:** Tratamiento de rebotes mecánicos (debouncing) en pulsadores, filtrado de ruido en lecturas analógicas/ultrasónicas y una arquitectura modular Bare Metal sobre el STM32F103RB orientada al control en tiempo real.

### 1.2 Proyectos similares
Se consideran tres alternativas de implementación para ponderar el alcance del hardware y la viabilidad técnica:

1.  **Proyecto Base (STM32F103RB + Sensor Ultrasonido + Servomotor + Menu LCD + Joystick + Buzzer + Botón + Flash Interna)**: Sistema de barrido angular con dos modos (Manual/Automático), alertas sonoras e interfaz gráfica.
2.  **Base + Módulo Bluetooth (HM-10):** Agrega conectividad inalámbrica por UART para transmitir datos de barrido y recibir comandos desde una aplicación.
3.  **Base + Matriz de LEDs 8x8 / Pantalla OLED gráfica (SPI/I2C):** Reemplaza o complementa la pantalla por una visualización del radar en tiempo real por hardware.

Para evaluar las alternativas, se ponderan los siguientes criterios (escala 1 a 10):
*   **Disponibilidad de Hardware (peso 9):** Facilidad de adquisición en el mercado local sin depender de importaciones.
*   **Impacto en el Proyecto (peso 8):** Aporte a la funcionalidad y valor agregado del prototipo.
*   **Costo (peso 8):** Inversión económica global requerida.
*   **Dificultad Técnica / Viabilidad (peso 10):** Complejidad de desarrollo dentro de los plazos y requerimientos de la materia.
*   **Interés Personal (peso 7):** Motivación del equipo.

| Criterio | Proyecto Base | Base + Módulo Bluetooth (HM-10) | Base + Matriz gráfica / OLED |
| :--- | :--- | :--- | :--- |
| Disponibilidad de Hardware (peso: 9) | 10 (90) | 10 (90) | 8 (72) |
| Impacto en el Proyecto (peso: 8) | 8 (64) | 10 (80) | 7 (56) |
| Costo (peso: 8) | 9 (72) | 8 (64) | 7 (56) |
| Viabilidad (peso: 10) | 9 (90) | 8 (80) | 6 (60) |
| Interés Personal (peso: 7) | 8 (56) | 9 (63) | 8 (56) |
| **Puntaje Total** | **372** | **377** | **300** |

### 1.3 Selección de proyecto
En función de la ponderación realizada, se selecciona la opción **Base + Módulo Bluetooth (HM-10)**. Esta alternativa cumple íntegramente con los requisitos de hardware obligatorio del curso sobre el microcontrolador STM32F103RB (Bluetooth HM-10 por USART, sensores analógicos por ADC, buzzer, almacenamiento en Flash interna, botones e interrupciones GPIO/EXTI) y aporta un diferencial técnico sin comprometer la viabilidad dentro del calendario.

## 2. Especificación de Hardware y Programación

### 2.1 Hardware Obligatorio y Adicional

**Hardware Obligatorio:**
*   Placa de desarrollo NUCLEO-F103RB / Microcontrolador STM32F103RB.
*   Módulo Bluetooth HM-10 (comunicación serie USART).
*   Sensor analógico: Joystick analógico (conectado a canales ADC del STM32).
*   1 Botón/Pulsador físico (conectado a entrada digital GPIO).
*   Buzzer (excitado mediante canal PWM de un Timer del STM32).
*   LEDs indicadores (para indicación de estados).
*   Flash interna del STM32F103RB (umbrales de distancia, paso de barrido, etc.).

**Hardware Adicional (específico del proyecto):**
*   Sensor de distancia por ultrasonido (Gestionado mediante Timers).
*   Servomotor (control de barrido angular 0°-180° mediante señal PWM a 50 Hz).
*   Display/Pantalla LCD (Visualización organizada en 3 renglones).

### 2.2 Programación Obligatoria & Adicional

**Programación Obligatoria:**
*   **Arquitectura Bare Metal:** Sin Sistema Operativo, del tipo Event-Triggered System.
*   **Estructurada / Modular:** Esquema Escrutar/Procesar/Actuar en C sobre el microcontrolador STM32F103RB.
*   **Patrones de Software Requeridos:**
    *   Ejecutor Cíclico/Super-Loop: Polling + Interrupciones (1 vuelta < 1ms).
    *   SysTick/Tick de Tiempo: Tick = 1ms usando el periférico del STM32 Callbacks.
    *   Tareas No Bloqueantes: Código sin retardos bloqueantes.
    *   Modelos de Estado: Diagramas de Estado Finito (FSM).
    *   Menú Interactivo: Navegación mediante botón, joystick y pantalla.
    *   Modos de operación: Mínimo dos modos (automático y manual).
    *   Bajo Consumo: Modos de reposo/Power saving del STM32 en períodos de inactividad.
    *   Uso de periféricos por I2C/SPI/USART: Para comunicación con periféricos.
    *   Gestión de Periféricos no bloqueantes: Polling sin bloqueos, Interrupciones y/o DMA con Callbacks.

**Programación Adicional:**
*   Algoritmo de filtrado digital (promedio móvil o media acotada) para eliminar lecturas espurias de distancia del ultrasonido y mediciones analógicas del joystick (ADC).
*   Software Debouncing: Máquina de estados no bloqueante basada en el SysTick de 1 ms para la eliminación de rebotes mecánicos en el botón.

### 2.3 Diagrama en bloques

<img width="900" height="400" alt="image" src="https://github.com/user-attachments/assets/85f73cdf-2a12-4ad9-93c9-66bc9eccaf69" />

## 3. Elicitación de Requisitos y Casos de Uso

### 3.1 Requisitos del Sistema

| Grupo | ID | Descripción |
| :--- | :--- | :--- |
| **Sensado y Control Espacial** | 1.1 | El sistema medirá distancias en centímetros mediante un sensor de ultrasonido (usando Timers del STM32F103RB) montado en un servomotor. |
| | 1.2 | El servomotor realizará un paneo/barrido automático continuo de 0° a 180° controlado mediante PWM (Timer del STM32) en modo automático. |
| | 1.3 | En modo manual, el usuario orientará el servomotor en tiempo real mediante las lecturas analógicas del joystick (canal ADC del STM32F103RB). |
| | 1.4 | Las lecturas analógicas (ADC) y de distancia deberán aplicar filtros digitales para eliminar ruido e imprecisiones. |
| **Alertas Sonoras y Visuales** | 2.1 | El sistema activará el buzzer cuando se detecte un objeto por debajo del umbral de distancia configurado. |
| | 2.2 | La frecuencia o cadencia del sonido del buzzer aumentará proporcionalmente (frecuencia PWM ajustable) a medida que el objeto esté más cerca. |
| | 2.3 | Se dispondrá de LEDs indicativos para señalar el estado del sistema (NORMAL, FALLA). |
| **Interfaz y Navegación** | 3.1 | La pantalla mostrará en Renglón 1 el Modo de funcionamiento (Manual / Automático). |
| | 3.2 | La pantalla mostrará en Renglón 2 el estado de detección (Objeto: SÍ / NO). |
| | 3.3 | La pantalla mostrará en Renglón 3 la distancia actual calculada en cm. |
| | 3.4 | El menú se navegará mediante un joystick y un botón físico con filtrado de rebotes (debouncing por FSM no bloqueante).<br>
| **Almacenamiento y Ajustes** | 4.1 | Los parámetros del sistema (umbrales de alerta, velocidad de barrido) se almacenarán en páginas dedicadas de la Flash interna del STM32F103RB. |
|  | 4.2 | Al reiniciar, el sistema cargará los datos guardados en Flash y verificará su validez. |
| **Conectividad y Estados** | 5.1 | El sistema transmitirá el estado del barrido y mediciones vía Bluetooth HM-10 utilizando el periférico USART del STM32. |
| | 5.2 | El firmware operará bajo los estados de alto nivel: NORMAL / FALLA. |
| | 5.3 | En caso de desconexión del sensor o lectura fuera de rango crítico, el sistema entrará en estado FALLA. |

### 3.2 Casos de Uso

**Caso de Uso 1: Detección de Obstáculo en Modo Automático**

| Elemento | Definición |
| :--- | :--- |
| **Disparador** | El sistema detecta un objeto dentro del rango del umbral mientras ejecuta el barrido de 180° |
| **Precondiciones** | STM32F103RB encendido en modo NORMAL, submodo AUTOMÁTICO. Sensor y servomotor operativos. Umbrales cargados desde la Flash interna. |
| **Flujo Principal** | 1. El Timer del STM32 incrementa el ancho de pulso PWM del servomotor para cambiar su ángulo.<br>2. El sensor realiza la lectura de distancia (medida por Timer Input Capture) y aplica filtrado digital.<br>3. El firmware valida que la distancia es menor al umbral configurado.<br>4. Se actualiza el Renglón 2 del LCD ("Detección: Si") y el Renglón 3 con la distancia en cm.<br>5. Se calcula la proximidad y se ajusta la frecuencia/cadencia del PWM del buzzer.<br>6. Se transmiten los datos vía USART al módulo HM-10 (Bluetooth). |
| **Flujos Alternativos** | a. Objeto fuera de rango: Se muestra "Detección: NO", se apaga el buzzer y continúa el paneo.<br>b. Fallo en el sensor: Si el Timer no recibe la señal de eco tras el timeout, el sistema pasa a estado FALLA y enciende el LED correspondiente. |

**Caso de Uso 2: Control Orientativo Manual mediante Joystick**

| Elemento | Definición |
| :--- | :--- |
| **Disparador** | El usuario selecciona el modo MANUAL desde el menú. |
| **Precondiciones** | STM32F103RB en modo NORMAL. Conversión ADC del joystick activa (mediante Polling no bloqueante o DMA). |
| **Flujo Principal** | 1. El usuario mueve la palanca del joystick hacia la izquierda o derecha.<br>2. El ADC del STM32 lee el valor analógico y ajusta el pulso PWM del servomotor proporcionalmente.<br>3. El sensor de ultrasonido mide la distancia en esa dirección puntual.<br>4. El Renglón 1 confirma "Modo: MANUAL", y los renglones 2 y 3 muestran la detección y distancia.<br>5. Si hay obstáculo cercano, el buzzer suena según la distancia de ese ángulo. |
| **Flujos Alternativos** | Joystick en reposo: El servomotor mantiene su última posición angular asignada. |

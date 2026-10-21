# **Sistema de Detección de Objetos**

 

**Autores: Lucas Lautaro Maselli Miño, Ian Gabriel Pereiras**

**Padrones: 113627, 113567**

**Fecha: 2do cuatrimestre 2026**


### 1.1 Objetivo del proyecto y resultados esperados

El objetivo de este proyecto es diseñar e implementar un sistema de detección y monitoreo de objetos por ultrasonido, utilizando un sensor de distancia montado sobre un servomotor para realizar exploraciones angulares del entorno. El sistema integrará señales de alerta sonoras proporcionales a la cercanía de los obstáculos e interfaz gráfica de usuario en tiempo real.

Como objetivos adicionales, el sistema incluirá:

* **Un mecanismo de alerta sonora proporcional**, controlado mediante un *buzzer* que se activará al detectar un objeto a partir de cierto umbral de distancia, incrementando progresivamente la frecuencia del sonido conforme el objeto se aproxime al sensor.
* **Dos modos de funcionamiento seleccionables desde un menú de usuario**:
* *Modo Manual*: Permite al usuario controlar la orientación del servomotor y direccionar el sensor en tiempo real mediante un *joystick*.
* *Modo Automático*: Ejecuta un barrido o paneo constante y automatizado de 180° a un paso determinado para la exploración continua del área.


* **Una pantalla/display con una interfaz organizada en tres renglones**, que mostrará permanentemente:
i. Primer renglón: El modo de funcionamiento seleccionado (*Manual* o *Automático*).
ii. Segundo renglón: La confirmación de si se ha detectado o no un objeto.
iii. Tercer renglón: La distancia calculada entre el sensor y el objeto detectado.
* **Un sistema de navegación para el menú**, controlado mediante **tres botones** físicos para el desplazamiento por la interfaz y la selección de configuraciones.

Se pretende que el sistema sea robusto frente a rebotes mecánicos de los pulsadores y ruidos de lectura del sensor, utilizando una arquitectura modular orientada al control en tiempo real. Al finalizar el proyecto se espera obtener un prototipo funcional, intuitivo y preciso para el monitoreo de cercanía y barrido espacial.

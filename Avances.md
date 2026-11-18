<img width="762" height="292" alt="image" src="https://github.com/user-attachments/assets/fa319b31-df72-4906-ab5b-7b1bd385841a" />

# Sistema de detección y monitoreo de objetos por ultrasonido

 

**Autores: Lucas Lautaro Maselli Miño, Ian Gabriel Pereiras**

**Padrones: 113627, 113567**

**Fecha: 2do cuatrimestre 2026**

Teniendo en cuenta los requerimientos del proyecto se deja detallado a continuación los avances del mismo.

| Estado | Descripción |
| --- | --- |
| 🟢 | Ya implementado |
| 🟡 | En proceso de implementarse |
| 🔴 | No se implementará |

#### **Sensado y Control Espacial**

| Req ID | Descripción | Estado |
| --- | --- | --- |
| 1.1 | El sistema medirá distancias en centímetros mediante un sensor de ultrasonido (usando Timers del STM32F103RB) montado en un servomotor. | 🟢 |
| 1.2 | El servomotor realizará un paneo/barrido automático continuo de 0° a 180° controlado mediante PWM (Timer del STM32) en modo automático. | 🟡 |
| 1.3 | En modo manual, el usuario orientará el servomotor en tiempo real mediante las lecturas analógicas del joystick (canal ADC del STM32F103RB). | 🟡 |
| 1.4 | Las lecturas analógicas (ADC) y de distancia deberán aplicar filtros digitales para eliminar ruido e imprecisiones. | 🟡 |

#### **Alertas Sonoras y Visuales**

| Req ID | Descripción | Estado |
| --- | --- | --- |
| 2.1 | El sistema activará el buzzer cuando se detecte un objeto por debajo del umbral de distancia configurado. | 🟡 |
| 2.2 | La frecuencia o cadencia del sonido del buzzer aumentará proporcionalmente (frecuencia PWM ajustable) a medida que el objeto esté más cerca. | 🟡 |
| 2.3 | Se dispondrá de LEDs indicativos para señalar el estado del sistema (NORMAL, FALLA). | 🟡 |

#### **Interfaz y Navegación**

| Req ID | Descripción | Estado |
| --- | --- | --- |
| 3.1 | La aplicación mostrará en Renglón 1 el Modo de funcionamiento (Manual / Automático). | 🟡 |
| 3.2 | La aplicación mostrará en Renglón 2 el estado de detección (Objeto: SÍ / NO). | 🟡 |
| 3.3 | La aplicación mostrará en Renglón 3 la distancia actual calculada en cm. | 🟡 |
| 3.4 | El menú se navegará mediante la aplicación con botones virtuales (control bluetooth). | 🟡 |

#### **Almacenamiento y Ajustes**

| Req ID | Descripción | Estado |
| --- | --- | --- |
| 4.1 | Los parámetros del sistema (umbrales de alerta, velocidad de barrido) se almacenarán en páginas dedicadas de la Flash interna del STM32F103RB. | 🟡 |
| 4.2 | Al reiniciar, el sistema cargará los datos guardados en Flash y verificará su validez. | 🟡 |

#### **Conectividad y Estados**

| Req ID | Descripción | Estado |
| --- | --- | --- |
| 5.1 | El sistema transmitirá el estado del barrido y mediciones vía Bluetooth HM-10 utilizando el periférico USART del STM32. | 🟡 |
| 5.2 | El firmware operará bajo los estados de alto nivel: NORMAL / FALLA. | 🟡 |
| 5.3 | En caso de desconexión del sensor o lectura fuera de rango crítico, el sistema entrará en estado FALLA. | 🟡 |

<p align="center">

&#x20; <img src="assets/fire-detection-system-banner.jpg"

&#x20;      alt="Sistema de detección y extinción de incendios"

&#x20;      width="100%">

</p>





\#  EIC901 — Etapa 1



\## Sistema de extinción por votación 2 de 3



\*\*Circuitos de interfaz analógica y digital\*\*



!\[Estado](https://img.shields.io/badge/Estado-Completado-brightgreen)

!\[Simulación](https://img.shields.io/badge/Simulaci%C3%B3n-Tinkercad-blue)

!\[Pruebas](https://img.shields.io/badge/Pruebas-8%2F8-success)

!\[Alimentación](https://img.shields.io/badge/Alimentaci%C3%B3n-5V-orange)



\---



\##  Descripción



Este proyecto implementa un \*\*sistema de extinción por votación 2 de 3 con detector de lazo cerrado\*\*.



El sistema recibe tres entradas digitales:



| Entrada | Descripción |

|---|---|

| \*\*A\*\* | Detector 1 — `1 = fuego` |

| \*\*B\*\* | Detector 2 — `1 = fuego` |

| \*\*C\*\* | Detector de lazo cerrado — `1 = normal`, `0 = fuego o corte del cable` |



La salida \*\*F\*\* determina la activación del actuador.



```text

F = 0 → Actuador desactivado

F = 1 → Actuador activado

```



\---



\##  Funcionamiento general



```mermaid

flowchart LR

&#x20;   A\["Detector A"] --> L\["Lógica combinacional"]

&#x20;   B\["Detector B"] --> L

&#x20;   C\["Detector C<br/>Lazo cerrado"] --> N\["NOT"]

&#x20;   N -->|"C'"| L



&#x20;   L -->|"F"| LED\["LED indicador"]

&#x20;   L -->|"F"| RB\["RB = 1 kΩ"]

&#x20;   RB --> Q\["Transistor NPN"]

&#x20;   Q --> M\["Motor / Actuador"]



&#x20;   D\["Diodo flyback"] -. Protección .-> M

```



La lógica digital genera la señal \*\*F\*\*, pero la salida de una compuerta lógica no se utiliza para alimentar directamente el actuador.



Por ello se implementó una etapa de potencia con:



\- transistor \*\*BJT NPN\*\*;

\- resistencia de base de \*\*1 kΩ\*\*;

\- diodo \*\*flyback\*\*;

\- motor de CC como representación de una electroválvula.



\---



\##  Función lógica



\### Función canónica



```text

F = A'BC' + AB'C' + ABC' + ABC

```



```text

F(A,B,C) = Σm(2,4,6,7)

```



\### Simplificación



Mediante mapa de Karnaugh:



```text

F = AB + AC' + BC'

```



Forma factorizada utilizada para la implementación:



```text

F = AB + C'(A + B)

```



\---



\##  Circuito combinacional



La implementación utiliza:



| Integrado | Función |

|---|---|

| \*\*74HC04\*\* | NOT |

| \*\*74HC08\*\* | AND |

| \*\*74HC32\*\* | OR |



Las señales internas se identificaron como:



```text

N = C'

P = AB

Q = A + B

R = C'(A + B)



F = P + R

```



\---



\##  Código de colores del circuito



Para facilitar la interpretación del montaje en Tinkercad se utilizó el siguiente código:



| Color | Uso |

|---|---|

| 🔴 Rojo | Alimentación +5 V |

| ⚫ Negro | GND |

| 🟡 Amarillo | Entrada A |

| 🟢 Verde | Entrada B |

| 🔵 Azul | Entrada C |

| 🟣 Morado | Señales lógicas internas |

| 🟠 Naranja | Salida F |

| ⚪ Gris | Etapa BJT / potencia |



\---



\##  Interfaz BJT



La etapa de potencia utiliza el transistor NPN como \*\*interruptor de lado bajo\*\*.



```text

+5 V

&#x20;│

Motor

&#x20;│

Collector

&#x20;│

BJT NPN

&#x20;│

Emitter

&#x20;│

GND

```



La base del transistor recibe la señal:



```text

F → RB = 1 kΩ → Base

```



El diodo flyback se conecta en paralelo con la carga inductiva para proteger la etapa de conmutación frente a sobretensiones producidas al apagar el motor.



\---



\##  Cálculos eléctricos



\### Corriente de base



```text

Vlogic = 5,0 V

VBE ≈ 0,7 V

RB = 1000 Ω

```



```text

IB = (Vlogic - VBE) / RB

```



```text

IB = (5,0 - 0,7) / 1000

IB ≈ 4,3 mA

```



\### Mediciones en Tinkercad



| Parámetro | Valor |

|---|---:|

| Voltaje lógico | 5,0 V |

| VBE aproximado | 0,7 V |

| Resistencia de base | 1 kΩ |

| Corriente de base | 4,3 mA |

| Corriente medida del motor | \*\*79,3 mA\*\* |

| Voltaje medido en la carga | \*\*≈ 4,70 V\*\* |

| Relación IC / IB | \*\*≈ 18,4\*\* |

| Potencia aproximada | \*\*≈ 0,37 W\*\* |



La corriente de \*\*79,3 mA\*\* corresponde al motor utilizado en la simulación como representación del actuador.



\---



\##  Validación del circuito



Se probaron sistemáticamente las \*\*8 combinaciones posibles\*\*.



| Prueba | A | B | C | F esperada | F obtenida | Resultado |

|---:|---:|---:|---:|---:|---:|---|

| 1 | 0 | 0 | 0 | 0 | 0 | ✅ |

| 2 | 0 | 0 | 1 | 0 | 0 | ✅ |

| 3 | 0 | 1 | 0 | 1 | 1 | ✅ |

| 4 | 0 | 1 | 1 | 0 | 0 | ✅ |

| 5 | 1 | 0 | 0 | 1 | 1 | ✅ |

| 6 | 1 | 0 | 1 | 0 | 0 | ✅ |

| 7 | 1 | 1 | 0 | 1 | 1 | ✅ |

| 8 | 1 | 1 | 1 | 1 | 1 | ✅ |



\### Resultado



```text

8 / 8 pruebas correctas

```



Cuando:



```text

F = 1

```



el LED se enciende, el transistor conduce y el motor se activa.



Cuando:



```text

F = 0

```



el LED y el motor permanecen desactivados.



\---



\##  Documentación del proyecto



| Recurso | Ubicación |

|---|---|

|  Cálculos y pruebas | \[`02\_Calculos`](./02\_Calculos/) |

|  Diagramas | \[`03\_Diagramas`](./03\_Diagramas/) |

|  Archivos de Tinkercad | \[`03\_Diagramas/Tinkercad`](./03\_Diagramas/Tinkercad/) |

|  Capturas y evidencias | \[`04\_Capturas`](./04\_Capturas/) |

|  Video de funcionamiento | \[`04\_Capturas/Video`](./04\_Capturas/Video/) |

|  Informe final | \[`05\_Informe`](./05\_Informe/) |



\---



\##  Estado de la Etapa 1



\- \[x] Tabla de verdad analizada

\- \[x] Función canónica derivada

\- \[x] Simplificación mediante Karnaugh

\- \[x] Verificación mediante álgebra booleana

\- \[x] Circuito combinacional diseñado

\- \[x] Implementación con 74HC04, 74HC08 y 74HC32

\- \[x] Interfaz de potencia con BJT

\- \[x] Resistencia de base calculada

\- \[x] Diodo flyback implementado

\- \[x] Corriente del actuador medida

\- \[x] Ocho combinaciones comprobadas

\- \[x] Diagramas documentados

\- \[x] Evidencias y video almacenados

\- \[x] Informe final completado



\---



\##  Resultado final



> \*\*El sistema implementa correctamente la lógica de votación 2 de 3 y controla una carga inductiva mediante una interfaz BJT, obteniendo resultados correctos en las 8 combinaciones de entrada evaluadas.\*\*



\*\*Estado del proyecto:  ETAPA 1 COMPLETADA\*\*


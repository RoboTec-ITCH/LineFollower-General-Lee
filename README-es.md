<p align="center">
  <img src="https://www.itchrobotec.com/images/logow.png" alt="Logo de RoboTec" width="720">
</p>

# LineFollower General Lee v2: Exhibición de Hardware y Arquitectura

[English](README.md) | [Español](README-es.md)

![Tipo de Proyecto](https://img.shields.io/badge/Proyecto-Exhibici%C3%B3n_T%C3%A9cnica-blue.svg)
![Plataforma](https://img.shields.io/badge/Plataforma-Arduino_Nano-00979D.svg)
![Estado](https://img.shields.io/badge/Estado-Competici%C3%B3n-orange.svg)
![Licencia](https://img.shields.io/badge/Licencia-MIT-green.svg)

## Índice

- [Resumen del Proyecto](#resumen-del-proyecto)
- [Estado del Proyecto](#estado-del-proyecto)
- [Arquitectura de Hardware y Características](#arquitectura-de-hardware-y-características)
  - [Chasis y Planta Motriz](#chasis-y-planta-motriz)
  - [Barra de Sensores Hermes](#barra-de-sensores-hermes)
  - [Módulo de Arranque](#módulo-de-arranque)
- [Integración de Componentes (BOM)](#integración-de-componentes-bom)
- [Esquema de Conexiones (Pinout)](#esquema-de-conexiones-pinout)
- [Configuración de Competencia (Pit Lane)](#configuración-de-competencia-pit-lane)
- [Arquitectura del Sistema y Flujo de Control](#arquitectura-del-sistema-y-flujo-de-control)
- [Créditos y Equipo](#créditos-y-equipo)
- [Licencia](#licencia)

## Resumen del Proyecto

El repositorio **LineFollower-General-Lee-Version-2** sirve como una exhibición técnica y portafolio de documentación para la arquitectura de hardware de un robot seguidor de línea de alta velocidad (velocista), desarrollado por el club de robótica **RoboTec**.

Este proyecto demuestra la ingeniería detrás de un robot de grado competitivo, destacando una barra frontal personalizada de 16 sensores reflectivos ("Hermes") y un avanzado sistema de control PID (Proporcional-Integral-Derivativo). El diseño incluye ajuste dinámico en tiempo real, frenado predictivo de alta inercia y procesamiento analógico multiplexado.

> **Nota:** Este repositorio está dedicado a la documentación del hardware y la perspectiva arquitectónica. El firmware principal de control en C++ es propiedad exclusiva de la organización y se mantiene en un repositorio privado por el equipo de ingeniería de RoboTec.

## Estado del Proyecto

**Listo para Competencia / Estable** - La arquitectura de hardware y la integración descritas aquí representan la iteración final del robot, validada en pista.

## Arquitectura de Hardware y Características

El hardware del robot está diseñado para velocidad extrema, precisión y adaptabilidad en la pista. La lógica central cuenta con perfiles PID dinámicos, frenado predictivo diferencial para curvas de 90 grados e inversión óptica por software.

A continuación se detalla el desglose físico de los módulos que impulsan al robot:

### Chasis y Planta Motriz

<p align="center">
  <img src="images/20260428_190512.jpg" alt="Chasis General Lee Versión 2 Superior" width="35%">
  &nbsp;
  <img src="images/20260428_190515.jpg" alt="Chasis General Lee Versión 2 Inferior" width="35%">
</p>

El chasis completamente ensamblado alberga la planta motriz de alta velocidad impulsada por dos motores DC de 3000 RPM. Estos son controlados por un driver TB6612FNG capaz de manejar 1.2A continuos (3A pico). Esta configuración ejecuta la aceleración extrema y las maniobras agresivas de "ancla" en reversa necesarias para el frenado predictivo.

### Barra de Sensores Hermes

<p align="center">
  <img src="images/hermes.png" alt="Barra de 16 Sensores Hermes" width="600">
</p>

La placa "Hermes" es un arreglo frontal personalizado de 16 sensores reflectivos analógicos diseñados para el seguimiento de línea de alta resolución. Utiliza un **multiplexor CD74HC4067** para ciclar rápidamente a través de los 16 canales, superando el límite de pines del Arduino Nano y entregando una lectura analógica precisa para los cálculos de promedio ponderado.

### Módulo de Arranque

<p align="center">
  <img src="images/control.png" alt="Módulo de Arranque Ingeniero Maker" width="600">
</p>

Para cumplir con las normas oficiales de competencia, el robot se enlaza con un receptor RF de _Ingeniero Maker_. Este módulo provee señales lógicas críticas:

- **Señal Ready:** Activa el "Modo Centinela", una rutina activa de autocentrado a baja velocidad que asegura la alineación perfecta en la línea de salida.
- **Señal Go:** Desbloquea oficialmente el lazo de control PID principal de alta velocidad, lanzando al robot.

## Integración de Componentes (BOM)

| Componente             | Especificaciones          | Rol en el Sistema                                     |
| :--------------------- | :------------------------ | :---------------------------------------------------- |
| **Microcontrolador**   | Arduino Nano (ATmega328P) | Procesamiento central y ejecución del lazo PID.       |
| **Barra de Sensores**  | Placa Custom "Hermes"     | Arreglo frontal de 16 sensores para alta resolución.  |
| **Multiplexor**        | CD74HC4067                | Multiplexación analógica de 16 canales.               |
| **Driver de Motores**  | TB6612FNG                 | Control de potencia PWM.                              |
| **Planta Motriz**      | 2x Motores DC (3000 RPM)  | Tracción diferencial y propulsión de alta velocidad.  |
| **Alimentación**       | LiPo 2S (7.4V nominal)    | Fuente de poder principal para lógica y motores.      |
| **Módulo de Arranque** | Módulo RF Ingeniero Maker | Receptor RF que provee las señales lógicas oficiales. |

## Esquema de Conexiones (Pinout)

<p align="center">
  <img src="images/schematic.png" alt="Diagrama de Conexiones" width="720">
</p>

La integración interna sigue esta distribución lógica exacta:

**Motores y Driver TB6612FNG**

- `AIN1`: D8 | `AIN2`: D7 | `PWMA`: D5 (Motor Derecho)
- `BIN1`: D9 | `BIN2`: D4 | `PWMB`: D6 (Motor Izquierdo)

**Arreglo de Sensores "Hermes" (CD74HC4067)**

- `S0`: A3 | `S1`: A2 | `S2`: A1 | `S3`: A0
- `COM` (Salida Analógica MUX): A4

**Módulo de Arranque (Ingeniero Maker)**

- `RDY` (Señal Ready): D12
- `GO` (Señal Start): D10

## Configuración de Competencia (Pit Lane)

Para operar el robot bajo condiciones de competencia, se ejecuta el siguiente flujo de trabajo en la zona de pits:

1. **Inicialización:** Enciende el robot usando la batería LiPo 2S. El sistema carga por defecto el primer perfil PID almacenado.
2. **Selección de Perfil:** Si no se ha recibido la señal `GO`, el usuario puede usar el botón integrado (o la entrada `Ready`) para iterar entre los perfiles PID. Un LED en la placa indica el perfil seleccionado.
3. **Alineación Centinela:** Al recibir la señal `Ready` vía RF, el robot entra en **Modo Centinela**. Se alinea activamente con la línea de la pista a muy baja velocidad para asegurar una posición de salida perfecta.
4. **Lanzamiento:** Al recibir la señal `GO`, se desbloquea el lazo de control PID de alta velocidad y comienza la secuencia de carrera.

## Arquitectura del Sistema y Flujo de Control

Aunque el código fuente es privado, el flujo de control interno opera de la siguiente manera:

1. **Adquisición de Datos:** El Arduino itera rápidamente por los 4 pines de control (S0-S3) del multiplexor CD74HC4067, leyendo la única salida analógica (`COM`) para escanear los 16 sensores de la placa Hermes.
2. **Cálculo de Error:** Un algoritmo de promedio ponderado al centro procesa los valores analógicos para calcular el error de posición exacto respecto a la línea.
3. **Procesamiento PID:** El perfil PID seleccionado calcula la corrección necesaria. Si se detecta una curva de 90 grados, el algoritmo de frenado predictivo interrumpe la salida PID estándar.
4. **Actuación:** La respuesta calculada se convierte en señales PWM independientes enviadas al driver TB6612FNG, generando tracción diferencial en los motores de 3000 RPM.

## Créditos y Equipo

Esta arquitectura de hardware y su lógica de control subyacente son el resultado de ingeniería y pruebas dedicadas.

- **Mauricio Gómez Márquez** - Arquitectura de Software, Lógica de Control PID y Optimización de Firmware.
- **Alexander Armando Martinez Gil** - Ingeniería de Hardware, Diseño de PCB (Placa "Hermes") e Integración de Sensores.
- **Club de Robótica RoboTec** - Organización, instalaciones de prueba y comunidad.

## Licencia

La documentación de la arquitectura de hardware, diagramas de conexión y presentaciones visuales de este repositorio se publican bajo la **Licencia MIT**. Consulta el archivo `LICENSE` para más detalles.

> **Nota:** El firmware de control en C++ subyacente sigue siendo propiedad exclusiva de la organización y no está cubierto por esta licencia de código abierto.

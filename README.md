# ⚙️ Taller de Conectividad en el Borde — Fundamentos de Biodiseño  
Taller de conectividad con ESP32 y MIT App Inventor  

<p align="center">
  <img src="taller_conectividad.png" alt="Taller de Conectividad en el Borde con ESP32 y MIT App Inventor" width="600"><br>
  <em>Figura 1. Taller de Conectividad en el Borde con ESP32 y MIT App Inventor.</em>
</p>

Este repositorio contiene el material completo del **Taller de Conectividad en el Borde** del curso **Fundamentos de Biodiseño**, dictado en el 4.º ciclo de la carrera de Ingeniería Biomédica en la Universidad Peruana Cayetano Heredia (UPCH).

---

## 🎯 Objetivo del taller

Fortalecer competencias en electrónica y desarrollo de aplicaciones móviles para dispositivos biomédicos simples, mediante la integración de un microcontrolador ESP32 y MIT App Inventor, abarcando comunicación Wi-Fi y Bluetooth Low Energy (BLE), adquisición de datos de sensores y visualización en tiempo real.

---

## 🧩 ¿Qué aprenderás?

- Configurar el entorno de desarrollo para ESP32 (Arduino IDE/PlatformIO)  
- Establecer comunicación Wi-Fi y BLE entre ESP32 y la app móvil  
- Diseñar bloques lógicos en MIT App Inventor para conexión, lectura y parseo de datos  
- Integrar un sensor biomédico (pulso o temperatura) al ESP32 y transmitir sus lecturas  
- Visualizar y graficar datos en tiempo real en la app  
- Depurar errores de conexión y validar la transmisión de datos  

---

## 📁 Estructura del repositorio

Contiene 5 carpetas, cada una con un módulo práctico del taller:

- **01_configuración_entorno**  
- **02_app_inventor_básico**  
- **03_comunicación_esp32_app**  
- **04_integración_sensor**  
- **05_caso_estudio_monitor_vital**  

Cada carpeta incluye:  
- Código Arduino (`.ino`)  
- Proyecto MIT App Inventor exportado (`.aia`)  
- `README.md` con descripción técnica del módulo  
- Diagrama de conexión y capturas de pantalla de la app  

---

## 🧪 Fundamentos teóricos integrados

| Concepto    | Aplicación en el taller                                      |
|-------------|--------------------------------------------------------------|
| Wi-Fi / BLE | Protocolos de red para transferencia de datos biomédicos     |
| PWM         | Control de intensidad de señal para notificaciones hápticas  |
| ADC         | Conversión de señal analógica de sensores a valores digitales|
| JSON / CSV  | Formatos de datos para envío y parseo en App Inventor        |
| BLE GATT    | Servicios y características para lectura remota de sensores  |

---

## 🧬 Aplicaciones biomédicas relacionadas

- Telemetría de signos vitales en pacientes remotos  
- Sistemas portátiles de monitoreo continuo (wearables)  
- Dispositivos de biofeedback en rehabilitación  
- Interfaces móviles de alerta temprana para cuidados críticos  

---

## 🛠️ Requisitos para experimentar

- **Hardware**  
  - Placa ESP32 (e.g., ESP32-WROOM-32)  
  - Sensor de pulso óptico (MAX30100/MAX30102) o termistor (NTC)  
  - Cables Dupont, protoboard o PCB de pruebas  
  - Fuente de alimentación USB 5 V y opcional 6–12 V para periféricos  

- **Software**  
  - Arduino IDE ≥ 1.8.13 o VS Code + PlatformIO  
  - MIT App Inventor 2 (navegador o Companion App)  
  - Driver USB-Serial para ESP32  

- **Conectividad**  
  - Red Wi-Fi local disponible **o** dispositivo móvil con BLE  

---

## 🧑‍🏫 Créditos

Este material fue desarrollado para el curso **Fundamentos de Biodiseño**  
**Docentes**: Renzo Chan Ríos / Lewis De La Cruz  
**Universidad Peruana Cayetano Heredia (UPCH)** — 2025  
Versión: 1.0  

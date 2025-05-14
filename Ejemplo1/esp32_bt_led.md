# Ejemplo 1 – Controlar un LED por Bluetooth clásico (ESP32 + MIT App Inventor)

## Descripción rápida
Enciende y apaga un LED conectado al ESP32 desde una app Android que usa **Bluetooth clásico**.  
Ideal para tu “¡hola‑mundo!” de IoT inalámbrico.

---

## Materiales
| Cantidad | Componente |
|----------|------------|
| 1 | Módulo ESP32 DevKit V1 |
| 1 | LED 5 mm (cualquier color) |
| 1 | Resistencia 220 Ω |
| — | Cable micro‑USB y smartphone Android |

---

## Conexiones
```
GPIO 2 ──►|──┬── LED (ánodo)  
             │                      (cátodo)  
             └── Resistencia 220 Ω ──► GND
```

---

## Código Arduino (BluetoothSerial)

```cpp
#include "BluetoothSerial.h"
BluetoothSerial BT;          // BT clásico

const uint8_t LED_PIN = 2;

void setup() {
  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, LOW);
  BT.begin("ESP32_LED");     // Nombre visible por el teléfono
}

void loop() {
  if (BT.available()) {
    char c = BT.read();
    if (c == '1') digitalWrite(LED_PIN, HIGH);  // ON
    if (c == '0') digitalWrite(LED_PIN, LOW);   // OFF
  }
}
```

Carga el sketch y comprueba en el monitor serie el mensaje *“Bluetooth started”*.

---

## App Inventor – Pasos básicos

1. **Proyecto ➜ Nuevo proyecto** y escribe un nombre.
2. **Diseñador**  
   * Arrastra un `ListPicker` (renómbralo a *btnConectar*).  
   * Arrastra dos botones: *btnON* y *btnOFF*.  
   * Desde *Palette ➜ Connectivity* arrastra `BluetoothClient` (es invisible).
3. **Bloques**  
   * En `btnConectar.BeforePicking` usa `BluetoothClient1.AddressesAndNames` para llenar la lista.  
   * En `btnConectar.AfterPicking` llama `BluetoothClient1.Connect` al elemento seleccionado.  
   * En `btnON.Click` → `BluetoothClient1.SendText("1")`.  
   * En `btnOFF.Click` → `BluetoothClient1.SendText("0")`.
4. **Ejecuta** con la app *MIT AI2 Companion*, selecciona el dispositivo “ESP32_LED”, pulsa **ON/OFF** y observa el LED.

---

## Sugerencias
* Cambia `LED_PIN` si usas otra patilla.  
* Añade un `Label` para mostrar el estado de la conexión.

---


## Recursos para profundizar

- **Documentación oficial BluetoothClient – MIT App Inventor**  
  <https://ai2.appinventor.mit.edu/reference/components/connectivity.html#BluetoothClient>
- **Guía de inicio de MIT App Inventor (registro y pruebas en vivo)**  
  <https://appinventor.mit.edu/explore/get-started>
- **Tutorial paso a paso en video “Bluetooth LED Control App”**  
  <https://www.youtube.com/watch?v=w5LgLsCumFI>
- **Extensión BT avanzada (Bluetooth Extension) – Foro MIT AI2**  
  <https://community.appinventor.mit.edu/t/bt-an-extension-to-work-with-bluetooth/15379>

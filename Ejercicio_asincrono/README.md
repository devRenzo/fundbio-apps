# Ejercicio – Monitor de flexión en tiempo real con ESP32 y MIT App Inventor

Replicarás el proyecto “**ESP32 Web Server Based Real‑Time ADC**” de DOFBOT, pero sustituyendo el potenciómetro por **un flexómetro (sensor de flexión)** para medir ángulos o curvatura.  
El ESP32 publicará el valor analógico por Wi‑Fi y tu app mostrará un **gauge** y una **gráfica en vivo**.

---

## 1. Objetivos de aprendizaje
* Leer un **sensor resistivo** (flexómetro) con el ADC del ESP32 y calibrarlo.  
* Servir los datos en tiempo real mediante un **Web Server** (endpoint `/readADC`).  
* Consumir y visualizar esos datos en **MIT App Inventor 2** con WebViewer/Chart2D.  
* Practicar *voltage‑divider* y mapeo de valores (0–4095 → 0–3300 mV → ángulo °).  

---

## 2. Materiales requeridos

| Cant | Componente | Notas |
|------|------------|-------|
| 1 | ESP32 DevKit V1 | Cualquier módulo con ADC |
| 1 | **Flex sensor 2.2″** | Resistencia ~30 kΩ recto, ~80 kΩ doblado  |
| 1 | Resistor 47 kΩ ±5 % | Forma el divisor de tensión  |
| 1 | Protoboard + jumpers |  |
| 1 | Smartphone Android | Con App Inventor Companion |
| — | Fuente 5 V / USB | Para el ESP32 |

> El valor del resistor fijo debe ser cercano a la **resistencia media** del flex sensor para maximizar la resolución del divisor de tensión. 

---

## 3. Esquema de conexiones

```
3.3 V ─────┐
           │
           ├── Flex sensor (variable R_flex) ────► A0 (ADC)
           │
           └── 47 kΩ ───► GND
```

La salida del divisor genera `V_out = Vcc · R_fixed / (R_fixed + R_flex)`; al doblar el sensor, `R_flex ↑` y `V_out ↓` (o viceversa, según orientación). 

---

## 4. Firmware ESP32 (extracto)

```cpp
#include <WiFiManager.h>
#include <WebServer.h>

const int FLEX_PIN = A0;             // GPIO36 (ADC1_CH0)
WebServer server(80);

void handleADC(){
  int raw = analogRead(FLEX_PIN);          // 0–4095
  int mv  = map(raw, 0, 4095, 0, 3300);    // mV
  server.send(200,"text/plane",String(mv)); // plain text
}

void setup(){
  Serial.begin(115200);
  WiFiManager().autoConnect("FlexAP","12345678");
  server.on("/readADC", handleADC);
  server.begin();
}

void loop(){ server.handleClient(); }
```

*Basado en el código de DOFBOT (potenciómetro) adaptado al flexómetro.* 

### Calibrar a grados

Añade:

```cpp
int angle = map(mv, 2500, 3100, 0, 90); // ajusta puntos recto / doblado
```

Cambia los límites *después* de medir tus valores reales en monitor serie. 

---

## 5. App Inventor – bloques mínimos

| Componente | Uso |
|------------|-----|
| **WebViewer** | Carga la página del ESP32 y ejecuta el `window.AppInventor.setWebViewString(...)`  |
| **Chart2D / Canvas gauge** | Muestra gráfica en tiempo real y marcador de ángulo |
| **Clock** (200 ms) | Temporizador para refrescar si prefieres `Web.Get` |
| **Label** | Texto del valor mV / ° |

### Flujo de bloques

1. `Screen1.Initialize`  → establece `WebViewer1.Url` = `http://<ip_esp32>/`  
2. `WebViewer1.WebViewStringChanged`  
   * decodifica `get WebViewer1.WebViewString`  
   * actualiza `Label.Text`, `Gauge.Angle`, `Chart2D.AddEntry`  
3. (Opcional) Usa `Clock` para hacer `Web1.Get` a `/readADC` cuando no uses WebViewer. 

---

## 6. Pasos sugeridos

1. **Conecta** el flex sensor y verifica voltaje con multímetro recto y doblado.  
2. **Carga** el firmware y comprueba los valores crudos en monitor serie.  
3. **Ajusta** constantes de mapeo (`mv_min`, `mv_max`) para un rango 0–90°.  
4. **Diseña** tu UI: gauge circular (0–90 °) + gráfica histórica.  
5. **Demuestra** la app doblando el sensor lentamente; observa la respuesta.

---

## 7. Entregables para el alumno

* Vídeo corto (< 1 min) mostrando sensor y app funcionando.  
* Capturas de pantalla de bloques App Inventor.  
* Código `.ino` comentado (GitHub o ZIP).  
* Informe en PDF/Markdown explicando calibración y rango obtenido.

---

## 8. Rubrica de evaluación (20 ptos)

| Criterio | Puntos |
|----------|--------|
| Conexión y lectura estable del flex sensor | 5 |
| Servidor web funcional y accesible | 4 |
| App recibe datos en tiempo real (< 300 ms) | 4 |
| Visualización clara (gauge + gráfico) | 4 |
| Documentación y video demostración | 3 |

---


---

¡Listo! Con este ejercicio aprenderás a integrar **sensores analógicos**, servidores web embebidos y **MIT App Inventor** para crear dashboards móviles en tiempo real.


## Referencias

1. Blog original del proyecto: **ESP32 Web Server Based Real‑Time ADC with MIT App Inventor 2**  
   <https://www.dofbot.com/post/esp32-web-server-based-real-time-adc-with-mit-app-inventor2>

2. Video explicativo (YouTube): **Real‑Time ADC with ESP32 & App Inventor 2**  
   <https://youtu.be/4xLlHZ1Ak3U?si=mpXKIspBn0J9rZSm>

3. Datasheet Flex Sensor″ (SparkFun)  
   <https://cdn.sparkfun.com/assets/8/e/7/a/0/flex22.pdf>

4. Tutorial divisor de tensión (Electronics‑Tutorials)  
   <https://www.electronics-tutorials.ws/resistor/voltage-divider.html>

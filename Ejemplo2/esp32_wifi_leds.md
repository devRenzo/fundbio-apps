# Ejemplo 2 – Control de LEDs por Wi‑Fi (HTTP GET) desde App Inventor

## Objetivo
Crear un **servidor web** en el ESP32 con dos URLs (`/led1` y `/led2`) y una app Android que envía solicitudes **HTTP GET** para encender y apagar LEDs a través de la red local.

---

## Hardware
| # | Componente |
|---|------------|
| 1 | ESP32 DevKit V1 |
| 2 | LED + 220 Ω (GPIO 4 y GPIO 5) |
| — | Router Wi‑Fi / punto de acceso |

---

## Sketch Arduino (WebServer)

```cpp
#include <WiFi.h>
#include <WebServer.h>

const char* ssid = "TU_SSID";
const char* password = "TU_PASS";

WebServer server(80);

const int LED1 = 4;
const int LED2 = 5;

void handleRoot(){
  String html = "<h2>ESP32 WebServer</h2>";
  html += "<a href="/led1/on">LED 1 ON</a> | ";
  html += "<a href="/led1/off">LED 1 OFF</a><br>";
  html += "<a href="/led2/on">LED 2 ON</a> | ";
  html += "<a href="/led2/off">LED 2 OFF</a>";
  server.send(200,"text/html",html);
}

void setup(){
  pinMode(LED1,OUTPUT); pinMode(LED2,OUTPUT);
  WiFi.begin(ssid,password);
  while(WiFi.status()!=WL_CONNECTED){ delay(500); }
  Serial.println(WiFi.localIP());

  server.on("/",handleRoot);
  server.on("/led1/on",[]{ digitalWrite(LED1,HIGH); server.sendHeader("Location","/"); server.send(303); });
  server.on("/led1/off",[]{ digitalWrite(LED1,LOW);  server.sendHeader("Location","/"); server.send(303); });
  server.on("/led2/on",[]{ digitalWrite(LED2,HIGH); server.sendHeader("Location","/"); server.send(303); });
  server.on("/led2/off",[]{ digitalWrite(LED2,LOW);  server.sendHeader("Location","/"); server.send(303); });

  server.begin();
}

void loop(){ server.handleClient(); }
```

---

## App Inventor – Cómo enviar la petición

1. Arrastra un `Web` (renómbralo a *WebLED*).  
2. Crea cuatro botones (`LED1_ON`, `LED1_OFF`, `LED2_ON`, `LED2_OFF`).  
3. En los bloques usa:  
   ```blocks
   when LED1_ON.Click do
       set WebLED.Url to "http://192.168.X.X/led1/on"
       call WebLED.Get
   ```  
   (Repite para cada URL).  
4. Asegúrate de estar en la **misma red** que el ESP32.

---

## Pruebas
* Observa la IP en el monitor serie y cópiala en las URLs.  
* Si todo va bien, al pulsar cada botón el LED cambia de estado en <~100 ms.

---

## Nota
Este patrón se extiende fácilmente a sensores usando `WebLED.Get` para **leer** datos JSON que el ESP32 envíe en la respuesta.

---

## Recursos para profundizar

- **Documentación del componente Web – HTTP GET**  
  <https://ai2.appinventor.mit.edu/reference/components/connectivity.html#Web>
- **Tutorial “Control ESP32 over Internet using Android App” (MicrocontrollersLab)**  
  <https://microcontrollerslab.com/esp32-controller-android-mit-app-inventor/>
- **Ejemplo “Very Simple HTTP GET” – Foro MIT AI2**  
  <https://community.appinventor.mit.edu/t/very-simple-http-get-example-code/13023>
- **Referencia general de componentes Connectivity**  
  <https://ai2.appinventor.mit.edu/reference/components/connectivity.html>

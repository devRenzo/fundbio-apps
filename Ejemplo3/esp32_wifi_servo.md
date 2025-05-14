# Ejemplo 3 – Controlar un servo con *slider* vía Wi‑Fi (JSON + Web)

## Introducción
El deslizador de la app envía un valor (0 – 180) al ESP32 mediante una petición **HTTP POST con JSON**.  
El ESP32 mueve el servo a la posición solicitada y responde con el ángulo actual.

---

## Componentes
| Q | Parte |
|---|-------|
| 1 | ESP32 DevKit V1 |
| 1 | Microservo SG‑90 |
| 1 | Fuente 5 V (el servo NO se alimenta del 3 V3) |

---

## Conexión
```
ESP32 5 V  ──  Servo rojo (+5 V)  
ESP32 GND  ──  Servo marrón (GND)  
GPIO 13    ──  Servo naranja (PWM)
```

---

## Código Arduino

```cpp
#include <WiFi.h>
#include <WebServer.h>
#include <ArduinoJson.h>
#include <ESP32Servo.h>

const char* ssid="TU_SSID";
const char* password="TU_PASS";

WebServer server(80);
Servo myServo;
int angle = 90;   // posición inicial

void handlePost(){
  StaticJsonDocument<64> doc;
  DeserializationError err = deserializeJson(doc, server.arg("plain"));
  if(!err && doc.containsKey("angulo")){
    angle = constrain(doc["angulo"],0,180);
    myServo.write(angle);
  }
  StaticJsonDocument<32> res;
  res["ok"]=true; res["angulo"]=angle;
  String buf; serializeJson(res,buf);
  server.send(200,"application/json",buf);
}

void setup(){
  myServo.attach(13);
  myServo.write(angle);
  WiFi.begin(ssid,password);
  while(WiFi.status()!=WL_CONNECTED){ delay(400); }
  Serial.println(WiFi.localIP());

  server.on("/servo",HTTP_POST,handlePost);
  server.begin();
}

void loop(){ server.handleClient(); }
```

---

## App Inventor – Estructura

1. Componentes invisibles:  
   * `WebServo` (POST).  
   * `Clock1` (Interval 300 ms, habilitado **false**).
2. UI: un `Slider` (0–180) y un `Label` para mostrar el ángulo.
3. Bloques:
   ```blocks
   when Slider.PositionChanged do
       set globalObjetivo to Slider.ThumbPosition
       Clock1.TimerEnabled ← true

   when Clock1.Timer do
       set Clock1.TimerEnabled ← false
       set WebServo.Url to "http://192.168.X.X/servo"
       call WebServo.PostText
           to JSONEncode( create object with key "angulo" value globalObjetivo )
   when WebServo.GotText do
       set Label1.Text to get responseContent
   ```
   <https://json.org> muestra el formato; usa `JSONTextEncode` disponible en bloques.

---

## Buenas prácticas
* Envía el dato cada **300 ms** usando `Clock` para no saturar la red.  
* Alimenta el servo con 5 V → 500 mA para evitar *resets* del ESP32.

---



## Recursos para profundizar

- **Documentación del componente Web – HTTP POST y carga de datos**  
  <https://appinventor.mit.edu/explore/content/post-documentation.html>
- **Guía “Using Web APIs with JSON” – MIT App Inventor**  
  <https://ai2.appinventor.mit.edu/reference/other/json-web-apis.html>
- **Hilo “Need help with ESP32 Servo control app” – Comunidad MIT AI2**  
  <https://community.appinventor.mit.edu/t/need-help-with-esp32-servo-control-app/103011>
- **Video “App Inventor: Using Web APIs with JSON” (YouTube)**  
  <https://www.youtube.com/watch?v=y1dGXscDPMw>

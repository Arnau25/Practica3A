# PRÁCTICA 3A: GENERACIÓN DE UNA PAGINA WEB.

## APARTADO 1

#### CÓDIGO:

````
#include <WiFi.h>
#include <WebServer.h>

// SSID & Password

const char* ssid = "MIWIFI_2G_2jcE"; // Enter your SSID here
const char* password = "9GLX7t3u"; //Enter your Password here


WebServer server(80); // Object of WebServer(HTTP port, 80 is defult)
void handle_root(void);


void setup() {

  Serial.begin(115200);
  Serial.println("Try Connecting to ");
  Serial.println(ssid);
  // Connect to your wi-fi modem
  WiFi.begin(ssid, password);
  // Check wi-fi is connected to wi-fi network
  while (WiFi.status() != WL_CONNECTED) {
  delay(1000);
  Serial.print(".");
}
Serial.println("");
Serial.println("WiFi connected successfully");
Serial.print("Got IP: ");
Serial.println(WiFi.localIP()); //Show ESP32 IP on serial

server.on("/", handle_root);

server.begin();
Serial.println("HTTP server started");
delay(100);
}
void loop() {
  server.handleClient();
}
// HTML & CSS contents which display on web server
String HTML = "<!DOCTYPE html>\
<html>\
<body>\
<h1>My Primera Pagina con ESP32 - Station Mode &#128522;</h1>\
</body>\
</html>";
// Handle root url (/)
void handle_root() {
  server.send(200, "text/html", HTML);
}
````

#### FUNCIONAMIENTO

Para iniciar incluimos las dos librerías, las cuales son: #include <WiFi.h&gt> la cual sirve para que podamos conectar nuestra placa al internet, y la segunda #include <WebServer.h>, en esta creamos un servidor web.

Seguidamente declaramos un char para poner el SSID de nuestro WIFI y en otro la contaseña de este.

````
const char* ssid = "MIWIFI_2G_2jcE";
const char* password = "9GLX7t3u";
````
A continuacion en el void setup() imprimiremos por pantalla el intento de conexion:
Try connecting to: MIWIFI_2G_2jcE

Esto lo haremos utilizando el siguiente codigo:
````
Serial.println("Try Connecting to ");
Serial.println(ssid);
````
Una vez finalizado conectamos el modem WIFI a la placa con:
````
WiFi.begin(ssid, password);
````
Al hacerlo debemos comprobar si el WIFI esta conectado a la red, lo haremos mediante este código:
````
while (WiFi.status() != WL_CONNECTED) {
delay(1000);
Serial.print(".");
}
````
En este bucle estamos comprobando si esta conectado, y si lo está nos mostrará un punto por pantalla.

Seguidamente impirmimos por pantalla que el WIFI se ha conectado correctamente y tambien imprimiremos la IP del ESP32.

````
Serial.println("");
Serial.println("WiFi connected successfully");
Serial.print("Got IP: ");
Serial.println(WiFi.localIP());
````

Y por ultimo en el void setup() empezamos el servidor HTTP con:
````
server.begin();
Serial.println("HTTP server started");
delay(100);
````
En el Loop debemos llamar a la función handleClient() esta se encarga de recibir las peticiones de los clientes y lanzar las funciones de callback asociadas en el ruteo.
````
void loop() {
server.handleClient();
}
````

Por otro lado creamos una string del nombre HTML que son los contenidos de tipo HTML que se van a mostrar en el servidor web, en nuestro caso se mostrara: My Primera Pagina con ESP 32- Station mode.
````
String HTML = "<!DOCTYPE html&gt;\
<html&gt;\
<body&gt;\
<h1&gt;My Primera Pagina con ESP32 - Station Mode &#128522;</h1&gt;\
body&gt;\
html&gt;";
````
Y finalmente, creamos la función void handle_root() que se ha llamado anteriormente dentro del void setup(). Dentro de esta funcion send, la cual tiene tres parámetros: 
El primer parámetro es el código de respuesta (200,301,303,404...), el segundo es el tipo de contenido HTTP(text/plain, text/html, text/json, image/pns...) y el ultimo el contenido del cuerpo de la respuesta, que en nuestro caso es el string HTML.

````
void handle_root() {
server.send(200, "text/html", HTML);
}
````



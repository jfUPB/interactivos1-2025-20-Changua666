
# Evidencias de la unidad 5

## Actividad 1
### Describe cómo se están comunicando el micro:bit y el sketch de p5.js. ¿Qué datos envía el micro:bit?
El micro:bit y el sketch de p5.js se comunican mediante un enlace serie (UART), usando la interfaz WebSerial del navegador. El programa en el micro:bit toma en cada ciclo los valores de aceleración en los ejes X y Y junto con el estado de los botones A y B.

### ¿Cómo es la estructura del protocolo ASCII usado?
El protocolo usado entre el micro:bit y p5.js consiste en mensajes de texto en formato ASCII con una estructura tipo CSV (valores separados con comas). Cada linea contiene cuatro campos: La lectura del acelerometro en el eje y y el eje x, y la lectura de los botones a y b, en ese mismo orden separados por comas. 



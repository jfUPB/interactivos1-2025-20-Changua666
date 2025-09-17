
# Evidencias de la unidad 5

## Actividad 1
### Describe cómo se están comunicando el micro:bit y el sketch de p5.js. ¿Qué datos envía el micro:bit?
El micro:bit y el sketch de p5.js se comunican mediante un enlace serie (UART), usando la interfaz WebSerial del navegador. El programa en el micro:bit toma en cada ciclo los valores de aceleración en los ejes X y Y junto con el estado de los botones A y B.

### ¿Cómo es la estructura del protocolo ASCII usado?
El protocolo usado entre el micro:bit y p5.js consiste en mensajes de texto en formato ASCII con una estructura tipo CSV (valores separados con comas). Cada linea contiene cuatro campos: La lectura del acelerometro en el eje y y el eje x, y la lectura de los botones a y b, en ese mismo orden separados por comas. 

### Muestra y explica la parte del código de p5.js donde lee los datos del micro:bit y los transforma en coordenadas de la pantalla.

´´´
 if (port.availableBytes() > 0) {
      let data = port.readUntil("\n");
      if (data) {
        data = data.trim();
        let values = data.split(",");
        if (values.length == 4) {
          microBitX = int(values[0]) + windowWidth / 2;
          microBitY = int(values[1]) + windowHeight / 2;
          microBitAState = values[2].toLowerCase() === "true";
          microBitBState = values[3].toLowerCase() === "true";
          updateButtonStates(microBitAState, microBitBState);
        } else {
          print("No se están recibiendo 4 datos del micro:bit");
        }
´´´




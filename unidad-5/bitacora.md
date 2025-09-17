
# Evidencias de la unidad 5

## Actividad 1
### Describe cómo se están comunicando el micro:bit y el sketch de p5.js. ¿Qué datos envía el micro:bit?
El micro:bit y el sketch de p5.js se comunican mediante un enlace serie (UART), usando la interfaz WebSerial del navegador. El programa en el micro:bit toma en cada ciclo los valores de aceleración en los ejes X y Y junto con el estado de los botones A y B.

### ¿Cómo es la estructura del protocolo ASCII usado?
El protocolo usado entre el micro:bit y p5.js consiste en mensajes de texto en formato ASCII con una estructura tipo CSV (valores separados con comas). Cada linea contiene cuatro campos: La lectura del acelerometro en el eje y y el eje x, y la lectura de los botones a y b, en ese mismo orden separados por comas. 

### Muestra y explica la parte del código de p5.js donde lee los datos del micro:bit y los transforma en coordenadas de la pantalla.

```
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
```

1. La linea port.availableBytes() > 0 → pregunta si hay datos esperando en el puerto serial.
2. La linea port.readUntil("\n") → lee los datos hasta el salto de línea.
3. La linea data.split(",") → separa los valores recibidos en un arreglo. El micro:bit está mandando 4 valores separados por comas.
4. Los otros dos (values[2] y values[3]) son los estados de los botones A y B del micro:bit, convertidos en booleanos (true o false).

### ¿Cómo se generan los eventos A pressed y B released que se generan en p5.js a partir de los datos que envía el micro:bit?

El microbit envia true o false segun si el boton esta presionado o no, despues el skecth guarda el estado anterior en prevmicroBitAState y prevmicroBitBState.
Para detectar el evento A pressed 
```
if (newAState === true && prevmicroBitAState === false)
```
Eso significa que si el boton esta suelto es false, y si esta presionado es true. y en ese momento se cambia lineModuleSize a un valor aleatorio, guarda la posicion actual del mouse e imprime A pressed. 
Con el boton b es casi lo mismo, lo unico que cambia es que al presionar este se genera un nuevo color aleatorio e imprime B Pressed.

# Capturas

## Actividad 2
# Captura 1
<img width="1287" height="305" alt="Captura de pantalla 2025-09-17 145654" src="https://github.com/user-attachments/assets/fae0e7b9-4be4-4784-acb3-ad82d9876b00" />
Me aparece esto debido a que los datos se mandan como binario puro, no como texto ASCII. Como los bytes que se transmiten no corresponden a caracteres legibles, algunos se ven con ese caracter de ?.

# Captura 2 
<img width="1330" height="220" alt="Captura de pantalla 2025-09-17 150331" src="https://github.com/user-attachments/assets/6812d267-0d34-4630-88bb-974b6937633a" />















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
Cada mensaje que envía el programa ocupa 6 bytes, porque el formato '>2h2B' indica dos enteros cortos con signo de 16 bits (4 bytes en total) correspondientes a los valores del acelerómetro en los ejes X y Y, y dos enteros sin signo de 8 bits (2 bytes en total) que representan el estado de los botones A y B. En consecuencia, cada bloque de 6 bytes en el monitor serie contiene la información completa de una lectura: los primeros dos bytes codifican el valor de X, los siguientes dos el valor de Y, y los últimos dos reflejan si los botones están presionados o no.

En el formato '>2h2B' los dos primeros valores (xValue y yValue) se guardan como enteros cortos con signo de 16 bits, por lo que pueden ser positivos o negativos. Los positivos aparecen en hexadecimal con ceros a la izquierda (por ejemplo, 100 se representa como 00 64), mientras que los negativos se convierten a su equivalente en complemento a dos (por ejemplo, -100 se representa como FF 9C). Los dos últimos valores (aState y bState) son enteros sin signo de 8 bits, que solo pueden tomar los valores 00 o 01, dependiendo de si los botones A y B están presionados o no. De esta forma, cada mensaje incluye exactamente seis bytes: dos para xValue, dos para yValue y uno para cada botón, donde los enteros reflejan tanto movimientos positivos como negativos del acelerómetro.

# Captura 3 

<img width="1464" height="357" alt="Captura de pantalla 2025-09-17 153506" src="https://github.com/user-attachments/assets/d32b3c46-7b40-42ed-b2b3-f59171e81734" />

En el experimento se observa que los datos enviados en binario ocupan menos espacio y son más eficientes en términos de velocidad de transmisión, ya que cada valor numérico se representa directamente en bytes sin necesidad de convertirlo a caracteres. Sin embargo, tienen la desventaja de no ser legibles para las personas, lo que dificulta depurar o interpretar los mensajes sin herramientas adicionales. Por otro lado, el envío en ASCII resulta más claro y fácil de entender, ya que los valores aparecen en formato de texto legible, lo que facilita la verificación durante las pruebas; pero esta representación ocupa más bytes y puede hacer que la transmisión sea más lenta y menos eficiente en aplicaciones donde la velocidad y el ancho de banda son críticos.















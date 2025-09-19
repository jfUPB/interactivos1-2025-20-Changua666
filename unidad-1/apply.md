# Unidad 1


## 🛠 Fase: Apply
### Actividad 5 
Este sistema enlaza una micro:bit con una computadora para controlar un círculo en la pantalla mediante los botones físicos de la placa.

Al presionar los botones A o B, la micro:bit envía un comando por el puerto serial (a través del cable USB) hacia un programa desarrollado en p5.js. Dicho programa interpreta los comandos y desplaza el círculo en la pantalla hacia la izquierda o la derecha, según el botón que se haya pulsado.

### Actividad 6
Codigo del programa 
```
let port;
let connectBtn;
let connectionInitialized = false;
let x = 200; // Posición inicial del círculo

function setup() {
createCanvas(400, 400);
background(220);

port = createSerial();

connectBtn = createButton("Connect to micro:bit");
connectBtn.position(80, 300);
connectBtn.mousePressed(connectBtnClick);
}

function draw() {
background(220); // Limpiar pantalla cada frame

if (port.opened() && !connectionInitialized) {
port.clear();
connectionInitialized = true;
}

if (port.availableBytes() > 0) {
let dataRx = port.read(1); // Lee 1 byte
if (dataRx) {
let charRx = String.fromCharCode(dataRx[0]); // Convierte a carácter

if (charRx === 'L') {
x -= 10; // Mover a la izquierda
} else if (charRx === 'R') {
x += 10; // Mover a la derecha
}
}
}

x = constrain(x, 0, width); // Mantener dentro del canvas
ellipse(x, height / 2, 50, 50); // Dibujar el círculo
}

function connectBtnClick() {
if (!port.opened()) {
port.open("MicroPython", 115200);
connectionInitialized = false;
} else {
port.close();
}
}
```
Codigo del microbit 
```
from microbit import *

uart.init(baudrate=115200)

while True:
    if button_a.is_pressed():
        uart.write('L')  # Left
        sleep(150)
    elif button_b.is_pressed():
        uart.write('R')  # Right
        sleep(150)
```

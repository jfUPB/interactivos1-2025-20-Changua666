# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 4 
[Link Del Programa](https://editor.p5js.org/Changua666/sketches/Xkow2L6QY)

**Codigo**
```
function setup() {
  createCanvas(windowWidth, windowHeight);
  noStroke();
}

function draw() {
  background(20, 30, 40, 50); // Fondo con transparencia para dejar trazas

  // Dibujar muchos círculos en posiciones aleatorias con sin/cos
  for (let i = 0; i < 200; i++) {
    let angle = random(TWO_PI);      // Ángulo aleatorio
    let r = random(width / 3);       // Radio aleatorio
    let x = width / 2 + cos(angle) * r;
    let y = height / 2 + sin(angle) * r;

    let sz = random(5, 20);          // Tamaño aleatorio
    let c = color(
      150 + 105 * sin(frameCount * 0.01 + i), // rojo varía con sin
      150 + 105 * cos(frameCount * 0.01 + i), // verde varía con cos
      random(200, 255),                       // azul aleatorio
      180
    );
    fill(c);
    ellipse(x, y, sz, sz);
  }
}
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4b4ef09c-6c4f-47e6-9f3f-a0ea9f1ed583" />

### Actividad 5
El sistema físico interactivo funciona así: el micro:bit detecta cuando se presiona el botón A y, mediante comunicación serial a 115200 baudios, envía constantemente un mensaje al computador —la letra “A” si el botón está presionado y “N” si no lo está—; en el navegador, un programa hecho en p5.js con la librería p5.webserial recibe estos datos y dibuja en cada frame un cuadrado cuyo color cambia según el mensaje recibido, rojo si llega “A” y verde si llega “N”, de modo que la acción física de presionar el botón se traduce en una respuesta visual inmediata en pantalla, integrando hardware y software en un solo ciclo de interacción.


### Actividad 6

[Link Del Programa](https://editor.p5js.org/Changua666/sketches/HV_4Jt-I6)

 Codigo de P5.js 
 ```
let port;
let connectBtn;
let connectionInitialized = false;

let circleX; // posición en x del círculo

function setup() {
  createCanvas(400, 400);
  circleX = width / 2; // empieza en el centro

  // Configurar conexión serial
  port = createSerial();

  // Botón de conexión
  connectBtn = createButton("Connect to micro:bit");
  connectBtn.position(10, height + 10);
  connectBtn.mousePressed(connectBtnClick);
}

function draw() {
  background(220);

  // Inicializar conexión limpia al abrir puerto
  if (port.opened() && !connectionInitialized) {
    port.clear();
    connectionInitialized = true;
  }

  // Leer datos del micro:bit
  if (port.availableBytes() > 0) {
    let dataRx = port.read(1);
    if (dataRx == "A") {
      circleX -= 5; // mover izquierda
    } else if (dataRx == "B") {
      circleX += 5; // mover derecha
    }
  }

  // Limitar círculo dentro del canvas
  circleX = constrain(circleX, 25, width - 25);

  // Dibujar círculo
  fill(100, 150, 255);
  ellipse(circleX, height / 2, 50, 50);

  // Actualizar estado del botón de conexión
  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
  } else {
    connectBtn.html("Disconnect");
  }
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

Codigo microbit 
```
let port;
let connectBtn;
let connectionInitialized = false;

let circleX; // posición en x del círculo

function setup() {
  createCanvas(400, 400);
  circleX = width / 2; // empieza en el centro

  // Configurar conexión serial
  port = createSerial();

  // Botón de conexión
  connectBtn = createButton("Connect to micro:bit");
  connectBtn.position(10, height + 10);
  connectBtn.mousePressed(connectBtnClick);
}

function draw() {
  background(220);

  // Inicializar conexión limpia al abrir puerto
  if (port.opened() && !connectionInitialized) {
    port.clear();
    connectionInitialized = true;
  }

  // Leer datos del micro:bit
  if (port.availableBytes() > 0) {
    let dataRx = port.read(1);
    if (dataRx == "A") {
      circleX -= 5; // mover izquierda
    } else if (dataRx == "B") {
      circleX += 5; // mover derecha
    }
  }

  // Limitar círculo dentro del canvas
  circleX = constrain(circleX, 25, width - 25);

  // Dibujar círculo
  fill(100, 150, 255);
  ellipse(circleX, height / 2, 50, 50);

  // Actualizar estado del botón de conexión
  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
  } else {
    connectBtn.html("Disconnect");
  }
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






# Evidencias de la unidad 4

## Código

[Enlace a la aplicación a modificar](https://editor.p5js.org/generative-design/sketches/P_2_0_03)

Código a modificar:

``` js
// P_2_0_03
//
// Generative Gestaltung – Creative Coding im Web
// ISBN: 978-3-87439-902-9, First Edition, Hermann Schmidt, Mainz, 2018
// Benedikt Groß, Hartmut Bohnacker, Julia Laub, Claudius Lazzeroni
// with contributions by Joey Lee and Niels Poldervaart
// Copyright 2018
//
// http://www.generative-gestaltung.de
//
// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.

/**
 * drawing with a changing shape by draging the mouse.
 *
 * MOUSE
 * position x          : length
 * position y          : thickness and number of lines
 * drag                : draw
 *
 * KEYS
 * 1-3                 : stroke color
 * del, backspace      : erase
 * s                   : save png
 */
'use strict';

var strokeColor;

function setup() {
  createCanvas(720, 720);
  colorMode(HSB, 360, 100, 100, 100);
  noFill();
  strokeWeight(2);
  strokeColor = color(0, 10);
}

function draw() {
  if (mouseIsPressed && mouseButton == LEFT) {
    push();
    translate(width / 2, height / 2);

    var circleResolution = int(map(mouseY + 100, 0, height, 2, 10));
    var radius = mouseX - width / 2;
    var angle = TAU / circleResolution;

    stroke(strokeColor);

    beginShape();
    for (var i = 0; i <= circleResolution; i++) {
      var x = cos(angle * i) * radius;
      var y = sin(angle * i) * radius;
      vertex(x, y);
    }
    endShape();

    pop();
  }
}

function keyReleased() {
  if (keyCode == DELETE || keyCode == BACKSPACE) background(0, 0, 100);
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');

  if (key == '1') strokeColor = color(0, 10);
  if (key == '2') strokeColor = color(192, 100, 64, 10);
  if (key == '3') strokeColor = color(52, 100, 71, 10);
}

```

[Enlace a la aplicación modificada](https://editor.p5js.org/Changua666/sketches/ix_tBqc6g)

Código modificado:

``` js
// P_2_0_03_Microbit
// Adaptación del sketch original para recibir datos de un micro:bit
// y controlar el dibujo con un botón de conexión.

// Variables para la conexión serie
let port;
let connectBtn;
let connectionInitialized = false;

// Variables para el dibujo
var strokeColor;

// Variables para los datos del micro:bit
var xValue = 0;
var yValue = 0;
var aState = false;
var bState = false;

function setup() {
  createCanvas(720, 720);
  colorMode(HSB, 360, 100, 100, 100);
  noFill();
  strokeWeight(2);
  strokeColor = color(0, 10);
  background(0, 0, 100); // Fondo inicial blanco

  // Funciones de conexión de p5.serial
  port = createSerial();
  connectBtn = createButton("Conectar a micro:bit");
  connectBtn.position(width - 200, height - 50);
  connectBtn.mousePressed(connectBtnClick);
  
  // Agregar un listener para recibir datos
  // Corrección del error: se usa port.onData() en lugar de port.on()

  console.log("Haz clic en 'Conectar a micro:bit' para iniciar.");
}

function draw() {
  // La lógica del botón en el bucle draw
  if (!port.opened()) {
    connectBtn.html("Conectar a micro:bit");
  } else {
    connectBtn.html("Desconectar");
    
    // El dibujo se activa si el micro:bit se mueve
    if (abs(xValue) > 50 || abs(yValue) > 50) {
      push();
      translate(width / 2, height / 2);

      // Mapeo de los valores del micro:bit a las variables de dibujo
      var circleResolution = int(map(yValue, -1024, 1024, 2, 20));
      var radius = map(xValue, -1024, 1024, 50, width / 2);

      var angle = TAU / circleResolution;
      stroke(strokeColor);

      beginShape();
      for (var i = 0; i <= circleResolution; i++) {
        var x = cos(angle * i) * radius;
        var y = sin(angle * i) * radius;
        vertex(x, y);
      }
      endShape();
      pop();
    }
  }
}

// Función que se llama cuando se reciben datos del puerto serie
function serialEvent() {
  // Leer la cadena completa hasta el salto de línea
  let inString = port.readUntil('\n');
  
  if (inString.length > 0) {
    inString = trim(inString);
    let sensors = split(inString, ',');
    
    // Asigna los valores del micro:bit a las variables
    if (sensors.length === 4) {
      xValue = int(sensors[0]);
      yValue = int(sensors[1]);
      aState = (sensors[2] === 'True');
      bState = (sensors[3] === 'True');
      
      // Control de borrado y color con los botones
      if (aState) {
          background(0, 0, 100);
      }
      if (bState) {
          // Cambia el color del trazo con el botón B
          let hue = map(xValue, -1024, 1024, 0, 360);
          strokeColor = color(hue, 100, 71, 10);
      }
    }
  }
}

// Función de manejo del botón de conexión
function connectBtnClick() {
  if (!port.opened()) {
    port.open("MicroPython", 115200);
    connectionInitialized = false;
  } else {
    port.close();
  }
}

```

## Video

[Video demostratativo](URL)




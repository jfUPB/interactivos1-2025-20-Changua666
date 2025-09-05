# Evidencias de la unidad 4

## Código

[Enlace a la aplicación a modificar](https://editor.p5js.org/generative-design/sketches/P_2_0_02)

Código a modificar:

``` js
// P_2_0_02
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
 * del, backspace      : erase
 * s                   : save png
 */
'use strict';

function setup() {
  createCanvas(720, 720);
  noFill();
  background(255);
  strokeWeight(2);
  stroke(0, 25);
}

function draw() {
  if (mouseIsPressed && mouseButton == LEFT) {
    push();
    translate(width / 2, height / 2);

    var circleResolution = int(map(mouseY + 100, 0, height, 2, 10));
    var radius = mouseX - width / 2;
    var angle = TAU / circleResolution;

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
  if (keyCode == DELETE || keyCode == BACKSPACE) background(255);
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');
}


```

[Enlace a la aplicación modificada](https://editor.p5js.org/Changua666/sketches/ix_tBqc6g)

Código modificado:

``` js

let port;
let reader;
let buffer = '';

let xValue = 0;
let yValue = 0;
let aState = false;
let bState = false;

const connectButton = document.getElementById('connectButton');

connectButton.addEventListener('click', async () => {
  try {
    port = await navigator.serial.requestPort();
    await port.open({ baudRate: 115200 });

    const decoder = new TextDecoderStream();
    port.readable.pipeTo(decoder.writable);
    const inputStream = decoder.readable;

    reader = inputStream.getReader();

    readLoop();

    connectButton.disabled = true;
    connectButton.textContent = "Conectado";
  } catch (e) {
    console.error('Error abriendo puerto serial:', e);
  }
});

async function readLoop() {
  let textBuffer = '';

  while (true) {
    const { value, done } = await reader.read();
    if (done) {
      reader.releaseLock();
      break;
    }
    textBuffer += value;
    let lines = textBuffer.split('\n');
    textBuffer = lines.pop();

    for (const line of lines) {
      if (!line.trim()) continue;
      let parts = line.trim().split(',');
      if (parts.length === 4) {
        xValue = parseInt(parts[0]);
        yValue = parseInt(parts[1]);
        aState = parts[2].toLowerCase() === 'true';
        bState = parts[3].toLowerCase() === 'true';
      }
    }
  }
}

function setup() {
  createCanvas(720, 720);
  colorMode(HSB, 360, 100, 100, 100);
  noFill();
  strokeWeight(2);
  strokeColor = color(0, 10);
  background(0, 0, 100);
}

function draw() {
  if (aState || bState) {
    push();
    translate(width / 2, height / 2);

    let circleResolution = int(map(yValue + 1024, 0, 2048, 2, 10));
    circleResolution = constrain(circleResolution, 2, 10);

    let radius = map(xValue + 1024, 0, 2048, 0, width / 2);

    let angle = TWO_PI / circleResolution;

    stroke(strokeColor);

    beginShape();
    for (let i = 0; i <= circleResolution; i++) {
      let x = cos(angle * i) * radius;
      let y = sin(angle * i) * radius;
      vertex(x, y);
    }
    endShape();

    pop();
  }
}

function keyReleased() {
  if (keyCode === DELETE || keyCode === BACKSPACE) background(0, 0, 100);
  if (key === 's' || key === 'S') saveCanvas('microbit-drawing', 'png');

  if (key === '1') strokeColor = color(0, 10);
  if (key === '2') strokeColor = color(192, 100, 64, 10);
  if (key === '3') strokeColor = color(52, 100, 71, 10);
}


```

## Video

[Video demostratativo](https://youtu.be/I_GuWhsOWYw?si=PxwmjEAt_HW254rC)








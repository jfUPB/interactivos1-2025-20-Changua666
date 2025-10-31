
# Evidencias de la unidad 8

## Actividad 1 
Para el diseño de la actividad me base en dos cosas, las visuales que usa el artista rusowsky en sus conciertos, y en la portada de su album "DAISY" 
[Ejemplo](https://www.tiktok.com/@pauledss/video/7554777796820749590?is_from_webapp=1&sender_device=pc&web_id=7559745528935859723)

<img width="610" height="610" alt="image" src="https://github.com/user-attachments/assets/d67642a5-ecc0-45d6-8d5c-204a891a32d4" />

Las visuales que quiero crear son un gif que aparece en la mitad, unas visualizadores del sonido detras de este (en el fondo) y con el microbit quiero poder controlar la intensidad de las visuales de atras mediante los botones a y b.

Con el celular se controlaran los gifs y ya. 

un boceto inicial que tuve del controlador del celular fue este 
<img width="720" height="1280" alt="image" src="https://github.com/user-attachments/assets/5cf4854f-e422-4a2e-859b-851a1c16c487" />

Pero ya despues deicidi que era mejor tener el controlador de intesidad con los botones a y b del microbit y lo descarte

# Diagrama 
<img width="1021" height="604" alt="image" src="https://github.com/user-attachments/assets/3f81c4fe-fd2a-4967-81b9-99a3ef5b7e8a" />

## Actividad 2 
# Proceso de construccion 

Primero decidi que era lo que queria mostrar (en este caso, los gifs y las visuales de la cancion atras de estos). 
Entonces primero le pregunte a chatgpt como puedo hacer un Ecualizador funcional para eso y lo cree, despues añadi la funcion del gif y esta fue la "primera version"
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e9e57d36-8c4c-425a-86c7-f45f1b4d6247" />
ya con esta base de sketch, me propuse a crear la conexion con el celular, el chat con chatgpt no lo encuentro pero basicamente cambie las funcionalidades que tenia en las teclas para que estuvieran en el touch del celular y ya despues le meti el slider de intensidad.

Ya despues hice la conexion con el microbit, que si fue mas luchada.
Primero le explique a la ia lo que queria hacer y le mande unos codigos de ejemplo de unidades anteriores, despues de eso por alguna razon no me aparecia nada en la pantalla del celular y si me toco corregir eso manualmente, despues de eso me di cuenta de que al darle click no se conectaba el microbit y literlamente estuve 1 hora tratando de solucionar eso con chagpt 


## Codigos Desktop
index.html

```
  <!DOCTYPE html>
  <html lang="es">
    <head>
      <meta charset="utf-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>Visuales Desktop</title>

      <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.6.0/p5.min.js"></script>
      <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.6.0/addons/p5.sound.min.js"></script>

    <script src="https://unpkg.com/p5.serialport/lib/p5.serialport.js"></script> 
  <script src="http://localhost:3000/socket.io/socket.io.js"></script>
  <script src="sketch.js"></script>

      <style>
        html, body {
          margin: 0;
          padding: 0;
          overflow: hidden;
          background: black;
        }
        canvas {
          display: block;
        }
      </style>
    </head>

    <body>
      </body>
  </html>
```

sketch.js

```
// public/desktop/sketch.js (CÓDIGO FINAL LIMPIO Y FUNCIONAL)

// === Variables Globales ===
let socket;
let mobileIntensity = 1; // Controlada por el Micro:bit (via Socket.IO)
let mobileMode = 1;      
let mobileGif = 0;       

let song, fft;
let gifElements = []; // Arreglo para los elementos DOM del GIF (para que se muevan)
let audioStarted = false; 
let startMusicButton; 

// === Precarga de Assets ===
function preload() {
    song = loadSound('../assets/malibu.mp3');
}


// === Setup del Canvas y Socket.IO ===
function setup() {
    createCanvas(windowWidth, windowHeight);
    noStroke();
    fft = new p5.FFT();
    textAlign(CENTER, CENTER);
    textSize(18);
    
    // 🔑 INICIALIZACIÓN DE BOTÓN (Solo Audio)
    startMusicButton = createButton('1. INICIAR MÚSICA');
    startMusicButton.position(width / 2 - 100, height - 70); 
    startMusicButton.size(200, 40);
    startMusicButton.style('background-color', '#fff');
    startMusicButton.mousePressed(startAudioContext);
    
    // 🔑 Carga los GIFs como Elementos DOM
    gifElements[0] = createImg('../assets/gorila.gif', 'Gorila');
    gifElements[1] = createImg('../assets/gorila-spin.gif', 'Gorila Spin');
    gifElements[2] = createImg('../assets/mico-baile.gif', 'Mico Baile');
    gifElements[3] = createImg('../assets/mico-danza.gif', 'Mico Danza');
    
    // Oculta todos los elementos GIF DOM al inicio
    for (let i = 0; i < gifElements.length; i++) {
        gifElements[i].hide();
    }
    
    // 🔌 Conexión a Socket.IO
    socket = io.connect(window.location.origin);
    
    // 🎧 Escuchar comandos del móvil (Modo y GIF)
    socket.on('visualControl', (data) => {
        mobileMode = data.newMode;
        mobileGif = data.newGif;
    });
    
    // 🌟 ESCUCHAR DATOS DEL MICRO:BIT DESDE EL SERVIDOR
    socket.on('microbitData', (data) => {
        let rawData = data.value;
        if (rawData.includes('intensity:')) {
            let value = parseFloat(rawData.split(':')[1]);
            if (!isNaN(value)) {
                mobileIntensity = constrain(value, 0.5, 5.0); 
            }
        }
    });
}

// 🔑 FUNCIÓN: Inicia el AudioContext
function startAudioContext() {
    userStartAudio(); 
    
    if (getAudioContext().state !== 'suspended' && !audioStarted) { 
        song.loop();
        audioStarted = true; 
        startMusicButton.attribute('disabled', '');
        startMusicButton.html('MÚSICA INICIADA ✅');
        console.log("AudioContext reanudado y música iniciada.");
    }
}

// === Bucle Principal de Dibujo ===
function draw() {
    background(248,255,3); 
    let spectrum = fft.analyze();
    let intensity = mobileIntensity; 

    // Selector de modo visual
    if (mobileMode === 1) { 
        drawSpectrumBars(spectrum, intensity);
    } else if (mobileMode === 2) {
        drawCircleSpectrum(spectrum, intensity);
    } else if (mobileMode === 3) {
        drawTriangleSpectrum(spectrum, intensity);
    } else if (mobileMode === 4) {
        drawSquareSpectrum(spectrum, intensity);
    }
    
    // 🚨 LÓGICA DE GIF CORREGIDA: Muestra solo el GIF seleccionado
    for (let i = 0; i < gifElements.length; i++) {
        if (i === mobileGif) {
            gifElements[i].show();
            // Asegúrate de que está centrado y con el tamaño correcto
            gifElements[i].position(width / 2 - 150, height / 2 - 150);
            gifElements[i].size(300, 300);
        } else {
            gifElements[i].hide();
        }
    }

    fill(0);
    text('MALIBU', width / 2, 40);
    text('Intensidad de las visuales: ' + intensity.toFixed(1) + ' (Control Micro:bit)', width / 2, 70); 
    text('Modo: ' + mobileMode + ' | GIF: ' + mobileGif, width / 2, 100); 
    
    if (!audioStarted) {
        text('Presiona "1. INICIAR MÚSICA" para comenzar.', width / 2, height - 20);
    } else {
        text('Música OK. Presiona A/B en el Micro:bit para ajustar la intensidad de las visuales.', width / 2, height - 20);
    }
}

// === FUNCIONES DE VISUALES ===

function drawSpectrumBars(spectrum, intensity) {
    let low = spectrum.slice(0, spectrum.length / 3);
    let mid = spectrum.slice(spectrum.length / 3, (2 * spectrum.length) / 3);
    let high = spectrum.slice((2 * spectrum.length) / 3);
    
    drawBarBand(low, intensity);
    drawBarBand(mid, intensity);
    drawBarBand(high, intensity);
}

function drawBarBand(spectrum, intensity) {
    fill(0);
    let step = 15;
    let bandWidth = (width / (spectrum.length / step)) * 0.8;
    for (let i = 0; i < spectrum.length; i += step) {
        let x = map(i, 0, spectrum.length, 0, width);
        let h = -height + map(spectrum[i] * intensity, 0, 255, height, 0);
        rect(x, height, bandWidth, h, 5);
    }
}

function drawCircleSpectrum(spectrum, intensity) {
    noFill();
    strokeWeight(3);
    stroke(0);
    push();
    translate(width / 2, height / 2);
    beginShape();
    for (let i = 0; i < spectrum.length; i += 10) {
        let angle = map(i, 0, spectrum.length, 0, TWO_PI);
        let r = map(spectrum[i] * intensity, 0, 255, 150, 400);
        let x = r * cos(angle);
        let y = r * sin(angle);
        vertex(x, y);
    }
    endShape(CLOSE);
    pop();
    noStroke();
}

function drawTriangleSpectrum(spectrum, intensity) {
    fill(0,0,0);
    noStroke();
    beginShape();
    for (let i = 0; i < spectrum.length; i += 8) {
        let x = map(i, 0, spectrum.length, 0, width);
        let y = height / 2 + map(spectrum[i] * intensity, 0, 255, 100, -100);
        vertex(x, y);
    }
    vertex(width, height);
    vertex(0, height);
    endShape(CLOSE);
}
    
function drawSquareSpectrum(spectrum, intensity) {
    fill(0,0,0);
    let step = 25;
    for (let i = 0; i < spectrum.length; i += step) {
        let x = map(i, 0, spectrum.length, 0, width);
        let size = map(spectrum[i] * intensity, 0, 255, 10, 120);
        rect(x, height / 2 - size / 2, size, size);
    }
}

function windowResized() {
    resizeCanvas(windowWidth, windowHeight);
    // Reposicionar el botón y el GIF al cambiar el tamaño de la ventana
    startMusicButton.position(width / 2 - 100, height - 70);
    // Reposicionar el GIF actualmente visible
    if (gifElements[mobileGif]) {
        gifElements[mobileGif].position(width / 2 - 150, height / 2 - 150);
    }
}
```

## Mobile 
index.html
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Visuals Mobile Control</title>
    <script src="/socket.io/socket.io.js"></script> 
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.9.0/p5.min.js"></script>
    <style>
        body { margin: 0; overflow: hidden; background-color: #f8ff03; }
        canvas { display: block; }
    </style>
</head>
<body>
    <script src="sketch.js"></script> 
</body>
</html>
```
sketch.js
```
// public/mobile/sketch.js (CÓDIGO FINAL Y FUNCIONAL)

let socket;

// Variables de estado
let currentMode = 1; 
let currentGif = 0;

// Arrays para almacenar los botones
let modeButtons = [];
let gifButtons = [];

function setup() {
    createCanvas(windowWidth, windowHeight);
    background(248,255,3); 
    
    socket = io.connect(window.location.origin);

    // === Variables de Diseño ===
    const padding = 20; 
    const spacing = 10; 
    const buttonHeight = 50;
    const buttonWidth = (width - 3 * padding) / 2;
    
    // --- 1. Botones de Modos Visuales ---
    let currentY = padding; 
    let modeLabels = ['1: BARRAS', '2: CÍRCULO', '3: TRIÁNGULO', '4: CUADRADO'];
    
    for (let i = 0; i < 4; i++) {
        let x = padding + (i % 2) * (buttonWidth + padding);
        let y = currentY + floor(i / 2) * (buttonHeight + spacing);
        
        let button = createButton(modeLabels[i]);
        button.size(buttonWidth, buttonHeight);
        button.position(x, y);
        button.mousePressed(() => {
            currentMode = i + 1;
            updateButtonStyles(modeButtons, currentMode);
            sendControlData();
        });
        modeButtons.push(button);
    }
    
    // --- 2. Botones de GIFs ---
    // Posición debajo de los botones de modo
    currentY += 2 * (buttonHeight + spacing) + spacing; 
    let gifLabels = ['GORILA 1', 'GORILA 2', 'MICO 1', 'MICO 2'];
    
    for (let i = 0; i < 4; i++) {
        let x = padding + (i % 2) * (buttonWidth + padding);
        let y = currentY + floor(i / 2) * (buttonHeight + spacing);
        
        let button = createButton(gifLabels[i]);
        button.size(buttonWidth, buttonHeight);
        button.position(x, y);
        button.mousePressed(() => {
            currentGif = i;
            updateButtonStyles(gifButtons, currentGif);
            sendControlData();
        });
        gifButtons.push(button);
    }
    
    // Inicializar estilos de botones
    updateButtonStyles(modeButtons, currentMode);
    updateButtonStyles(gifButtons, currentGif);
    
    sendControlData();
}

function draw() {
    // CORRECCIÓN: backgroundrgba no es válido. Debe ser background().
    background(248, 255, 3);
    fill(0);
    textAlign(CENTER, TOP); // CENTRADO para el mensaje de guía
    textSize(18);
    // Posición del mensaje de guía
    text('INTENSIDAD: Controlada por Micro:bit (A/B)', width / 2, height - 30);
}

// --- FUNCIONES DE UTILIDAD ---

function updateButtonStyles(buttonsArray, activeValue) {
    buttonsArray.forEach((btn, index) => {
        let value = (buttonsArray === modeButtons) ? (index + 1) : index;
        if (value === activeValue) {
            btn.style('background-color', '#000');
            btn.style('color', '#f8ff03');
            btn.style('border', '3px solid #000');
        } else {
            btn.style('background-color', '#f0f0f0');
            btn.style('color', '#000');
            btn.style('border', '2px solid #000');
        }
    });
}

function sendControlData() {
    let data = {
        newMode: currentMode,
        newGif: currentGif,
    };
    // 🚨 CORRECCIÓN: Usar 'visualControl' para coincidir con lo que escucha el servidor
    socket.emit('visualControl', data);
}

function windowResized() {
    // Volver a calcular posiciones al cambiar el tamaño
    const padding = 20;
    const spacing = 10;
    const buttonHeight = 50;
    const buttonWidth = (width - 3 * padding) / 2;

    let currentY = padding; 
    
    // Reposicionar Modos
    for (let i = 0; i < modeButtons.length; i++) {
        let x = padding + (i % 2) * (buttonWidth + padding);
        let y = currentY + floor(i / 2) * (buttonHeight + spacing);
        modeButtons[i].size(buttonWidth, buttonHeight);
        modeButtons[i].position(x, y);
    }

    // Reposicionar GIFs
    currentY += 2 * (buttonHeight + spacing) + spacing;
    for (let i = 0; i < gifButtons.length; i++) {
        let x = padding + (i % 2) * (buttonWidth + padding);
        let y = currentY + floor(i / 2) * (buttonHeight + spacing);
        gifButtons[i].size(buttonWidth, buttonHeight);
        gifButtons[i].position(x, y);
    }
    
    resizeCanvas(windowWidth, windowHeight);
}
```
## Server

```
// public/mobile/sketch.js (CÓDIGO FINAL Y FUNCIONAL)

let socket;

// Variables de estado
let currentMode = 1; 
let currentGif = 0;

// Arrays para almacenar los botones
let modeButtons = [];
let gifButtons = [];

function setup() {
    createCanvas(windowWidth, windowHeight);
    background(248,255,3); 
    
    socket = io.connect(window.location.origin);

    // === Variables de Diseño ===
    const padding = 20; 
    const spacing = 10; 
    const buttonHeight = 50;
    const buttonWidth = (width - 3 * padding) / 2;
    
    // --- 1. Botones de Modos Visuales ---
    let currentY = padding; 
    let modeLabels = ['1: BARRAS', '2: CÍRCULO', '3: TRIÁNGULO', '4: CUADRADO'];
    
    for (let i = 0; i < 4; i++) {
        let x = padding + (i % 2) * (buttonWidth + padding);
        let y = currentY + floor(i / 2) * (buttonHeight + spacing);
        
        let button = createButton(modeLabels[i]);
        button.size(buttonWidth, buttonHeight);
        button.position(x, y);
        button.mousePressed(() => {
            currentMode = i + 1;
            updateButtonStyles(modeButtons, currentMode);
            sendControlData();
        });
        modeButtons.push(button);
    }
    
    // --- 2. Botones de GIFs ---
    // Posición debajo de los botones de modo
    currentY += 2 * (buttonHeight + spacing) + spacing; 
    let gifLabels = ['GORILA 1', 'GORILA 2', 'MICO 1', 'MICO 2'];
    
    for (let i = 0; i < 4; i++) {
        let x = padding + (i % 2) * (buttonWidth + padding);
        let y = currentY + floor(i / 2) * (buttonHeight + spacing);
        
        let button = createButton(gifLabels[i]);
        button.size(buttonWidth, buttonHeight);
        button.position(x, y);
        button.mousePressed(() => {
            currentGif = i;
            updateButtonStyles(gifButtons, currentGif);
            sendControlData();
        });
        gifButtons.push(button);
    }
    
    // Inicializar estilos de botones
    updateButtonStyles(modeButtons, currentMode);
    updateButtonStyles(gifButtons, currentGif);
    
    sendControlData();
}

function draw() {
    // CORRECCIÓN: backgroundrgba no es válido. Debe ser background().
    background(248, 255, 3);
    fill(0);
    textAlign(CENTER, TOP); // CENTRADO para el mensaje de guía
    textSize(18);
    // Posición del mensaje de guía
    text('INTENSIDAD: Controlada por Micro:bit (A/B)', width / 2, height - 30);
}

// --- FUNCIONES DE UTILIDAD ---

function updateButtonStyles(buttonsArray, activeValue) {
    buttonsArray.forEach((btn, index) => {
        let value = (buttonsArray === modeButtons) ? (index + 1) : index;
        if (value === activeValue) {
            btn.style('background-color', '#000');
            btn.style('color', '#f8ff03');
            btn.style('border', '3px solid #000');
        } else {
            btn.style('background-color', '#f0f0f0');
            btn.style('color', '#000');
            btn.style('border', '2px solid #000');
        }
    });
}

function sendControlData() {
    let data = {
        newMode: currentMode,
        newGif: currentGif,
    };
    // 🚨 CORRECCIÓN: Usar 'visualControl' para coincidir con lo que escucha el servidor
    socket.emit('visualControl', data);
}

function windowResized() {
    // Volver a calcular posiciones al cambiar el tamaño
    const padding = 20;
    const spacing = 10;
    const buttonHeight = 50;
    const buttonWidth = (width - 3 * padding) / 2;

    let currentY = padding; 
    
    // Reposicionar Modos
    for (let i = 0; i < modeButtons.length; i++) {
        let x = padding + (i % 2) * (buttonWidth + padding);
        let y = currentY + floor(i / 2) * (buttonHeight + spacing);
        modeButtons[i].size(buttonWidth, buttonHeight);
        modeButtons[i].position(x, y);
    }

    // Reposicionar GIFs
    currentY += 2 * (buttonHeight + spacing) + spacing;
    for (let i = 0; i < gifButtons.length; i++) {
        let x = padding + (i % 2) * (buttonWidth + padding);
        let y = currentY + floor(i / 2) * (buttonHeight + spacing);
        gifButtons[i].size(buttonWidth, buttonHeight);
        gifButtons[i].position(x, y);
    }
    
    resizeCanvas(windowWidth, windowHeight);
}
```










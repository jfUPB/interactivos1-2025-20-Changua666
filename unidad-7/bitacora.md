
# Evidencias de la unidad 7

## Actividad 1 
### ¿Qué URL de Dev Tunnels obtuviste? ¿Por qué crees que necesitamos usar esta URL en lugar de http://localhost:3000 o la IP local de tu computador para que el celular se conecte?

Obtuve esta url [https://7dzjhqs1-3000.use2.devtunnels.ms/](https://7dzjhqs1-3000.use2.devtunnels.ms/) y creo que se necesita usar esta para crear ese tunel con los servidores de microsoft para poder conectar el celular sin necesidad de estar en la misma red.

### Describe brevemente qué hace npm install y npm start.
npm install instala las dependencias que necesita el repo para poder funcionar y npm start inicia el proyecto.

### ¿Qué mensajes observaste en la terminal del servidor al conectar el cliente de escritorio y el cliente móvil? ¿Eran diferentes los mensajes o identificadores?

Al conectar el cliente del escritorio y el cliente movil me aparecio esto 

<img width="573" height="187" alt="image" src="https://github.com/user-attachments/assets/2b470e79-9ea1-4f49-b609-1e341859f357" />

Yo veo que los dos mensajes son iguales 

### Describe el comportamiento observado: ¿Funcionó la interacción? ¿Hubo algún retraso (latencia)?
En el computador aparece esto 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/162a6b93-bd68-4b2a-b6ee-710602e1c01d" />
En el celular aparece esto 
<img width="739" height="1600" alt="image" src="https://github.com/user-attachments/assets/62e977a0-10f7-4721-a4a3-354ad0965224" />

y al tocar el touch de mi celular se mueve la bola que aparece en la pantalla del computador, yo no observo un delay visible pero me imagino que tiene un poco de delay. 

## Actividad 2 
### Explica con tus propias palabras: ¿Por qué es necesario Dev Tunnels en este escenario y cómo funciona conceptualmente?
Dev Tunnels es necesario porque el servidor corre en localhost, que solo el computador puede ver.
Con Dev Tunnels, se crea una URL pública que actúa como un puente seguro entre Internet y el servidor local.
Así, el celular (u otros dispositivos) pueden conectarse al servidor aunque no estén en la misma red.

### Describe la función de touchMoved() y por qué se usa la variable threshold en el cliente móvil.
La funcion touchMoved() se ejecuta cada vez que un usuario mueve el dedo sobre la pantalla tactil de su dispositivo.
Esta funcion sirve para detectar y enviar las coordenadas del movimiento (posicion eje X e Y).

La variable threshold se usa para enviar datos por cada movimiento. 
Solo cuando el dedo se mueve una distancia mayor del threshold se envia la info, evitando sobrecargar la conexion con tantos mensajes. 

### Compara brevemente Dev Tunnels con simplemente usar la IP local. ¿Cuáles son las ventajas y desventajas de cada uno?

**Usar la ip local** 
- Ventajas: Es rapido y no depende del internet
- Desventajas: Solo funciona si los dos dipositivos estan en la misma red y no hay bloqueos con firewall

**usar Dev Tunnels**
- Ventajas: Permite conectarse desde cualquier lugar (sin importar la red) con una URL publica y segura
- Desventajas: Requiere estar conectado a internet y puede llegar a ser mas lento que la ip local

## Actividad 3 
**¿Cuál es la función principal de express.static(‘public’) en este servidor? ¿Cómo se compara con el uso de app.get(‘/ruta’, …) del servidor de la Unidad 6?**
La funcion express.static(‘public’) permite que el servidor comparta de manera automatica todos los archivos que se encuentran dentro de la carpeta public, como paginas en HTML o scripts. Gracias a esto, no se ve la necesidad de crear rutas manuales por cada archivo. En cambio, cuando se usa app.get(‘/ruta’, …), el server define de forma manual que debe responder al acceder a una ruta especifica. 

**Flujo Detallado**
1) Que evento lo envia desde el celular ?
En el cliente movil (p5.js) el evento que detecta el movimiento en la pantalla tactil suele ser touchmoved() (o touchStarted() ). Dentro de esa funcion se crea un payload con las coordenadas y se envia al server con socket.emit(...)

2) ¿Qué evento lo recibe el servidor?
En el servidor Node.js + Socket.IO se recibe con socket.on('message', ...) (el mismo nombre de evento que usó el móvil).

3) ¿Qué hace el servidor con él?
El servidor hace lo tipico, valida la informacion, añade metadatos, guarda un log, lo reenvia.

4) ¿Qué evento lo envía el servidor al escritorio?
El servidor emite otro evento hacia los clientes. El escritorio (stage) tiene un listener socket.on('message', ...) que recibe ese paquete y actualiza en tiempo real lo que pasa en pantalla

5) Por qué se usa socket.broadcast.emit en lugar de io.emit o socket.emit?
- socket.emit(...) envía solo al emisor (la misma conexión).
No sirve aquí porque el objetivo es notificar a otros clientes, no de nuevo al que ya envió el toque.

- io.emit(...) envía a todos los sockets conectados, incluyendo al que generó el evento.
Si se usa io.emit, el cliente que tocó recibirá su propio mensaje de vuelta. Eso puede provocar eco visual/sonoro o duplicación de la acción (el toque ya fue procesado localmente en el móvil, luego vuelve con latencia y se procesa otra vez).

- socket.broadcast.emit(...) envía a todos los demás excepto al emisor.
Es la opción indicada cuando el emisor ya aplicó el efecto localmente y solo quieres que los demás vean esa acción en tiempo real.
Reduce tráfico y evita efectos de duplicación/eco.



### Si conectaras dos computadores de escritorio y un móvil a este servidor, y movieras el dedo en el móvil, ¿Quién recibiría el mensaje retransmitido por el servidor? ¿Por qué?

Si se conectaran dos computadores de escritorio y un celular al server, y se moviera el dedo en el celular, solo los dos computadores de escritorio recibirian el mensaje retransmitido.

Esto sucede porque el server usa socket.broadcast.emit(), que envia el mensaje a todos los clientes que esten conectados excepto al que lo envio. En este caso, el celular fue el emisor (ya quqe detecto el toque y envio el mensaje), por lo tanto, no recibe su propio mensaje de vuelta. Los otros dispositivos si lo reciben, porque estan conectados al mismo server y no fueron quienes originaron ese evento.

### ¿Qué información útil te proporcionan los mensajes console.log en el servidor durante la ejecución?


## Actividad 5 
Mi idea es hacer un visual basado en esta portada de este album 
<img width="300" height="300" alt="Charli_XCX_-_Brat_album_cover" src="https://github.com/user-attachments/assets/394ee9df-e648-4ed0-add8-b9a223961f98" />
Entonces se me ocurrio representarlo con 2 cosas: 
- 4 tipos de ecualizadores a los que se les puede ajustar la intesidad:
  - Unas barras verticales sencillas, cada color representa un tono: Los bajos son blancos, los medios negros y los agudos son verdes
  - Un circulo que tambien toma los distintos tonos: Los bajos son los puntos cerca del espectro, los medios y agudos estan mas adelante
  - Una onda lateral tipo montaña que tambien toma los distintos tonos
  - Unos visualizadores con forma cuadrada que toman las distintas frecuencias
- 4 gifs que representan esa energia caotica y divertida del album que tambien se pueden cambiar.

### Codigo Server.js
```

const express = require('express');
const http = require('http');
const socketio = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = socketio(server);
const port = 3000;


app.use(express.static('public')); 

io.on('connection', (socket) => {
    console.log('Un cliente se ha conectado:', socket.id);

    
    socket.on('touchData', (data) => {
       
        socket.broadcast.emit('visualControl', data); 
    });

    socket.on('disconnect', () => {
        console.log('Cliente desconectado:', socket.id);
    });
});


server.listen(port, () => {
    console.log(`Server is listening on http://localhost:${port}/`);
});
```

### Cliente desktop  
```
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Visuales Desktop</title>

 
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.9.0/p5.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.9.0/addons/p5.sound.min.js"></script>

  
    <script src="http://localhost:3000/socket.io/socket.io.js"></script>

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
    <script src="sketch.js"></script>
  </body>
</html>

```

### Cliente Mobile 

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
        body { margin: 0; overflow: hidden; background-color: #8ace00; }
        canvas { display: block; }
    </style>
</head>
<body>
    <script src="sketch.js"></script> 
</body>
</html>
```

### Sketch Desktop
```
let socket;
let mobileIntensity = 1; 
let mobileMode = 1;      
let mobileGif = 0;       

let song, fft;
let gifImgs = [];
let audioStarted = false; 


function preload() {
    song = loadSound('../assets/club-classics.mp3');
    gifImgs[0] = loadImage('../assets/pickle-rick-dance.gif');
    gifImgs[1] = loadImage('../assets/choerry-dance.gif');
    gifImgs[2] = loadImage('../assets/yoonchae-dance.gif');
    gifImgs[3] = loadImage('../assets/shaq.gif');
}


function setup() {
    createCanvas(windowWidth, windowHeight);
    noStroke();
    fft = new p5.FFT();
    textAlign(CENTER, CENTER);
    textSize(18);
    
    
    socket = io.connect(window.location.origin);
    
   
    socket.on('visualControl', (data) => {
        mobileMode = data.newMode;
        mobileGif = data.newGif;
        mobileIntensity = data.newIntensity;
    });
}


function mousePressed() {
    // 1. Intentar iniciar/reanudar el AudioContext del navegador
    userStartAudio(); 
    
   
    if (getAudioContext().state !== 'suspended' && !audioStarted && song.isLoaded()) {
        song.loop();
        audioStarted = true; // Marcar como iniciado
        console.log("AudioContext reanudado y música iniciada por clic.");
    }
}


function draw() {
    background(138, 206, 0); // Fondo BRAT Verde
    let spectrum = fft.analyze();
    let intensity = mobileIntensity; 

    
    let low = spectrum.slice(0, spectrum.length / 3);
    let mid = spectrum.slice(spectrum.length / 3, (2 * spectrum.length) / 3);
    let high = spectrum.slice((2 * spectrum.length) / 3);

   
    if (mobileMode === 1) { 
        drawSpectrumBars(low, mid, high, intensity);
    } else if (mobileMode === 2) {
        drawCircleSpectrum(spectrum, intensity);
    } else if (mobileMode === 3) {
        drawTriangleSpectrum(spectrum, intensity);
    } else if (mobileMode === 4) {
        drawSquareSpectrum(spectrum, intensity);
    }

  
    if (gifImgs[mobileGif]) { 
        imageMode(CENTER);
        image(gifImgs[mobileGif], width / 2, height / 2, 300, 300);
    }

    fill(0);
    text('brat', width / 2, 40);
    text('Intensidad de las visuales: ' + intensity.toFixed(1), width / 2, 70);
    text('Modo: ' + mobileMode + ' | GIF: ' + mobileGif, width / 2, 100); 
    
    
    if (!audioStarted) {
        text('CLIC EN LA PANTALLA PARA INICIAR LA MÚSICA (me toco hacer eso porque no vi mas solucion porfa profe no me raje)', width / 2, height - 50);
    }
}


function drawSpectrumBars(low, mid, high, intensity) {
    drawSpectrum(low, color(255, 255, 255), intensity);
    drawSpectrum(mid, color(0), intensity);
    drawSpectrum(high, color(138, 206, 0), intensity);
}

function drawSpectrum(spectrum, c, intensity) {
    fill(c);
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
    fill(0);
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
    fill(0);
    let step = 25;
    for (let i = 0; i < spectrum.length; i += step) {
        let x = map(i, 0, spectrum.length, 0, width);
        let size = map(spectrum[i] * intensity, 0, 255, 10, 120);
        rect(x, height / 2 - size / 2, size, size);
    }
}

function windowResized() {
    resizeCanvas(windowWidth, windowHeight);
}
```

### Sketch Mobile 
```
let socket;


let currentMode = 1; 
let currentGif = 0;
let intensitySlider; 


let modeButtons = [];
let gifButtons = [];

function setup() {
    createCanvas(windowWidth, windowHeight);
    background(138, 206, 0); 
    
    socket = io.connect(window.location.origin);

    const padding = 20; 
    const spacing = 10; 
    const buttonHeight = 50;
    const buttonWidth = (width - 3 * padding) / 2;
    
   
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
    
   
   
    currentY += 2 * (buttonHeight + spacing) + spacing; 
    let gifLabels = ['PICKLE RICK', 'CHOERRY', 'YOONCHAE', 'SHAQ'];
    
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
    
   
    currentY += 2 * (buttonHeight + spacing) + padding; 

    intensitySlider = createSlider(0.5, 5.0, 1.0, 0.1);
    intensitySlider.style('width', (width - 2 * padding) + 'px');
    intensitySlider.position(padding, currentY);
    intensitySlider.input(sendControlData);
    
   
    updateButtonStyles(modeButtons, currentMode);
    updateButtonStyles(gifButtons, currentGif);
    
    sendControlData();
}

function draw() {
    background(138, 206, 0);
    fill(0);
    textAlign(LEFT, TOP);
    textSize(18);
    // Mostrar información de intensidad junto al slider
    text('INTENSIDAD: ' + intensitySlider.value().toFixed(1), intensitySlider.x, intensitySlider.y + intensitySlider.height + 5);
}



function updateButtonStyles(buttonsArray, activeValue) {
    buttonsArray.forEach((btn, index) => {
        let value = (buttonsArray === modeButtons) ? (index + 1) : index;
        if (value === activeValue) {
            btn.style('background-color', '#000');
            btn.style('color', '#8ace00');
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
        newIntensity: intensitySlider.value()
    };
    socket.emit('touchData', data);
}

function windowResized() {
    resizeCanvas(windowWidth, windowHeight);
}
```







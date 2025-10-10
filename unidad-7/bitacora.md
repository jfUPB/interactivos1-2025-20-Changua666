
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




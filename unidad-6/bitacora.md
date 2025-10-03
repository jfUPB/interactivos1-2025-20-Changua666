
# Evidencias de la unidad 6

## Actividad 1
**¿Qué ocurrió en la terminal cuando ejecutaste npm install? ¿Cuál crees que es su propósito?**
Se clono y se instalo el repo donde esta la aplicacion interactiva que estamos analizando 

**¿Qué mensaje específico apareció en la terminal después de ejecutar npm start? ¿Qué indica este mensaje?**
Aparecio esto 
```
> nodejs-test-1@1.0.0 start
> node server.js
```
supongo que indica que se ejecuto alguna accion de el repositorio 

**Describe lo que ves inicialmente en page1 y page2 en tu navegador.**
Aparece un mensaje de que se estan conectando 

**¿Qué mensajes aparecieron en la terminal del servidor cuando abriste page1 y page2?**
Por lo que interpreto aparecen mensajes de el estado de las paginas (si estan sincronizadas, el tamaño de estas, etc) 

**Describe qué sucede en ambas páginas del navegador cuando mueves una de las ventanas.**
Las ventanas estan como conectadas


## Actividad 2 

**Piensa en cómo te conectas a Internet en casa o en la Universidad. ¿Usas Wi-Fi? ¿Un cable de red? Eso es simplemente tu “rampa de acceso” a la gran red de carreteras. ¿Qué pasaría si esa rampa se corta? Anota tus ideas.**

Si la conexión a Internet en casa o en la universidad se corta, sería como perder la rampa de acceso a la gran red de información. Esto significaría no poder ingresar a páginas web ni plataformas educativas, lo que afectaría directamente el estudio y la comunicación con profesores y compañeros. También interrumpiría el acceso a servicios de mensajería, videollamadas y redes sociales, limitando tanto el trabajo como el entretenimiento. En ese momento sería necesario buscar alternativas, como usar datos móviles o trabajar sin conexión, lo cual evidencia lo mucho que dependemos del Internet para nuestras actividades cotidianas.

**¿Puedes identificar otros ejemplos de relaciones Cliente-Servidor en tu vida diaria (no necesariamente digitales)? Por ejemplo, al pedir comida en un restaurante. ¿Quién es el cliente y quién el servidor? ¿Qué se pide y qué se entrega?**

Por ejemplo, cuando vamos a un restaurante, el cliente es la persona que pide un plato y el servidor es el mesero o la cocina que entrega la comida. En una biblioteca, el cliente es el estudiante que solicita un libro y el servidor es el bibliotecario que lo entrega. Lo mismo sucede en el transporte público, donde el pasajero pide un viaje y el conductor lo lleva a su destino. En las clases, el estudiante es el cliente que pide explicaciones y el profesor actúa como servidor al brindar el conocimiento. Incluso en un supermercado, el comprador solicita pagar sus productos y el cajero responde con la factura y el permiso de salida. En todos estos casos se repite la misma lógica: alguien pide un recurso o servicio y otro lo entrega.

Cuando observamos una dirección web como https://www.youtube.com/watch?v=LJu6gBEHIvs&list=RDLJu6gBEHIvs&start_radio=1&t=119s
, podemos identificar que el protocolo es https, el nombre de dominio es www.youtube.com
 y la ruta es /watch?v=LJu6gBEHIvs&list=RDLJu6gBEHIvs&start_radio=1&t=119s, que indica el recurso específico al que queremos acceder. Si solo escribimos el nombre de dominio, como www.youtube.com, el navegador envía la petición al servidor sin ruta y este responde con una página por defecto, generalmente la página principal o de inicio. Esto sucede porque los servidores suelen estar configurados para mostrar automáticamente un archivo base, como index.html, cuando no se indica una ruta específica.

 **Compara HTTP con los protocolos seriales que usaste.**
Tanto HTTP como los protocolos seriales sirven para comunicar dispositivos y ambos transmiten información en forma de bytes, pero la diferencia está en el nivel de complejidad. Mientras que un protocolo serial como el del micro:bit envía datos de manera directa y simple, byte a byte, HTTP organiza la comunicación con reglas claras que incluyen métodos, encabezados y códigos de estado. Esto es necesario porque HTTP funciona en una red global donde millones de clientes y servidores distintos deben entenderse sin importar la plataforma o el sistema operativo. Por eso, HTTP no solo envía datos, sino que también especifica qué recurso se pide, cómo debe responder el servidor y en qué formato, lo que lo hace mucho más complejo que un simple envío de bytes.

**Piensa en una página web simple, como un formulario de login.
¿Qué parte crees que es HTML (ej. los campos de texto, el botón)?
¿Qué parte es CSS (ej. el color del botón, el tipo de letra)?
¿Qué parte es JavaScript (ej. la comprobación de si escribiste algo antes de enviar, el mensaje de “contraseña incorrecta” que aparece sin recargar la página)?**

En un formulario de login, el HTML define la estructura con los campos de texto y el botón, el CSS da el estilo como colores, letras y tamaños, y el JavaScript añade interactividad, por ejemplo validando que no falten datos o mostrando mensajes de error sin recargar la página.

**Compara el bucle draw() de p5.js con este modelo de “esperar a que algo pase y reaccionar”.
¿Qué ventajas crees que tiene el modelo basado en eventos para una interfaz de usuario web?**

El bucle draw() de p5.js redibuja la pantalla constantemente, lo cual es útil en animaciones, pero en una interfaz web sería ineficiente, ya que estaría actualizando la página 60 veces por segundo aunque nada cambiara. En cambio, el modelo basado en eventos solo reacciona cuando ocurre una acción, como un clic o una tecla presionada, lo que ahorra recursos, mejora el rendimiento y hace más práctica la interacción con el usuario.

**¿Por qué crees que podría ser útil usar JavaScript tanto en el cliente (navegador) como en el servidor? ¿Se te ocurre alguna ventaja para los desarrolladores?**

Usar JavaScript en el cliente y en el servidor tiene una gran ventaja: permite que todo el proyecto esté en el mismo lenguaje. Eso significa que los desarrolladores no necesitan aprender tecnologías distintas para cada parte, pueden reutilizar código (por ejemplo, validaciones de formularios) tanto en el navegador como en el servidor, y el trabajo en equipo se vuelve más sencillo porque todos hablan el mismo “idioma”. Además, usar un solo lenguaje facilita el mantenimiento y reduce errores, ya que no hay que estar “traduciendo” lógica de un lenguaje a otro.

## Actividad 5 
Mi idea es un semaforo que en una pestaña aparezca y en la otra se controle, la hice porque me parecio sencillo y ya 

Codigo server.js
 ```
// server.js
const express = require("express");
const app = express();
const http = require("http").createServer(app);
const { Server } = require("socket.io");
const io = new Server(http);

app.use(express.static("public"));

io.on("connection", (socket) => {
  console.log("Un usuario conectado");

  socket.on("cambiarColor", (color) => {
    // retransmitir el nuevo color a TODOS los clientes
    io.emit("actualizarColor", color);
  });

  socket.on("disconnect", () => {
    console.log("Usuario desconectado");
  });
});

http.listen(3000, () => {
  console.log("Servidor corriendo en http://localhost:3000");
});
```

Codigo control html

```
// server.js
const express = require("express");
const app = express();
const http = require("http").createServer(app);
const { Server } = require("socket.io");
const io = new Server(http);

app.use(express.static("public"));

io.on("connection", (socket) => {
  console.log("Un usuario conectado");

  socket.on("cambiarColor", (color) => {
    // retransmitir el nuevo color a TODOS los clientes
    io.emit("actualizarColor", color);
  });

  socket.on("disconnect", () => {
    console.log("Usuario desconectado");
  });
});

http.listen(3000, () => {
  console.log("Servidor corriendo en http://localhost:3000");
});
```

Codigo visor html 
```
<!DOCTYPE html>
<html>
<head>
  <title>Visor de Semáforo</title>
  <script src="/socket.io/socket.io.js"></script>
  <style>
    #semaforo {
      width: 100px;
      height: 100px;
      border-radius: 50%;
      background: gray;
      margin: 50px auto;
    }
  </style>
</head>
<body>
  <h1>Semáforo</h1>
  <div id="semaforo"></div>

  <script>
    const socket = io();
    const semaforo = document.getElementById("semaforo");

    socket.on("actualizarColor", (color) => {
      semaforo.style.background = color;
    });
  </script>
</body>
</html>
```

Los hosts son 
```
http://localhost:3000/control.html
```
y 
```
http://localhost:3000/visor.html
```





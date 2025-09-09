# Unidad 2

## 🔎 Fase: Set + Seek  

### Actividad 01
***¿Como funciona este ejemplo?***
Este programa en MicroPython para micro:bit implementa el parpadeo independiente de dos LEDs en la matriz, utilizando una máquina de estados para cada píxel.

Se define una clase Pixel, encargada de gestionar el comportamiento de un LED en una posición específica (pixelX, pixelY). Cada instancia alterna entre encendido (valor 9) y apagado (valor 0) según un intervalo de tiempo configurado por el usuario.

La lógica de parpadeo se ejecuta continuamente mediante el método update(), que se llama dentro de un bucle infinito, permitiendo que cada LED parpadee de manera independiente.

***¿Cuales son los estados de este programa?***
Init: Inicializa el tiempo (startTime) y muestra el estado inicial del pixel.
WaitTimeout: Espera a que pase el tiempo para alternar el estado del píxel (encendido o apagado) y actualizar el tiempo de referencia.
De esta manera cada pixel es independiente y parpadea en su propio intervalo de tiempo sin necesidad de de interrupciones ni eventos externos. 

***¿Cuales son los eventos/inputs del sistema?***
El script no depende de un input ya que sus eventos internos son el paso del tiempo medido con  utime.ticks_ms() y utime.ticks_diff().

***¿Cuales son las acciones que realiza el programa?***

Inicializar el tiempo de referencia:
Se guarda el tiempo actual utilizando utime.ticks_ms() cuando el estado es "Init".

Encender o apagar el píxel:
Se utiliza display.set_pixel(x, y, valor) para mostrar el estado del pixel en la matriz LED. El valor cambia entre 0 (apagado) y 9 (encendido).

Alternar el estado del brillo:
Cuando ha pasado el intervalo de tiempo definido, el valor del pixel se alterna de 0 a 9.

Actualizar el tiempo de referencia nuevamente:
Cada vez que se cambia el estado del pixel, se actualiza startTime para medir el siguiente intervalo correctamente.

Mantenerse en el estado "WaitTimeout":
Una vez inicializado, el objeto permanece en este estado ejecutando las acciones anteriores en cada ciclo de actualizacion.

# Actividad 2

  ### *1. Codigo del semaforo*  
  
``` python
from microbit import *
import utime

class Pixel:
    def __init__(self, pixelX, pixelY):
        self.pixelX = pixelX
        self.pixelY = pixelY

    def on(self):
        display.set_pixel(self.pixelX, self.pixelY, 9)

    def off(self):
        display.set_pixel(self.pixelX, self.pixelY, 0)

rojo = Pixel(2, 3)
amarillo = Pixel(2, 2)
verde = Pixel(2, 1)
#yo sigma
while True:
    rojo.on()
    amarillo.off()
    verde.off()
    sleep(3000)

    rojo.off()
    amarillo.on()
    verde.off()
    sleep(1000)

    rojo.off()
    amarillo.off()
    verde.on()
    sleep(3000)
```

### **2. los estados, eventos y acciones del codigo

### Estados
El sistema tiene tres estados principales, uno por cada luz del semáforo:

- *Rojo*: LED rojo encendido
- *Amarillo*: LED amarillo encendido
- *Verde*: LED verde encendido

### Eventos
Los cambios de estado se dan por eventos de tiempo:

- Después de *3 segundos* → pasa de *Rojo* a *Amarillo*
- Después de *1 segundo* → pasa de *Amarillo* a *Verde*
- Después de *3 segundos* → pasa de *Verde* a *Rojo*

### Acciones
Cada estado enciende un LED y apaga los otros:

- *Rojo*:  
  - rojo.on()
  - amarillo.off()
  - verde.off()

- *Amarillo*:  
  - amarillo.on()  
  - rojo.off()  
  - verde.off()

- *Verde*:  
  - verde.on()  
  - rojo.off()  
  - amarillo.off()


# Actividad 3
## 1. ¿Por qué decimos que este programa permite realizar de manera concurrente varias tareas?

Aunque MicroPython en Micro:bit no ejecuta tareas en paralelo real, este programa simula concurrencia porque:

Usa una máquina de estados para cambiar entre estados según eventos (botón A) y tiempo transcurrido.

Mientras espera el tiempo definido por interval, no bloquea el ciclo principal, por lo que puede:

Revisar continuamente si el botón A fue presionado.

Revisar si ya pasó el tiempo para cambiar de estado.

Es decir, parece que monitorea el tiempo y la entrada del botón al mismo tiempo, aunque en realidad se hace de forma secuencial en un while True rápido.

## 2. Estados, eventos y acciones del programa

Estados definidos:

STATE_INIT → estado inicial, muestra cara feliz por primera vez.

STATE_HAPPY → muestra Image.HAPPY.

STATE_SMILE → muestra Image.SMILE.

STATE_SAD → muestra Image.SAD.

Eventos que generan cambios de estado:

Presionar botón A (button_a.was_pressed()).

Cumplir el tiempo del intervalo (utime.ticks_diff(...) > interval).

Acciones que realiza el sistema:

Mostrar una imagen en la matriz LED (display.show(Image.XXX)).

Reiniciar el temporizador (start_time = utime.ticks_ms()).

Ajustar el nuevo intervalo (interval = XXX_INTERVAL).

## 3. Vectores de prueba del programa

### Vector de prueba 1: Cambio automático HAPPY → SMILE
Condiciones iniciales:

Micro:bit recién encendido.

Estado actual: STATE_HAPPY (después de STATE_INIT).

Evento:

Dejar pasar 1.5 segundos sin presionar ningún botón.

Resultado esperado:

Pasa a STATE_SMILE.

Acciones: mostrar Image.SMILE y reiniciar temporizador a 1 s.

Resultado real:

 Image.SMILE se muestra en pantalla.

 Intervalo actualizado a SMILE_INTERVAL.

### Vector de prueba 2: Presionar botón A en estado SMILE
Condiciones iniciales:

Estado actual: STATE_SMILE.

Evento:

Presionar botón A.

Resultado esperado:

Pasa a STATE_HAPPY.

Acciones: muestra Image.HAPPY y reinicia temporizador a 1.5 s.

Resultado real:

 La cara feliz aparece.

 Se reinicia el conteo de tiempo.

### Vector de prueba 3: Cambio automático SMILE → SAD
Condiciones iniciales:

Estado actual: STATE_SMILE.

Evento:

Dejar pasar 1 segundo sin presionar botón.

Resultado esperado:

Pasa a STATE_SAD.

Acciones: mostrar Image.SAD y reiniciar temporizador a 2 s.

Resultado real:

 La cara triste aparece.

 Se reinicia el temporizador correctamente.














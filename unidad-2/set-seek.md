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

## CODIGO DE CLASE 

´´´
from microbit import *
import utime

STATE_INIT = 0
STATE_HAPPY = 1 
STATE_SMILE = 2
STATE_SAD = 3 
TIME_IN_HAPPY = 1500


currentState = STATE_INIT


def tarea1():
    global currentState
    global startTime
    global interval
    
    if currentState == STATE_INIT: 
        display.show(Image.HAPPY) 
        startTime = utime.ticks_ms()
        interval = TIME_IN_HAPPY 
        currentState = STATE_HAPPY
        
    elif currentState == STATE_HAPPY: 
        if utime.ticks_diff(utime.ticks_ms(),startTime) > interval:
            pass
    elif currentState == STATE_SMILE: 
        pass
    elif currentState == STATE_SAD: 
        pass 
    else: 
        pass

while True: 
    tarea1()
´´´














# Unidad 2


## 🛠 Fase: Apply
### Actividad 5 
Codigo Microbit 
```
from microbit import *

uart.init(baudrate=115200)

while True:
    if button_a.is_pressed():        # UP
        uart.write('U')
    elif button_b.is_pressed():      # DOWN
        uart.write('D')
    elif accelerometer.was_gesture("shake"):   # ARMED
        uart.write('A')
    elif pin_logo.is_touched():      # RESET (touch)
        uart.write('R')
    else:
        uart.write('N')
    sleep(100)
```
Codigo p5.js 
[Codigo](https://editor.p5js.org/Changua666/sketches/6IiSj3WPo)


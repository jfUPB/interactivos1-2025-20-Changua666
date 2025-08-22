# Unidad 3


## 🤔 Fase: Reflect
## Actividad 8 

# Parte 1 
1. ¿ Que es una maquina de estados ?

Es un modelo Computacional que se usa para demostrar el comportamiento de una maquina/sistema que puede estar en mas de un estado definido y que cambia de estados en respuesta a algun evento o entrada.

2. ¿ por qué la técnica de máquina de estados es tan útil para gestionar la “concurrencia” ?

Los microcontroladores (como el microbit) no puede hacer multiples tareas al mismo tiempo debido a que solo tiene un hilo de ejecucion. La maquina de estados nos ayuda a organizar y distribuir las tareas en pequeños pasos. ejecutados uno por uno en cada ciclo (como draw() en p5.js) sin parar la ejecucion completa.

Al usar funciones como sleep() o delay() el programa se detiene completamente por un tiempo y durante este tiempo no puede responder a ningun evento. Esto hace que el dispositivo pierda eficiencia e interactividad. 

3. Añadiria una trancision dentro del estado del conteo regresivo para detectar este nuevo evento
4. Explica qué es un “vector de prueba” y por qué es una herramienta crucial para verificar que una máquina de estados funciona como se espera.

Un vector de prueba es como una secuencia de eventos y entradas, junto con lo que se espera que ocurra en cada paso. Este se usa para verificar el comportamiento de una maquina de estados. 
Este contiene: 
- El estado incial
- Las entradas
- Estado esperado
- Salida esperada

Esta es una herramienta crucial para para verificar que todo este bien ya que verifica que las trancisiones funcionen correctamente, permite generar cambios de manera mas sencilla, ayuda a detectar errores y facilita el uso de pruebas automatizadas. 

# Parte 2
1. Me resulto mas desafiante traducir el codigo ya que siempre se me ha complicado un poco la parte de programar, mas con un lenguaje nuevo.
2. 



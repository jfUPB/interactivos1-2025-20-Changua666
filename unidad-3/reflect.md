# Unidad 3


## 🤔 Fase: Reflect
## Actividad 8 

# Parte 1 
1. ¿ Que es una maquina de estados ?
Es un modelo Computacional que se usa para demostrar el comportamiento de una maquina/sistema que puede estar en mas de un estado definido y que cambia de estados en respuesta a algun evento o entrada.

2. ¿ por qué la técnica de máquina de estados es tan útil para gestionar la “concurrencia” ?
Los microcontroladores (como el microbit) no puede hacer multiples tareas al mismo tiempo debido a que solo tiene un hilo de ejecucion. La maquina de estados nos ayuda a organizar y distribuir las tareas en pequeños pasos. ejecutados uno por uno en cada ciclo (como draw() en p5.js) sin parar la ejecucion completa.

Al usar funciones como sleep() o delay() el programa se detiene completamente por un tiempo y durante este tiempo no puede responder a ningun evento. Esto hace que el dispositivo pierda eficiencia e interactividad. 

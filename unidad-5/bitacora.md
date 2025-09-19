
# Evidencias de la unidad 5
# Actividad 3 
### Explica por qué en la unidad anterior teníamos que enviar la información delimitada y además marcada con un salto de línea y ahora no es necesario.
En la unidad anterior era necesario usar delimitadores y un salto de línea porque los números se enviaban en formato ASCII, lo que generaba cadenas de longitud variable que el receptor no podía separar por sí solo. Los delimitadores indicaban dónde acababa un valor y el salto de línea marcaba el fin del paquete. En cambio, al usar un formato binario con struct.pack, cada dato tiene un tamaño fijo y el paquete siempre mide 6 bytes, por lo que el receptor sabe exactamente cuántos bytes leer sin necesidad de separadores adicionales.


### Compara el código de la unidad anterior relacionado con la recepción de los datos seriales que ves ahora. ¿Qué cambios observas?

En la unidad anterior los datos llegaban en formato ASCII, lo que obligaba a usar un salto de línea para detectar el fin del paquete y split para separar los valores, además de convertir cadenas de texto a números o booleanos. En cambio, en el código actual los datos llegan en binario con un tamaño fijo de 6 bytes, por lo que basta con leer esa cantidad exacta, acceder a cada campo por su posición en el buffer y convertirlos directamente a enteros o booleanos. Esto elimina la necesidad de delimitadores y conversiones, haciendo la transmisión más eficiente pero menos legible.


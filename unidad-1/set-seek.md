# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 4 
[Link Del Programa](https://editor.p5js.org/Changua666/sketches/Xkow2L6QY)

**Codigo**
```
function setup() {
  createCanvas(windowWidth, windowHeight);
  noStroke();
}

function draw() {
  background(20, 30, 40, 50); // Fondo con transparencia para dejar trazas

  // Dibujar muchos círculos en posiciones aleatorias con sin/cos
  for (let i = 0; i < 200; i++) {
    let angle = random(TWO_PI);      // Ángulo aleatorio
    let r = random(width / 3);       // Radio aleatorio
    let x = width / 2 + cos(angle) * r;
    let y = height / 2 + sin(angle) * r;

    let sz = random(5, 20);          // Tamaño aleatorio
    let c = color(
      150 + 105 * sin(frameCount * 0.01 + i), // rojo varía con sin
      150 + 105 * cos(frameCount * 0.01 + i), // verde varía con cos
      random(200, 255),                       // azul aleatorio
      180
    );
    fill(c);
    ellipse(x, y, sz, sz);
  }
}
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4b4ef09c-6c4f-47e6-9f3f-a0ea9f1ed583" />







```js
function setup() {
  createCanvas(800, 800);
  noLoop(); 
  background(30); 
  noFill(); 
  stroke(255, 100); 
  strokeWeight(1); 
}

function draw() {
  // Parámetros
  const numLines = 100;
  const maxRadius = 400;
  const angleOffset = 0.1;

  translate(width / 2, height / 2); // Mueve el origen al centro del lienzo

  for (let i = 0; i < numLines; i++) {
    // Usa funciones trigonométricas para generar coordenadas
    let angle = map(i, 0, numLines, 0, TWO_PI);
    let radius = random(maxRadius * 0.2, maxRadius);

    // Coordenadas iniciales y finales de la línea
    let x1 = radius * cos(angle);
    let y1 = radius * sin(angle);
    let x2 = radius * cos(angle + angleOffset);
    let y2 = radius * sin(angle + angleOffset);

    // Dibuja una línea con coordenadas aleatorias
    line(x1, y1, x2, y2);

    // Dibuja un círculo en cada punto
    ellipse(x1, y1, 5, 5);
  }
}

// Presiona la tecla 'R' para regenerar el patrón
function keyPressed() {
  if (key === 'r' || key === 'R') {
    background(30);
    redraw();
  }
}
```

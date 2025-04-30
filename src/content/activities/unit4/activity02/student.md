# Solución

Este sketch genera un conjunto de líneas onduladas y paralelas que fluyen de manera suave y orgánica sobre el lienzo. Las líneas son curvas que se crean punto a punto con algo de aleatoriedad, pero dentro de un control que las mantiene suaves. Este tipo de técnica es común en el arte generativo para crear patrones naturales o que recuerdan estructuras orgánicas.

---

## Explicación del código por partes

```js
let lines = [];
let noiseScale = 0.005;
let stepSize = 2;
```

**lines:** un arreglo para almacenar las líneas.

**noiseScale:** escala para el ruido Perlin (valores bajos = más suave).

**stepSize:** espacio entre puntos en la dirección horizontal (X).

### Setup()

```js
function setup() {
  createCanvas(800, 800);
  noLoop();

  for (let y = 0; y < height; y += 10) {
    let line = [];
    for (let x = 0; x < width; x += stepSize) {
      let nx = x * noiseScale;
      let ny = y * noiseScale;
      let angle = noise(nx, ny) * TWO_PI;
      let dy = sin(angle) * 10;

      line.push(createVector(x, y + dy));
    }
    lines.push(line);
  }
}
```

- Se crea el lienzo 800x800.

- Se generan múltiples líneas horizontales (espaciadas cada 10 píxeles en y).

Para cada línea, se crean puntos que:

- Están espaciados horizontalmente (x += stepSize).

- Tienen una ligera variación en y basada en ruido Perlin (ruido suave).

- Se usa sin(angle) para que la distorsión se vea más como una ondulación.

- noise() genera valores entre 0 y 1 que se convierten en un ángulo entre 0 y 2π.

### Draw ()

```js
function draw() {
  background(255);
  stroke(0);
  noFill();

  for (let line of lines) {
    beginShape();
    for (let v of line) {
      vertex(v.x, v.y);
    }
    endShape();
  }
}
```
- Pone fondo blanco.

- Dibuja cada línea con beginShape() → vertex() → endShape().

- El resultado son muchas líneas suavemente onduladas.

## Experimentación

### Mayor ruido (más desorden)

```javascript
let noiseScale = 0.02; // antes 0.005
```

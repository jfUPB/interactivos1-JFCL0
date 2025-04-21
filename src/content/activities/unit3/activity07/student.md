```js
let tiempo = 20;
let estado = 'configuracion'; // Estados: configuracion, armado, explosion

function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(0);
  fill(255);
  textSize(32);
  textAlign(CENTER, CENTER);
  
  if (estado === 'configuracion') {
    text('Configurar: ' + tiempo + 's', width / 2, height / 2);
  } else if (estado === 'armado') {
    text('Tiempo restante: ' + tiempo + 's', width / 2, height / 2);
  } else if (estado === 'explosion') {
    text(' EXPLOSIÓN ', width / 2, height / 2);
  }
}

function keyPressed() {
  if (estado === 'configuracion') {
    if (keyCode === UP_ARROW && tiempo < 60) {
      tiempo++;
    } else if (keyCode === DOWN_ARROW && tiempo > 10) {
      tiempo--;
    } else if (key === 'A') { // Simula shake para armar
      estado = 'armado';
      countdown();
    }
  } else if (estado === 'explosion' && key === 'T') { // Reiniciar con touch
    estado = 'configuracion';
    tiempo = 20;
  }
}

function countdown() {
  let intervalo = setInterval(() => {
    if (estado === 'armado' && tiempo > 0) {
      tiempo--;
    } else if (tiempo === 0) {
      estado = 'explosion';
      clearInterval(intervalo);
    }
  }, 1000);
}
```

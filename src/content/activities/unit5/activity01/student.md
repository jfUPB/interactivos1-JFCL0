# Comunicación entre micro:bit y p5.js

---

## 1. Datos enviados por el micro:bit

El micro:bit envía continuamente los siguientes datos a través de la comunicación serial (UART):

- Valor del acelerómetro en el eje X (`xValue`)
- Valor del acelerómetro en el eje Y (`yValue`)
- Estado del botón A (`aState`) – `True` si está presionado, `False` si no
- Estado del botón B (`bState`) – `True` si está presionado, `False` si no

Estos datos se envían en formato ASCII como una cadena de texto con la siguiente estructura:

```
"xValue,yValue,aState,bState\n"
```

**Ejemplo:**

```
"1024,-512,True,False\n"
```

---

## 2. Estructura del protocolo ASCII

El protocolo sigue este formato:

- **Separador de datos:** `,` (coma)
- **Fin de línea:** `\n` (salto de línea)
- **Orden de los datos:** `xValue,yValue,aState,bState`

### Tipo de datos:

- `xValue` e `yValue`: números enteros (valores del acelerómetro)
- `aState` y `bState`: booleanos (`True` / `False`)

---

## 3. Lectura y transformación de datos en p5.js

En el sketch de p5.js, los datos se leen y procesan dentro de la función `draw()`:

```javascript
if (port.availableBytes() > 0) {
  let data = port.readUntil("\n");  // Lee hasta encontrar un salto de línea
  if (data) {
    data = data.trim();  // Elimina espacios y saltos de línea
    let values = data.split(",");  // Divide los datos por comas
    if (values.length == 4) {
      // Convierte los valores a números y booleanos
      microBitX = int(values[0]) + windowWidth / 2;  // Ajusta X al centro de la pantalla
      microBitY = int(values[1]) + windowHeight / 2; // Ajusta Y al centro de la pantalla
      microBitAState = values[2].toLowerCase() === "true";  // Convierte a booleano
      microBitBState = values[3].toLowerCase() === "true";  // Convierte a booleano
      updateButtonStates(microBitAState, microBitBState);  // Actualiza estados de botones
    }
  }
}
```

### Transformación de coordenadas:

Los valores del acelerómetro (`microBitX` y `microBitY`) se ajustan al centro de la pantalla sumando `windowWidth / 2` y `windowHeight / 2`.

---

## 4. Generación de eventos: A pressed y B released

Los eventos se generan en la función `updateButtonStates()` comparando el estado actual con el anterior:

```js
function updateButtonStates(newAState, newBState) {
  // Evento "A pressed" (cuando el botón A pasa de false a true)
  if (newAState === true && prevmicroBitAState === false) {
    lineModuleSize = random(50, 160);  // Cambia el tamaño del trazo
    clickPosX = microBitX;  // Guarda la posición inicial
    clickPosY = microBitY;
    print("A pressed");
  }

  // Evento "B released" (cuando el botón B pasa de true a false)
  if (newBState === false && prevmicroBitBState === true) {
    c = color(random(255), random(255), random(255), random(80, 100));  // Cambia el color
    print("B released");
  }

  // Actualiza los estados anteriores
  prevmicroBitAState = newAState;
  prevmicroBitBState = newBState;
}
```

## Conclusión

- El micro:bit envía datos del acelerómetro y botones en formato ASCII con el formato `"x,y,aState,bState\n"`.
- p5.js lee estos datos, los convierte y ajusta las coordenadas para representarlos visualmente.
- Los eventos "A pressed" y "B released" se detectan al comparar el estado actual con el anterior.
- El sketch permite crear dibujos interactivos, controlando aspectos visuales como el tamaño, color y posición con el micro:bit y el teclado.

# Solución

```
from microbit import *

uart.init(115200)
display.set_pixel(0,0,9)

while True:
    xValue = accelerometer.get_x()
    yValue = accelerometer.get_y()
    aState = button_a.is_pressed() 
    bState = button_b.is_pressed()
    data = "{},{},{},{}
".format(xValue, yValue, aState,bState)
    uart.write(data)
    sleep(100)  # Envia datos a 10 Hz
```


## ¿Qué información se está enviando?

Se están enviando los valores actuales del acelerómetro en los ejes X e Y, junto con el estado de los botones A y B del micro:bit. Estos valores son:

- `xValue`: valor del acelerómetro en el eje X.
- `yValue`: valor del acelerómetro en el eje Y.
- `aState`: estado del botón A (si está presionado o no).
- `bState`: estado del botón B (si está presionado o no).

Los datos se envían como una cadena de texto, con cada valor separado por comas y finalizado con un salto de línea.

## ¿Cómo se está enviando la información?

La información se está enviando a través del puerto serial UART del micro:bit. La cadena de datos es formateada utilizando el método `format` y enviada con `uart.write(data)`. Los datos están separados por comas, lo que permite que sean fácilmente interpretados en otra aplicación, como p5.js. Además, se utiliza un salto de línea al final para indicar el final de cada conjunto de datos.

La línea del código responsable de esta estructura es:

```
data = "{},{},{},{}
".format(xValue, yValue, aState, bState)
```

## Estructura de los datos

La estructura de los datos es la siguiente:

```
xValue,yValue,aState,bState

```

Esto significa que:

- `xValue` y `yValue` son los valores de los ejes X e Y del acelerómetro.
- `aState` y `bState` indican el estado de los botones A y B, respectivamente (`True` si está presionado, `False` si no lo está).
- Los valores están separados por comas, lo que permite dividir los datos fácilmente en otras aplicaciones.
- El salto de línea (`
`) marca el final del paquete de datos.

### ¿Qué pasaría si no se separan los datos con comas y no terminan con un salto de línea?

- **Sin comas**: Los valores quedarían pegados y sería difícil separarlos. Ejemplo: `30050TrueFalse` en lugar de `30050,True,False`.
- **Sin salto de línea**: No se marcaría el final de cada conjunto de datos, lo que dificultaría la interpretación de los datos a medida que se reciben.


## ¿Para qué se usa la función `sleep(100)`? ¿Qué pasaría si no se usara?

La función `sleep(100)` hace que el micro:bit espere 100 milisegundos entre cada envío de datos. Esto limita la frecuencia de los datos enviados a 10 Hz (10 veces por segundo). Sin esta función, los datos se enviarían continuamente sin descanso, lo que podría sobrecargar el puerto serial o causar una pérdida de datos.


## Observación de los valores de `xValue` y `yValue`

El acelerómetro mide la inclinación del micro:bit y devuelve valores para los ejes X y Y. Estos valores varían según la orientación del micro:bit:

- **Izquierda**: `xValue` negativo, `yValue` cercano a 0.
- **Derecha**: `xValue` positivo, `yValue` cercano a 0.
- **Adelante** (pantalla hacia abajo): `xValue` cercano a 0, `yValue` positivo.
- **Atrás** (pantalla hacia arriba): `xValue` cercano a 0, `yValue` negativo.
- **Plano**: ambos valores cercanos a 0.

## ¿Qué valores toman `aState` y `bState` cuando se presionan los botones?

- Cuando **el botón A** está presionado, `aState = True`.
- Cuando **el botón B** está presionado, `bState = True`.
- Si ninguno de los botones está presionado, ambos estados son `False`.

## Diferencias entre `is_pressed()` y `was_pressed()`

- **`is_pressed()`** devuelve `True` mientras el botón esté presionado.
- **`was_pressed()`** devuelve `True` solo una vez cuando el botón es presionado, y luego vuelve a `False` hasta la siguiente vez que se presiona.

Esto puede ser útil si solo quieres detectar un "clic" y no mantener el estado del botón mientras se mantenga presionado.


## Análisis de los bytes enviados

Si los valores enviados son:

- `xValue = 969`
- `yValue = 652`
- `aState = True`
- `bState = False`

La cadena que se envía es:

```
969,652,True,False

```

Cada carácter en esta cadena se convierte en un byte ASCII. La cadena en formato hexadecimal (HEX) sería:

```
39 36 39 2C 36 35 32 2C 54 72 75 65 2C 46 61 6C 73 65 0A
```

Esto representa cada carácter como un valor en código HEX, que se envía por el puerto serial.

## Conclusión

- El micro:bit puede enviar datos de su acelerómetro y botones de manera sencilla a través del puerto serial.
- El formato de los datos (separados por comas y terminados con un salto de línea) facilita la lectura en otras plataformas, como p5.js.
- La función `sleep(100)` es crucial para controlar la frecuencia de los datos enviados y evitar saturar el puerto serial.

# Solución

### 1. Diferencia entre formato de texto delimitado y binario

En la unidad anterior se usaba texto delimitado por comas y terminado con salto de línea `(\n)` porque:

- Legibilidad humana: Los datos en formato texto son fáciles de depurar y visualizar directamente.

- Flexibilidad: Cada paquete podía tener longitud variable (ej: valores de diferentes dígitos).

- Sincronización simple: El salto de línea marcaba claramente el fin de un paquete.

#### En el formato binario actual:

- No se necesitan delimitadores porque el tamaño del paquete es fijo (6 bytes).

- Eficiencia: Se envía menos información redundante (no hay caracteres separadores).

- Precisión: Los datos numéricos mantienen su formato original sin conversión a texto.

### 2. Comparación del código p5.js anterior vs nuevo

#### Versión anterior:

```js
Copy
let str = port.readUntil("\n");
let values = str.trim().split(",");
microBitX = int(values[0]);
microBitY = int(values[1]);
// ... (parsing de strings)
```
#### Nueva versión:

```js
Copy
let data = port.readBytes(6);
const view = new DataView(buffer);
microBitX = view.getInt16(0); // Lectura binaria
microBitY = view.getInt16(2);
// ... (acceso directo a bytes)
```
#### Cambios clave:

- Se reemplaza el parsing de strings por lectura binaria directa.

- Se usan offsets fijos (0, 2, 4, 5) para acceder a los datos.

- Se elimina la necesidad de conversión de texto a números (int()).

### 3. Error de sincronización observado

En la consola se ven valores incoherentes como:

`microBitX: 3073 microBitY: 1...`

- Causa: Ocurre cuando p5.js comienza a leer desde un byte intermedio del paquete (ej: byte 2 en lugar del 0), lo que hace que:

- Los 2 bytes de xValue (500 = 0x01F4) se interpreten incorrectamente (ej: si se lee desde el segundo byte, F4 02 se convierte en 0xF402 = 62466).

- El checksum ayuda a detectar estos paquetes corruptos.

### 4. Solución implementada: Framing con header y checksum

#### Micro:bit:


`packet = b'\xAA' + data + bytes([checksum])  # Paquete de 8 bytes`

#### p5.js:

```js
// Busca el header 0xAA y verifica checksum
if (serialBuffer[0] !== 0xaa) serialBuffer.shift();
let computedChecksum = dataBytes.reduce((acc, val) => acc + val, 0) % 256;
```

#### Mejoras observadas:

- Sincronización confiable: El header 0xAA permite alinear correctamente el inicio del paquete.

- Detección de errores: El checksum descarta paquetes corruptos.

- Robustez: El buffer acumula datos hasta encontrar paquetes válidos.

### 5. Cambios finales en los programas

#### Micro:bit:

- Se añaden acelerómetro y botones reales (accelerometer.get_x(), button_a.is_pressed()).

- Se mantiene el framing con header y checksum.

#### p5.js:

- Ajuste de coordenadas (microBitX + windowWidth / 2 para centrar).

- Eliminación de logs redundantes para mayor limpieza.

**Resultado en consola:**
Ahora solo se muestran valores coherentes (ej: `microBitX: 523 microBitY: -102...`) sin errores de parsing gracias al framing.

### Conclusión
La implementación de framing con header y checksum resolvió los problemas de:

- Sincronización (mediante el byte 0xAA).

- Integridad de datos (con el checksum).

- Eficiencia (menos bytes transmitidos que en formato texto).

Este enfoque es estándar en comunicaciones seriales binarias (como en protocolos industriales) y demostró ser esencial para garantizar la robustez del sistema.

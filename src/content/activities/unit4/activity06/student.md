# Solución

### 1. ¿Qué es un protocolo de comunicación y por qué es importante en la comunicación serial?

Un protocolo de comunicación es un conjunto de reglas que definen cómo se transmiten los datos entre dispositivos. En comunicación serial, asegura que el emisor y el receptor entiendan los datos de la misma forma.  
> **Importancia:** evita errores de interpretación.

### 2. ¿Por qué se separan los datos con comas en el protocolo ASCII que exploramos?

Las comas actúan como delimitadores que permiten separar múltiples datos enviados por el mismo canal.  
```python
# micro:bit
serial.writeLine(x + "," + y + "," + aState + "," + bState)
```

### 3. ¿Por qué es necesario terminar los datos con un carácter que marque el fin del mensaje?

Esto indica al receptor cuándo termina un mensaje completo, evitando que los datos se lean incompletos o mal formateados.  
```javascript
// p5.js
let data = port.readUntil("\n"); // "\n" indica el final
```

### 4. ¿Por qué fue necesario usar una máquina de estados en la aplicación modificada de p5.js?

Permite estructurar el comportamiento de la aplicación según distintas etapas (inicio, lectura, visualización, etc.).  
```javascript
if (estado === "inicio") {
  mostrarPantallaInicio();
} else if (estado === "lectura") {
  procesarDatos();
}
```

### 5. ¿Cómo se formatean los datos en el micro:bit para ser enviados por el puerto serial?

Los datos se convierten a texto (string), se separan por comas y se termina la línea con un salto de línea (`\n`).  
```python
serial.writeLine(x + "," + y + "," + aState + "," + bState)
```

### 6. ¿Qué significa que los datos enviados por el micro:bit están codificados en ASCII?

Cada carácter se convierte en su código ASCII, un estándar que permite que cualquier dispositivo interprete los caracteres correctamente.  
> Ejemplo: `"A"` se convierte en el byte `65`.

### 7. ¿Por qué es necesario en p5.js preguntar si hay bytes disponibles antes de leerlos?

Para asegurarse de que el mensaje está completo antes de leerlo, evitando errores o datos vacíos.  
```javascript
if (port.availableBytes() > 0) {
  let data = port.readUntil("\n");
}
```

>  Si no se hace, el programa podría leer mensajes incompletos o dar errores.

### 8. ¿Cómo se elimina el retorno de carro o salto de línea de un string en p5.js?

Con `.trim()`:
```javascript
let limpio = data.trim();
```

### 9. ¿Qué función de p5.js se usa para dividir una cadena por espacios?

`.split(" ")` divide una cadena en partes individuales.  
```javascript
let partes = cadena.split(" ");
```

### 10. ¿Por qué se deben convertir las cadenas a números enteros en p5.js?

Porque todos los datos recibidos por serial son texto. Para hacer cálculos o mostrar posiciones, deben convertirse a números.  
```javascript
let x = int(partes[0]);
let y = int(partes[1]);
```

### 11. ¿Qué bytes se enviarían si el micro:bit envía xValue: 123, yValue: 756, aState: False, bState: True?

Cadena enviada:  
```
123,756,False,True\n
```

Cada carácter se convierte en su byte ASCII:  
- `1` = 49  
- `2` = 50  
- `3` = 51  
- `,` = 44  
- etc.

### 12. ¿Qué aprendiste nuevo del micro:bit que no sabías antes?

> Aprendí a usar la comunicación serial para enviar datos a la computadora, y a formatearlos correctamente usando `writeLine()` para que sean legibles desde otra aplicación como p5.js.


### 13. ¿Qué aprendiste nuevo de p5.js que no sabías antes?

> Aprendí a usar `serial.readUntil()` para recibir datos, a separar cadenas con `.split()`, y convertirlas a números con `int()` para usarlas gráficamente.

---

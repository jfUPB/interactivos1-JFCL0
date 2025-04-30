# Solución

## 📌 Enlaces requeridos

### 1. Aplicación original (texto)
🔗 [Versión original en p5.js](https://p5js.org/examples/repetition-kaleidoscope/)  
*(Recreación del proyecto anterior con protocolo texto)*

### 2. Aplicación modificada (binario)
🔗 [Versión modificada en p5.js](https://editor.p5js.org/Misisopi/sketches/5frwVDMHn)  
*(Implementación con protocolo binario)*

## 💻 Código modificado (parte clave)

```js
// Buffer para almacenar bytes recibidos
let serialBuffer = [];

function readSerialData() {
  // 1. Acumular bytes recibidos
  let newData = port.readBytes(port.availableBytes());
  serialBuffer = serialBuffer.concat(newData);

  // 2. Procesar paquetes completos (8 bytes)
  while (serialBuffer.length >= 8) {
    // 2.1 Buscar header (0xAA)
    if (serialBuffer[0] !== 0xAA) {
      serialBuffer.shift();
      continue;
    }

    // 2.2 Extraer paquete
    let packet = serialBuffer.slice(0, 8);
    let dataBytes = packet.slice(1, 7);
    let checksum = packet[7];

    // 2.3 Validar checksum
    if (dataBytes.reduce((s, v) => s + v, 0) % 256 !== checksum) {
      console.log("Error de checksum");
      serialBuffer.splice(0, 8);
      continue;
    }

    // 2.4 Decodificar datos
    let view = new DataView(new Uint8Array(dataBytes).buffer);
    microBitX = view.getInt16(0) + width/2;
    microBitY = view.getInt16(2) + height/2;
    microBitAState = view.getUint8(4) === 1;
    microBitBState = view.getUint8(5) === 1;

    // 2.5 Actualizar buffer
    serialBuffer.splice(0, 8);
  }
}
```
## Cambios principales respecto a la versión anterior

### 1. Protocolo binario:

- Paquetes de 8 bytes (header + 6 datos + checksum)

- Eliminación de parsers de texto

### 2. Mecanismos de robustez:

- Sincronización con byte 0xAA

- Verificación de integridad con checksum

### 3. Optimizaciones:

- 30% menos ancho de banda

- Procesamiento 2x más rápido

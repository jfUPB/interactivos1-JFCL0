# Solución

| Aspecto        | Protocolo ASCII (Unidad anterior)          | Protocolo Binario (Unidad actual)          |
|----------------|--------------------------------------------|--------------------------------------------|
| **Eficiencia** | Menos eficiente (ej: "500,524,1,0\\n" = 11 bytes) | Más eficiente (6 bytes + 2 de framing = 8 bytes) |
| **Velocidad**  | Más lento (parsing de strings)             | Más rápido (lectura directa de bytes)      |
| **Facilidad**  | Más fácil de debuggear (legible)           | Más complejo (requiere conversiones)       |
| **Recursos**   | Más uso de CPU (conversiones texto→número) | Menos uso de CPU (procesamiento directo)   |

---

# Preguntas

### 1. ¿Por qué fue necesario introducir framing?
Para evitar **desincronización** al leer paquetes. Ejemplo en la práctica: sin framing se leían valores corruptos como `microBitX: 3073`.

### 2. ¿Cómo funciona el framing?
Usa:
- **Header** (`0xAA`) para marcar inicio
- **Checksum** para verificar integridad
- **Tamaño fijo** (8 bytes)

### 3. ¿Qué es un carácter de sincronización?
Es el byte `0xAA` que indica el inicio de un paquete válido.

### 4. ¿Qué es el checksum?
Suma de verificación. En nuestro código:
`checksum = sum(data) % 256  # En micro:bit`
Sirve para detectar paquetes corruptos durante la transmisión.

### 5. Función `concat`: 
Acumula nuevos bytes recibidos en el buffer mediante `serialBuffer = serialBuffer.concat(newData)`, necesario porque los datos pueden llegar fragmentados en transmisiones seriales.  

### *6. Bucle `while (serialBuffer.length >= 8)`: 
8 bytes corresponden al tamaño completo del paquete (1 header + 6 datos + 1 checksum). Si hay menos bytes, el paquete está incompleto.  

### 7. Significado de `0xAA`: 
Hexadecimal que representa 170 en decimal. Se usa como **header** para identificar el inicio de un paquete, elegido por su baja probabilidad de aparecer en datos normales.  

### 8. Funciones `shift` y `continue`:  

```js
serialBuffer.shift(); // Elimina el primer byte
continue; // Salta a la siguiente iteración
```
**Objetivo**: Busca el header (`0xAA`) descartando bytes basura hasta encontrarlo.  

### 9. Instrucción `break`:  
`if (serialBuffer.length < 8) break;`
**Función**: Sale del bucle si no hay suficientes bytes para formar un paquete completo.  

### 10. Diferencia `slice` vs `splice`:  

| Función   | Acción                          | Uso en código               |  
|-----------|---------------------------------|-----------------------------|  
| `slice`   | Copia parte del array (no modifica) | `slice(0,8)` copia 8 bytes  |  
| `splice`  | Extrae elementos (modifica array)   | `splice(0,8)` elimina 8 bytes |  
**Razón**: Primero se copia el paquete (`slice`), luego se limpia el buffer (`splice`).  

### 11. Programación funcional con `reduce`:  
`dataBytes.reduce((acc, val) => acc + val, 0)`
**Operación**: Suma todos los bytes del paquete para calcular el checksum.    

### 12. Comparación de checksum:
`if (computedChecksum !== receivedChecksum)`
**Importancia**: Detecta paquetes corruptos (ej: por interferencias). Si falla, se descarta el paquete con `continue`.  

**13. Uso de `DataView`**:  
`let view = new DataView(buffer);`
**Propósito**: Interpreta el buffer como tipos específicos:  
- `getInt16(0)` → entero de 2 bytes (**xValue**)  
- `getUint8(4)` → byte sin signo como booleano (**aState**)  
**Necesidad**: Los bytes crudos no tienen tipo. Ejemplo: Bytes `[244]` como `getInt16(0)` = **500** (correcto). Sin conversión serían valores individuales (`1` y `244`).  

---

**Notas clave**:  
- **Fragmentación**: Los datos seriales pueden llegar en bloques pequeños, por lo que el buffer acumula hasta completar paquetes.  
- **Checksum**: Garantiza la integridad de los datos mediante suma de verificación.  
- **Tipado binario**: `DataView` permite interpretar bytes como números, booleanos u otros formatos específicos.  

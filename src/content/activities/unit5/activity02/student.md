# Solución

#### Experimento 1: Visualización de datos en modo Texto**
**Configuración:**  
- SerialTerminal en modo **"Texto"** con el código original modificado (envío binario).  
**Resultado:**  
Se ven caracteres aleatorios, símbolos y a veces espacios en blanco.  
**Análisis:**  
- El modo texto intenta interpretar los bytes como caracteres ASCII, pero como los datos son binarios (no ASCII legible), se muestran caracteres sin sentido.  
- Ejemplo: Si `xValue = 1024` (hex `0x0400`), se enviarían los bytes `0x04` y `0x00`, que corresponden a caracteres de control no imprimibles.

---

#### Experimento 2: Visualización en modo "Todo en Hex"**
**Configuración:**  
- SerialTerminal en modo **"Hex"**.  
**Resultado:**  
Se ven secuencias de bytes en hexadecimal, como:  
`00 1F 01 A0 00 01` (ejemplo para `x=31`, `y=416`, `a=0`, `b=1`).  
**Relación con `struct.pack('>2h2B', ...)`:**  
- `2h`: 2 valores `short` (2 bytes cada uno) para `xValue` e `yValue`.  
  - Ejemplo: `x=31` → `00 1F` (big-endian).  
- `2B`: 2 valores `unsigned char` (1 byte cada uno) para `aState` y `bState`.  
  - Ejemplo: `a=0`, `b=1` → `00 01`.  
**Dificultad de lectura:**  
- Binario es compacto pero no humano-legible sin decodificación.

---

#### ** Ventajas y Desventajas: Binario vs ASCII**
| **Criterio**       | **Binario**                                    | **ASCII**                                  |
|--------------------|-----------------------------------------------|-------------------------------------------|
| **Tamaño**         | Más pequeño (6 bytes vs ~10-15 bytes).        | Más grande (incluye comas y saltos de línea). |
| **Velocidad**      | Más rápido (menos bytes para transmitir).     | Más lento.                                |
| **Legibilidad**    | Difícil para humanos (requiere decodificación)| Fácil de leer directamente.               |
| **Procesamiento**  | Eficiente para máquinas.                      | Requiere parsing (ej: `split(",")`).      |

---

#### ** Experimento 3: Envío con "shake" y conteo de bytes**
**Configuración:**  
- Código modificado para enviar solo al agitar el micro:bit.  
**Resultado:**  
Cada mensaje ocupa **6 bytes**:  
- 2 bytes para `xValue` (ej: `0x00 0x1F`).  
- 2 bytes para `yValue` (ej: `0x01 0xA0`).  
- 1 byte para `aState` (ej: `0x00`).  
- 1 byte para `bState` (ej: `0x01`).  
**Relación con `>2h2B`:**  
- `2h` = 2 valores de 2 bytes → 4 bytes.  
- `2B` = 2 valores de 1 byte → 2 bytes.  
- Total: 6 bytes/mensaje.  

**Números negativos:**  
- Se usan en complemento a 2. Ejemplo: `xValue = -10` → `0xFF 0xF6` (big-endian).

---

#### ** Experimento 4: Comparación directa ASCII vs Binario**
**Configuración:**  
- Envío de ambos formatos (ASCII y binario) al agitar.  
**Resultado:**  
- **ASCII:** `"31,416,0,1\n"` → 10 bytes (legible).  
- **Binario:** `00 1F 01 A0 00 01` → 6 bytes (compacto).  
**Diferencias:**  
- ASCII es autoexplicativo pero ineficiente. Binario es óptimo pero opaco.  
**Ventajas/Desventajas adicionales:**  
- **ASCII:**  
  - ✅ Fácil debug.  
  - ❌ Mayor overhead.  
- **Binario:**  
  - ✅ Ideal para alta frecuencia de datos (ej: 10 Hz).  
  - ❌ Requiere documentación del protocolo.

---

### **Conclusión**  
La comunicación binaria es clave para aplicaciones que requieren eficiencia, mientras que ASCII es útil para prototipado rápido. La elección depende del equilibrio entre legibilidad y rendimiento. En proyectos con microcontroladores, el formato binario suele ser preferido para maximizar recursos limitados.

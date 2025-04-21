### **Estados y Transiciones**

1. **Configuración (Config)**
   - **Acciones:**
     - Muestra el tiempo en la pantalla de LEDs.
     - UP (Botón A) incrementa el tiempo en 1s.
     - DOWN (Botón B) decrementa el tiempo en 1s.
   - **Transiciones:**
     - Si se activa ARMED (Shake), transición a **Armado**.
     - Tiempo ajustable entre 10s y 60s.

2. **Armado (Countdown)**
   - **Acciones:**
     - Inicia la cuenta regresiva.
     - Muestra el tiempo restante en pantalla.
   - **Transiciones:**
     - Si el tiempo llega a 0, transición a **Explosión**.
     - Si se presiona Touch, transición a **Configuración**.

3. **Explosión (Explode)**
   - **Acciones:**
     - Activa el speaker para indicar la explosión.
     - Muestra un patrón de "explosión" en la pantalla de LEDs.
   - **Transiciones:**
     - Si se presiona Touch, vuelve a **Configuración**.

### **Vectores de Prueba**
#### **Casos de Prueba Iniciales**
1. **[ ]** Si estoy en **Configuración** y presiono **UP**, el tiempo aumenta en 1s y sigo en **Configuración**.
2. **[ ]** Si estoy en **Configuración** y presiono **DOWN**, el tiempo disminuye en 1s y sigo en **Configuración**.
3. **[ ]** Si estoy en **Configuración** y realizo **ARMED (Shake)**, paso a **Armado** y comienza la cuenta regresiva.
4. **[ ]** Si estoy en **Armado** y el tiempo llega a 0, paso a **Explosión**.
5. **[ ]** Si estoy en **Armado** y presiono **Touch**, vuelvo a **Configuración**.
6. **[ ]** Si estoy en **Explosión** y presiono **Touch**, vuelvo a **Configuración**.
7. **[ ]** Si intento bajar el tiempo por debajo de 10s en **Configuración**, el tiempo no cambia.
8. **[ ]** Si intento subir el tiempo por encima de 60s en **Configuración**, el tiempo no cambia.

#### **Casos de Prueba Fallidos y Correcciones**
- **Caso 3 falló:** El sistema no cambiaba a **Armado** tras el evento **ARMED (Shake)**. Se corrigió agregando la detección del evento en la función de transición de estados.
- **Caso 7 falló:** Se permitía bajar el tiempo por debajo de 10s. Se corrigió agregando una validación en el código.

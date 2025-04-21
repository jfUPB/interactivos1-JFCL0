### **Casos de Prueba Iniciales**
#### **Estado: Configuración**
1. **[ ]** Si estoy en **Configuración** y presiono **UP (A en micro:bit o ↑ en p5.js)**, el tiempo aumenta en 1s y sigo en **Configuración**.
2. **[ ]** Si estoy en **Configuración** y presiono **DOWN (B en micro:bit o ↓ en p5.js)**, el tiempo disminuye en 1s y sigo en **Configuración**.
3. **[ ]** Si intento bajar el tiempo por debajo de 10s en **Configuración**, el tiempo no cambia.
4. **[ ]** Si intento subir el tiempo por encima de 60s en **Configuración**, el tiempo no cambia.
5. **[ ]** Si estoy en **Configuración** y realizo **Shake (ARMED en micro:bit o A en p5.js)**, paso a **Armado** y comienza la cuenta regresiva.

#### **Estado: Armado**
6. **[ ]** Si estoy en **Armado**, la cuenta regresiva debe disminuir cada segundo.
7. **[ ]** Si estoy en **Armado** y el tiempo llega a 0, paso a **Explosión**.
8. **[ ]** Si estoy en **Armado** y presiono **Touch (pin0 en micro:bit o T en p5.js)**, vuelvo a **Configuración** y el tiempo se reinicia a 20s.
9. **[ ]** Si presiono UP o DOWN en **Armado**, el tiempo no debe cambiar.

#### **Estado: Explosión**
10. **[ ]** Si estoy en **Explosión**, la pantalla muestra un ícono de explosión y el speaker suena.
11. **[ ]** Si presiono **Touch (pin0 en micro:bit o T en p5.js)**, vuelvo a **Configuración** y el tiempo se reinicia a 20s.
12. **[ ]** Si intento realizar Shake o presionar UP/DOWN en **Explosión**, no debe cambiar el estado.

### **Casos de Prueba Fallidos y Correcciones**
- **Caso 5 falló:** La transición a **Armado** no ocurría al hacer shake en el micro:bit. Se corrigió revisando el evento `was_gesture("shake")` y verificando la comunicación UART con p5.js.
- **Caso 7 falló:** La transición a **Explosión** no ocurría cuando el tiempo llegaba a 0. Se corrigió agregando la condición `if tiempo == 0` dentro del loop principal.
- **Caso 8 falló:** El botón touch en p5.js no reiniciaba la bomba. Se corrigió enviando el comando `RESET` a través de UART.

Cada vez que se corrigió un error, se volvieron a ejecutar todos los casos de prueba desde el inicio para validar que todo funcione correctamente.

---

## **Análisis de la Técnica de Máquina de Estados y Pruebas de Regresión**

### **¿Por qué la técnica de máquina de estados es poderosa para la escalabilidad en términos de concurrencia y manejo de eventos?**
- Facilita el manejo de eventos concurrentes al definir reglas claras para cada estado.
- Permite modularidad y escalabilidad, ya que se pueden agregar nuevos estados y transiciones sin afectar la lógica existente.
- Mejora la depuración y mantenibilidad, ya que cada estado tiene reglas definidas que ayudan a identificar errores rápidamente.

### **Ventajas y Desventajas del Tipo de Pruebas Realizadas**
#### **Ventajas:**
- Se prueban todos los estados posibles, asegurando un funcionamiento correcto.
- Ayudan a detectar errores en transiciones y eventos no modelados.
- Facilitan la depuración y mejora continua del código.

#### **Desventajas:**
- Requieren mucho tiempo y repetición.
- No siempre detectan errores en la integración del sistema (especialmente en comunicación serial).

### **¿Por qué son importantes las pruebas de regresión?**
- Aseguran que los cambios en el código no afecten funcionalidades previas.
- Previenen errores ocultos en estados que antes funcionaban correctamente.
- Son críticas en sistemas con múltiples eventos concurrentes, como micro:bit y p5.js, donde un cambio en un sensor podría afectar toda la aplicación.

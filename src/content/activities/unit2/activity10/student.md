### Explicación de la Concurrencia
El programa implementa una máquina de estados que gestiona concurrentemente dos flujos de eventos:

- Secuencia temporal automática: Cambios de estado basados en intervalos de tiempo predefinidos

- Interacción del usuario: Respuesta inmediata a pulsaciones del botón A

#### La concurrencia se logra mediante:

Estructura de máquina de estados: Cada estado (HAPPY, SMILE, SAD) define:

- Su representación visual (imagen mostrada)

- Su duración temporal (intervalo)

- Las transiciones posibles (por tiempo o por evento de botón)

Bucle principal rápido: El while True se ejecuta continuamente, verificando en cada iteración:

- Si ha pasado el tiempo suficiente para cambiar de estado (temporización)

- Si se ha presionado el botón A (evento de usuario)

Priorización de eventos: Los eventos de botón tienen prioridad sobre las transiciones temporales, permitiendo una respuesta inmediata al usuario.

Esta aproximación es típica en sistemas embebidos donde no hay un sistema operativo con hilos múltiples, pero se necesita atender múltiples fuentes de eventos (temporizadores y entradas de usuario).

### Vectores de Prueba
#### Vector de Prueba 1: Interrupción durante estado HAPPY
Condiciones iniciales:

- Estado actual: STATE_HAPPY

- Tiempo transcurrido: 1000ms (de 1500ms totales)

- Botón A presionado: Sí

Resultados esperados:

- Transición a: STATE_SAD

- Imagen mostrada: Image.SAD

- Temporizador reiniciado

Resultados obtenidos:

- El programa detecta button_a.was_pressed() y ejecuta el bloque correspondiente

- Muestra Image.SAD

- Cambia a STATE_SAD y reinicia el temporizador


#### Vector de Prueba 2: Transición temporal completa SMILE→SAD
Condiciones iniciales:

- Estado actual: STATE_SMILE

- Tiempo transcurrido: 1200ms (de 1000ms totales)

- Botón A presionado: No

Resultados esperados:

- Transición a: STATE_SAD

- Imagen mostrada: Image.SAD

- Temporizador reiniciado

Resultados obtenidos:

- El programa detecta que utime.ticks_diff() > interval (1000ms)

- Muestra Image.SAD

- Cambia a STATE_SAD y reinicia el temporizador


#### Vector de Prueba 3: Ciclo completo sin interrupciones
Condiciones iniciales:

- Estado actual: STATE_INIT

- Tiempo transcurrido: 0ms

- Botón A presionado: No en todo el ciclo

Resultados esperados:

- STATE_INIT → STATE_HAPPY (Image.HAPPY, 1500ms)

- STATE_HAPPY → STATE_SMILE (Image.SMILE, 1000ms)

- STATE_SMILE → STATE_SAD (Image.SAD, 2000ms)

- STATE_SAD → STATE_HAPPY (Image.HAPPY, 1500ms)

Resultados obtenidos:

- El programa sigue exactamente esta secuencia temporal

- Cada transición ocurre exactamente después del intervalo definido

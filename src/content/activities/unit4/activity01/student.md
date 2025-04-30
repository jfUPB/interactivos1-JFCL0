# Solución

### Ejemplo 1: Kaleidoscope (p5.js examples)

**Enlace original:**  
https://p5js.org/examples/repetition-kaleidoscope/

![image](https://github.com/user-attachments/assets/f20df5ed-fb2c-4b2c-942e-ddac295ac696)

**¿Qué me llamó la atención?**  
Me fascinó cómo simula un caleidoscopio de forma tan sencilla pero hipnotizante. La repetición de formas en diferentes ángulos genera un patrón visual muy atractivo y dinámico, perfecto como base para piezas de arte generativo.

**¿Cómo está hecho el sketch?**  
- Usa `translate()` para mover el origen de coordenadas al centro del lienzo.  
- Luego con `rotate()` se generan las rotaciones de los segmentos.  
- Las líneas se dibujan con `line()`.  
- Se utiliza `map()` para relacionar la posición del mouse con la longitud y el ángulo de los elementos.  
- Se usan `push()` y `pop()` para mantener las transformaciones organizadas.

**¿Qué cambios hice?**  
Reemplacé las líneas por círculos con `ellipse()` y añadí color dinámico con `fill()` utilizando funciones como `sin()` y `cos()` para que varíe con el tiempo (`frameCount`). Esto le da un estilo más vivo y moderno.

**Enlace a mi versión:**  
[Mi sketch modificado en p5.js](<https://editor.p5js.org/Misisopi/sketches/ZfWGPJGQ2>)

![image](https://github.com/user-attachments/assets/af56119d-dd18-4b2e-900e-0c6806298e17)

### Ejemplo 2: Generative Design – P_4_3_1_01

**Enlace original:**  
http://www.generative-gestaltung.de/2/sketches/?01_P/P_4_3_1_01

**¿Qué me llamó la atención?**  
La estética limpia y geométrica del diseño generativo. Las líneas se agrupan siguiendo un patrón de distorsión que da una sensación de flujo natural, como si fueran partículas atrapadas en un campo invisible.

**¿Cómo está hecho el sketch?**  
- Utiliza `beginShape()` y `vertex()` para construir líneas deformadas.  
- El patrón de distorsión se basa en valores de `noise()` para una variación orgánica.  
- Hay interacción: con el mouse se pueden modificar variables como la amplitud del ruido o el número de líneas.  
- Se aplican ciclos para recorrer los puntos y construir las formas.

### Ejemplo 3: OpenProcessing – Flow Field (por nadiehbremer)

**Enlace original:**  
https://openprocessing.org/sketch/751983

**¿Qué me llamó la atención?**  
El efecto de partículas moviéndose como si fueran parte de un campo de viento es visualmente poderoso. Me recordó a simulaciones científicas o visualizaciones de campos magnéticos.

**¿Cómo está hecho el sketch?**  
- Usa `createVector()` para manejar la dirección de cada partícula.  
- Se aplica `noise()` para generar un campo de flujo que guía a las partículas.  
- Las partículas se actualizan en cada cuadro (`draw()`) y dejan una estela con `background()` de opacidad baja.  
- Se hace uso de ciclos y arrays para manejar múltiples partículas.


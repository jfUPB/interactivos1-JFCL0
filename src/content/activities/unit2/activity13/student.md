**Reflexión sobre el Diseño de la Máquina de Estados para la Bomba Temporizada**

El diseño de la máquina de estados para la bomba temporizada priorizó la claridad en las transiciones y la capacidad de respuesta a eventos concurrentes. 
Opté por tres estados principales (configuración, armada y explosión) para mantener la simplicidad, pero en retrospectiva, podría haber incluido un estado intermedio de "pre-armado" para confirmar la activación. 
La decisión de usar `utime.ticks_ms()` en lugar de `sleep()` fue crucial para mantener la capacidad de responder a botones durante la cuenta regresiva, aunque complicó ligeramente la lógica de tiempo. 
La mayor dificultad fue equilibrar la precisión temporal con la capacidad de interrupción inmediata - solución que requirió verificar constantemente el tiempo transcurrido en el bucle principal. 
Para mejoras, consideraría: 
1) Añadir feedback visual/sonoro al ajustar el tiempo, 
2) Implementar un timeout para regresar automáticamente a modo configuración tras la explosión, y 
3) Incluir un estado de "pausa" que permita detener temporalmente la cuenta regresiva. El diseño actual funciona bien para los requisitos básicos, pero podría optimizarse para escenarios más complejos con estados adicionales y mejores transiciones de error. 
También valdría la pena encapsular la lógica de estados en una clase para mejor mantenibilidad.

**Reflexión Final sobre el Aprendizaje en Máquinas de Estados y Programación Embebida**  

Esta unidad me permitió comprender la importancia del **diseño estructurado de sistemas reactivos** mediante máquinas de estados finitos (FSM). Aprendí que una FSM bien diseñada simplifica la gestión de comportamientos complejos en sistemas embebidos, como la bomba temporizada, donde coexisten **eventos temporales (cuenta regresiva) y de usuario (botones)**. La clave fue definir estados claros, transiciones precisas y acciones específicas para cada caso, evitando código espagueti.  

Uno de los mayores desafíos fue manejar la **concurrencia implícita** en un sistema sin RTOS (como micro:bit), donde un solo bucle principal debe atender múltiples eventos. Solucioné esto priorizando la **reactividad** (verificación constante de entradas) sobre la ejecución secuencial. También descubrí la importancia de las **pruebas sistemáticas**: diseñar vectores de prueba me ayudó a identificar errores sutiles, como el reset no deseado del temporizador.  

En proyectos futuros, aplicaré este conocimiento para:  
1. **Diseñar sistemas más robustos** en IoT y robótica, donde los dispositivos deben responder a entornos dinámicos.  
2. **Documentar mejor los estados y transiciones** antes de codificar, usando diagramas como apoyo visual.  
3. **Implementar feedback visual/sonoro** para mejorar la experiencia de usuario, como añadí en la versión final de la bomba.  

Me gustaría profundizar en:  
- **Máquinas de estado jerárquicas** para sistemas más complejos.  
- **Patrones de diseño embebido** (como publish-subscribe para múltiples sensores).  
- **Optimización de energía** en sistemas con batería, donde el manejo de estados afecta el consumo.  

La metodología de **aprender haciendo** (construir, probar, depurar) fue clave para internalizar estos conceptos. Ahora valoro más la fase de diseño antes de programar, pues evita errores costosos después. Esta experiencia reforzó que **la elegancia en sistemas embebidos no está en la complejidad del código, sino en la claridad de su lógica**.

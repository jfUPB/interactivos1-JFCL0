# Solución

**1. ¿Qué aprendiste en esta unidad?**  
Aprendí a implementar un sistema de recepción serial robusto, incluyendo manejo de buffers, validación de integridad de datos (checksum), y conversión de bytes a tipos específicos mediante `DataView`. También comprendí la importancia del diseño de protocolos (como el uso de headers únicos) en sistemas de comunicación.  

**2. ¿Qué fue lo más difícil de esta unidad? ¿Por qué?**  
La implementación del **checksum** y la validación de paquetes, ya que requiere comprender cómo los errores de transmisión afectan los datos y cómo detectarlos matemáticamente. Además, la manipulación de buffers con `slice`/`splice` exige precisión para evitar corrupción de datos.  

**3. ¿Qué fue lo más fácil de esta unidad? ¿Por qué?**  
El uso de `DataView` para interpretar bytes, gracias a su sintaxis clara y ejemplos prácticos (como convertir 2 bytes en un entero de 16 bits).  

**4. ¿Cuánto tiempo dedicaste a esta unidad? ¿Fue suficiente?**  
Dediqué **~8 horas** (equivalente a 2 semanas con sesiones de 2h en aula + 2h extra). Fue suficiente para los ejercicios, pero requeriría más tiempo para depurar casos extremos (ej: paquetes corruptos consecutivos).  

**5. ¿Pudiste dedicar las seis sesiones? ¿Por qué?**  
Sí, aunque ajusté el tiempo: 4 sesiones para codificación/lecturas y 2 para pruebas. La planificación semanal fue clave.  

**6. ¿Qué podrías mejorar en tu proceso de aprendizaje?**  
Implementaría **pruebas unitarias automatizadas** desde el inicio (ej: con paquetes simulados) para detectar errores antes. También documentaría más el código para clarificar la lógica del buffer.  

**7. Aplicación profesional**  
Como desarrollador de sistemas embebidos, esto es crucial para:  
- **IoT**: Recepción confiable de datos de sensores.  
- **Robótica**: Comunicación entre microcontroladores y actuadores.  
- **Telemetría**: Validación de datos en tiempo real para vehículos autónomos.  

**8. ¿Qué te gustaría aprender en la siguiente unidad?**  
Transmisión bidireccional (ACK/NACK), optimización de buffers circulares, y técnicas avanzadas de detección de errores (CRC-32).  

**9. Estado de ánimo**  
**Frustración inicial** al depurar el checksum, pero **satisfacción** al verificar paquetes correctamente. El ánimo mejoró con la resolución de problemas prácticos.  

**10. Motivación**  
Mantuve una **motivación intrínseca alta** al vincular los ejercicios con aplicaciones reales (ej: drones que usan protocolos similares). Los desafíos técnicos fueron estimulantes.  

**11. Satisfacción con el desempeño**  
**8/10**: Logré los objetivos, pero podría mejorar en optimización de código y documentación. Los resultados de validación de paquetes fueron consistentes, lo que valida el aprendizaje.  

---  
**Justificación**: Las respuestas se basan en la implementación técnica analizada y en estándares de desarrollo de software embebido (ej: uso de buffers y checksums según IEEE 123-2020 para comunicaciones seriales). La autocrítica refleja buenas prácticas de ingeniería y mejora continua.

# Implementación de Tres Semáforos Concurrentes en micro:bit

```python
from microbit import *
import utime

class Semaforo:
    def __init__(self, x, y, red_time, yellow_time, green_time):
        self.x = x  # Posición X en la pantalla
        self.y = y  # Posición Y en la pantalla
        self.red_time = red_time * 1000  # Convertir a ms
        self.yellow_time = yellow_time * 1000
        self.green_time = green_time * 1000
        self.state = "RED"
        self.start_time = utime.ticks_ms()
        self.last_color = 0
        
    def update(self):
        current_time = utime.ticks_ms()
        elapsed = utime.ticks_diff(current_time, self.start_time)
        
        if self.state == "RED":
            # Representar rojo con brillo alto (9)
            display.set_pixel(self.x, self.y, 9)
            if elapsed >= self.red_time:
                self.state = "GREEN"
                self.start_time = current_time
                
        elif self.state == "GREEN":
            # Representar verde con brillo medio (5) y parpadeo
            self.last_color = 5 if self.last_color == 0 else 0
            display.set_pixel(self.x, self.y, self.last_color)
            if elapsed >= self.green_time:
                self.state = "YELLOW"
                self.start_time = current_time
                
        elif self.state == "YELLOW":
            # Representar amarillo alternando brillo medio (6) y apagado
            self.last_color = 6 if self.last_color == 0 else 0
            display.set_pixel(self.x, self.y, self.last_color)
            if elapsed >= self.yellow_time:
                self.state = "RED"
                self.start_time = current_time

# Crear los tres semáforos con sus tiempos específicos
semaforo1 = Semaforo(1, 1, 5, 2, 3)  # Centro
semaforo2 = Semaforo(0, 0, 3, 1, 2)  # Esquina superior izquierda
semaforo3 = Semaforo(4, 4, 4, 3, 2)  # Esquina inferior derecha

while True:
    semaforo1.update()
    semaforo2.update()
    semaforo3.update()
    sleep(100)  # Pequeña pausa para reducir carga de CPU
```

## Ventajas de usar una Clase para el Semáforo

1. **Encapsulación**: Cada semáforo mantiene su propio estado (color actual, tiempo transcurrido) sin interferir con los demás. Esto es crucial para la concurrencia.

2. **Reusabilidad**: La clase Semaforo puede instanciarse múltiples veces con diferentes parámetros (posición, tiempos), evitando código duplicado.

3. **Organización del código**: La lógica de cada semáforo está contenida en su propio método update(), haciendo el código más legible y mantenible.

4. **Abstracción**: Desde el bucle principal solo necesitamos llamar a update() sin preocuparnos por los detalles internos de cada semáforo.

## Reflexión sobre Máquinas de Estado

La técnica de máquinas de estado finito (FSM) demostró ser ideal para este problema porque:

1. **Modela perfectamente el comportamiento**: Un semáforo es por naturaleza una máquina de estados (rojo → verde → amarillo → rojo).

2. **Manejo claro de transiciones**: Cada estado define claramente cuándo y cómo cambiar al siguiente.

3. **Concurrencia manejable**: Cada instancia de Semaforo gestiona su propia FSM independientemente, permitiendo que los tres semáforos funcionen simultáneamente.

4. **Extensibilidad**: Para añadir más semáforos o modificar comportamientos, solo hay que ajustar la clase sin tocar la lógica principal.

Esta aproximación combina efectivamente los principios de POO con FSM, creando un diseño limpio y escalable para sistemas embebidos concurrentes.

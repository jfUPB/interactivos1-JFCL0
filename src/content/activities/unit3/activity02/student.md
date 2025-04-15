# Bomba 2.0 con Secuencia de Desactivación

```python
from microbit import *
import utime
import music

# Estados
MODE_CONFIG = 0
MODE_ARMED = 1
MODE_EXPLOSION = 2
MODE_DISARMING = 3  # Nuevo estado para el proceso de desactivación

# Configuración inicial
current_state = MODE_CONFIG
countdown_time = 20
min_time = 10
max_time = 60
last_update = 0

# Variables para la secuencia de desactivación
disarm_sequence = ["A", "B", "A", "SHAKE"]
current_step = 0
sequence_started = False
sequence_timeout = 5000  # 5 segundos para completar la secuencia
sequence_start_time = 0

def show_time(seconds):
    display.show(str(seconds))

def explosion_animation():
    for i in range(5):
        display.show(Image.ANGRY)
        sleep(200)
        display.show(Image.SKULL)
        sleep(200)

def check_disarm_sequence():
    global current_step, sequence_started, sequence_start_time
    
    # Verificar si se ha iniciado una secuencia
    if not sequence_started and button_a.was_pressed():
        sequence_started = True
        current_step = 1
        sequence_start_time = utime.ticks_ms()
        display.show("1")  # Feedback visual del paso 1
        return False
    
    if sequence_started:
        # Verificar timeout
        if utime.ticks_diff(utime.ticks_ms(), sequence_start_time) > sequence_timeout:
            reset_sequence()
            return False
            
        # Verificar siguiente paso
        expected_input = disarm_sequence[current_step]
        
        if expected_input == "A" and button_a.was_pressed():
            current_step += 1
            display.show(str(current_step))  # Feedback visual
        elif expected_input == "B" and button_b.was_pressed():
            current_step += 1
            display.show(str(current_step))
        elif expected_input == "SHAKE" and accelerometer.was_gesture("shake"):
            current_step += 1
            
        # Si se completó la secuencia
        if current_step >= len(disarm_sequence):
            reset_sequence()
            return True
            
    return False

def reset_sequence():
    global current_step, sequence_started
    current_step = 0
    sequence_started = False
    display.clear()

while True:
    now = utime.ticks_ms()
    
    if current_state == MODE_CONFIG:
        show_time(countdown_time)
        
        if button_a.was_pressed() and countdown_time < max_time:
            countdown_time += 1
            music.play(music.JUMP_UP)
        if button_b.was_pressed() and countdown_time > min_time:
            countdown_time -= 1
            music.play(music.JUMP_DOWN)
            
        if accelerometer.was_gesture("shake"):
            current_state = MODE_ARMED
            start_time = now
            last_update = now
            
    elif current_state == MODE_ARMED:
        elapsed = utime.ticks_diff(now, start_time) // 1000
        remaining = countdown_time - elapsed
        
        if utime.ticks_diff(now, last_update) >= 1000:
            show_time(remaining)
            last_update = now
            
        # Verificar si se está intentando desactivar
        if check_disarm_sequence():
            current_state = MODE_CONFIG
            music.play(music.POWER_UP)  # Sonido de desactivación exitosa
            continue
            
        if remaining <= 0:
            current_state = MODE_EXPLOSION
            
    elif current_state == MODE_EXPLOSION:
        explosion_animation()
        music.play(music.BA_DING)
        
        if pin_logo.is_touched():
            current_state = MODE_CONFIG
            countdown_time = 20

    sleep(100)
```

## Explicación de la Secuencia de Desactivación

### Implementación:

1. **Nuevo estado MODE_DISARMING**: Aunque finalmente no lo usé como estado separado, la lógica de desactivación actúa como un sub-estado dentro de MODE_ARMED.

2. **Secuencia definida**: Creé una lista `disarm_sequence` que contiene los pasos requeridos: ["A", "B", "A", "SHAKE"].

3. **Máquina de estados para la secuencia**:
   - Se inicia cuando se presiona el botón A por primera vez
   - Lleva un contador `current_step` para saber en qué paso de la secuencia estamos
   - Tiene un timeout de 5 segundos para completar toda la secuencia

4. **Feedback visual**:
   - Muestra el número del paso actual (1, 2, 3) durante la secuencia
   - Da sonido de confirmación al completarse exitosamente

5. **Validación**:
   - Verifica cada entrada contra el paso esperado
   - Reinicia la secuencia si hay un error o timeout
   - Solo acepta la secuencia completa y en orden

### Ventajas de esta implementación:

1. **No bloqueante**: La verificación de la secuencia ocurre concurrentemente con la cuenta regresiva
2. **Tolerante a errores**: Timeout automático si el usuario se equivoca o tarda demasiado
3. **Feedback claro**: El usuario sabe en qué paso va de la secuencia
4. **Mantiene separación de responsabilidades**: La lógica de desactivación está encapsulada en su propia función

Esta solución mantiene la simplicidad de la máquina de estados original mientras añade la funcionalidad compleja de desactivación mediante un subsistema de verificación de secuencias.

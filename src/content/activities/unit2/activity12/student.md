## Implementación en MicroPython

```python
from microbit import *
import utime
import music

# Estados
MODE_CONFIG = 0
MODE_ARMED = 1
MODE_EXPLOSION = 2

# Configuración inicial
current_state = MODE_CONFIG
countdown_time = 20  # Valor inicial 20s
min_time = 10
max_time = 60
last_update = 0

def show_time(seconds):
    display.show(str(seconds))

def explosion_animation():
    for i in range(5):
        display.show(Image.ANGRY)
        sleep(200)
        display.show(Image.SKULL)
        sleep(200)

while True:
    now = utime.ticks_ms()
    
    if current_state == MODE_CONFIG:
        # Mostrar tiempo actual
        show_time(countdown_time)
        
        # Ajustar tiempo con botones
        if button_a.was_pressed() and countdown_time < max_time:
            countdown_time += 1
        if button_b.was_pressed() and countdown_time > min_time:
            countdown_time -= 1
            
        # Armar con shake
        if accelerometer.was_gesture("shake"):
            current_state = MODE_ARMED
            start_time = now
            last_update = now
            
    elif current_state == MODE_ARMED:
        # Calcular tiempo restante
        elapsed = utime.ticks_diff(now, start_time) // 1000
        remaining = countdown_time - elapsed
        
        # Actualizar pantalla cada segundo
        if utime.ticks_diff(now, last_update) >= 1000:
            show_time(remaining)
            last_update = now
            
        # Comprobar si ha terminado el tiempo
        if remaining <= 0:
            current_state = MODE_EXPLOSION
            
        # Desarmar con touch
        if pin_logo.is_touched():
            current_state = MODE_CONFIG
            
    elif current_state == MODE_EXPLOSION:
        # Mostrar animación y sonido
        explosion_animation()
        music.play(music.BA_DING)
        
        # Volver a modo configuración con touch
        if pin_logo.is_touched():
            current_state = MODE_CONFIG
            countdown_time = 20  # Resetear a valor inicial

    sleep(100)  # Pequeña pausa para reducir carga de CPU
```

## Detalles de Implementación

1. **Gestión del Tiempo**:
   - Usa `utime.ticks_ms()` para mediciones precisas
   - Convierte milisegundos a segundos para la cuenta regresiva

2. **Entradas**:
   - Botón A (UP): Incremento tiempo
   - Botón B (DOWN): Decremento tiempo
   - Shake (ARMED): Inicia cuenta regresiva
   - Touch (RESET): Vuelve a modo configuración

3. **Salidas**:
   - Pantalla LED: Muestra tiempo o animaciones
   - Speaker: Sonido de explosión

4. **Validaciones**:
   - Límites de tiempo (10-60 segundos)
   - Reset después de explosión

![Video](https://cdn.discordapp.com/attachments/899462124445790261/1361825378636927096/VID_20250415_170551.mp4?ex=68002a37&is=67fed8b7&hm=75cdaa2f7ba0d33e0ecb4eee3186f644da842badb57b184486cc6bfccb57fbbd&)

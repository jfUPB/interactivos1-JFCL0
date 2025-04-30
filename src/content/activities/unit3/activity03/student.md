# Bomba 3.0 con Control Dual (Sensores + Serial)

```python
from microbit import *
import utime
import music

# Estados
MODE_CONFIG = 0
MODE_ARMED = 1
MODE_EXPLOSION = 2
MODE_DISARMING = 3

# Configuración inicial
current_state = MODE_CONFIG
countdown_time = 20
min_time = 10
max_time = 60
last_update = 0

# Variables para manejo de eventos
event_occurred = False
current_event = None
disarm_sequence = ["A", "B", "A", "S"]
current_step = 0
sequence_started = False
sequence_timeout = 5000
sequence_start_time = 0

def show_time(seconds):
    display.show(str(seconds))

def explosion_animation():
    for i in range(5):
        display.show(Image.ANGRY)
        sleep(200)
        display.show(Image.SKULL)
        sleep(200)

def check_disarm_sequence(event):
    global current_step, sequence_started, sequence_start_time
    
    if not sequence_started and event == "A":
        sequence_started = True
        current_step = 1
        sequence_start_time = utime.ticks_ms()
        display.show("1")
        return False
    
    if sequence_started:
        if utime.ticks_diff(utime.ticks_ms(), sequence_start_time) > sequence_timeout:
            reset_sequence()
            return False
            
        expected_input = disarm_sequence[current_step]
        
        if event == expected_input:
            current_step += 1
            if current_step < len(disarm_sequence):
                display.show(str(current_step))
            
        if current_step >= len(disarm_sequence):
            reset_sequence()
            return True
            
    return False

def reset_sequence():
    global current_step, sequence_started
    current_step = 0
    sequence_started = False
    display.clear()

def tareaEventos():
    global event_occurred, current_event
    
    # Leer eventos de hardware
    if button_a.was_pressed():
        current_event = "A"
        event_occurred = True
    elif button_b.was_pressed():
        current_event = "B"
        event_occurred = True
    elif accelerometer.was_gesture("shake"):
        current_event = "S"
        event_occurred = True
    elif pin_logo.is_touched():
        current_event = "T"
        event_occurred = True
    
    # Leer eventos seriales
    if uart.any():
        incoming = uart.read(1).decode('utf-8').upper()
        if incoming in ["A", "B", "S", "T"]:
            current_event = incoming
            event_occurred = True

def tareaBomba():
    global current_state, countdown_time, event_occurred, current_event
    global start_time, last_update, sequence_started, current_step
    
    now = utime.ticks_ms()
    
    # Consumir evento si existe
    event = current_event if event_occurred else None
    if event_occurred:
        event_occurred = False
    
    if current_state == MODE_CONFIG:
        show_time(countdown_time)
        
        if event == "A" and countdown_time < max_time:
            countdown_time += 1
            music.play(music.JUMP_UP)
        elif event == "B" and countdown_time > min_time:
            countdown_time -= 1
            music.play(music.JUMP_DOWN)
        elif event == "S":
            current_state = MODE_ARMED
            start_time = now
            last_update = now
            
    elif current_state == MODE_ARMED:
        elapsed = utime.ticks_diff(now, start_time) // 1000
        remaining = countdown_time - elapsed
        
        if utime.ticks_diff(now, last_update) >= 1000:
            show_time(remaining)
            last_update = now
            
        if event and check_disarm_sequence(event):
            current_state = MODE_CONFIG
            music.play(music.POWER_UP)
            
        if remaining <= 0:
            current_state = MODE_EXPLOSION
            
    elif current_state == MODE_EXPLOSION:
        explosion_animation()
        music.play(music.BA_DING)
        
        if event == "T":
            current_state = MODE_CONFIG
            countdown_time = 20

# Configurar comunicación serial
uart.init(baudrate=9600)

while True:
    tareaEventos()
    tareaBomba()
    sleep(100)
```

## Explicación de la Implementación

### 1. Sistema de Eventos Unificado

He implementado un sistema centralizado de manejo de eventos que:
- Usa dos variables globales:
  - `event_occurred`: Booleano que indica si hay un evento pendiente
  - `current_event`: Almacena el tipo de evento ('A', 'B', 'S' o 'T')

### 2. Separación de Responsabilidades

**tareaEventos()**:
- Detecta eventos tanto de hardware (botones, shake, touch) como seriales
- Normaliza todos los eventos al mismo formato (letras mayúsculas)
- Marca cuando ocurre un evento y almacena su tipo

**tareaBomba()**:
- Contiene toda la lógica de la máquina de estados
- Consume los eventos generados por tareaEventos()
- No interactúa directamente con los periféricos
- Maneja la secuencia de desactivación con los eventos recibidos

### 3. Ventajas del Diseño

1. **Desacoplamiento**: La lógica de la bomba no depende de cómo se generan los eventos
2. **Extensibilidad**: Fácil añadir nuevas fuentes de eventos (ej: comunicación por radio)
3. **Mantenibilidad**: Cambios en el hardware no afectan la lógica principal
4. **Consistencia**: Mismo comportamiento para eventos físicos y seriales

### 4. Flujo de Eventos

1. Cualquier input (físico o serial) se convierte en un evento normalizado
2. tareaBomba() procesa los eventos cuando están disponibles
3. Los eventos se consumen inmediatamente después de procesarse
4. La secuencia de desactivación funciona igual para ambos tipos de input

### 5. Uso del Puerto Serial

Para probar con la aplicación web sugerida:
1. Conecta el micro:bit por USB
2. Abre la aplicación serial terminal
3. Selecciona el puerto del micro:bit
4. Envía los caracteres: A, B, S o T (mayúsculas o minúsculas)

Este diseño cumple con todos los requisitos manteniendo un código limpio y modular, permitiendo fácil expansión para futuras características.

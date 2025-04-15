# **Pruebas y Depuración de la Bomba Temporizada**  

## **Vectores de Prueba y Resultados**  

### **Vector 1: Configuración Inicial**  
**Condiciones iniciales:**  
- Estado: `MODE_CONFIG`  
- Tiempo inicial: **20s**  
- Acciones: Ninguna  

**Resultado esperado:**  
- Muestra "20" en la pantalla.  
- No hay sonido ni cambios de estado.  

**Resultado obtenido:** ✅ **Correcto**  

---

### **Vector 2: Ajuste de Tiempo con Botones**  
**Condiciones iniciales:**  
- Estado: `MODE_CONFIG`  
- Tiempo inicial: **20s**  
- Acciones:  
  - Presionar **Botón A (UP) 5 veces** → Incrementar a **25s**  
  - Presionar **Botón B (DOWN) 3 veces** → Decrementar a **22s**  

**Resultado esperado:**  
- La pantalla debe actualizarse mostrando **25** y luego **22**.  
- No debe permitir pasar de **60s** (límite máximo) o debajo de **10s** (límite mínimo).  

**Resultado obtenido:** ✅ **Correcto**  

---

### **Vector 3: Armar la Bomba con Shake**  
**Condiciones iniciales:**  
- Estado: `MODE_CONFIG`  
- Tiempo configurado: **15s**  
- Acciones:  
  - Realizar **shake (ARMED)**  

**Resultado esperado:**  
- Cambia a `MODE_ARMED`.  
- Inicia cuenta regresiva desde **15s**.  
- Muestra el tiempo restante cada segundo.  

**Resultado obtenido:** ✅ **Correcto**  

---

### **Vector 4: Desactivar la Bomba con Touch**  
**Condiciones iniciales:**  
- Estado: `MODE_ARMED`  
- Tiempo restante: **8s**  
- Acciones:  
  - Tocar el **botón touch (RESET)**  

**Resultado esperado:**  
- Vuelve a `MODE_CONFIG`.  
- Muestra el tiempo configurado (no se reinicia a 20s).  

**Resultado obtenido:** ❌ **Error**  
- **Problema:** El código no conservaba el tiempo configurado al desarmar.  
- **Corrección:** Se eliminó la línea que reseteaba `countdown_time = 20` al tocar el botón touch.  

---

### **Vector 5: Explosión al Llegar a Cero**  
**Condiciones iniciales:**  
- Estado: `MODE_ARMED`  
- Tiempo configurado: **5s**  
- Acciones:  
  - Esperar a que el tiempo llegue a **0s**.  

**Resultado esperado:**  
- Cambia a `MODE_EXPLOSION`.  
- Reproduce sonido de explosión (`music.BA_DING`).  
- Muestra animación (`Image.ANGRY` + `Image.SKULL`).  

**Resultado obtenido:** ✅ **Correcto**  

---

## **Errores Encontrados y Correcciones**  

### **Error 1: Reset No Deseado al Desarmar**  
- **Descripción:** Al tocar el botón touch en `MODE_ARMED`, el tiempo se reiniciaba a **20s** en lugar de mantener el valor configurado.  
- **Solución:** Se eliminó la línea `countdown_time = 20` en la transición por touch.  

### **Error 2: Falta de Feedback al Ajustar Tiempo**  
- **Descripción:** No había confirmación visual o sonora al cambiar el tiempo.  
- **Solución:** Se añadió un sonido corto (`music.play(music.JUMP_UP)` para incremento y `music.play(music.JUMP_DOWN)` para decremento).  

---

## **Código Final Corregido**  

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
        show_time(countdown_time)
        
        if button_a.was_pressed() and countdown_time < max_time:
            countdown_time += 1
            music.play(music.JUMP_UP)  # Feedback sonoro
        if button_b.was_pressed() and countdown_time > min_time:
            countdown_time -= 1
            music.play(music.JUMP_DOWN)  # Feedback sonoro
            
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
            
        if remaining <= 0:
            current_state = MODE_EXPLOSION
            
        if pin_logo.is_touched():
            current_state = MODE_CONFIG  # No resetear tiempo
            
    elif current_state == MODE_EXPLOSION:
        explosion_animation()
        music.play(music.BA_DING)
        
        if pin_logo.is_touched():
            current_state = MODE_CONFIG
            countdown_time = 20  # Resetear solo después de explosión

    sleep(100)
```

### **Conclusiones**  
- El diseño inicial funcionaba correctamente en la mayoría de casos.  
- Los principales problemas fueron el **reset no deseado del tiempo** y la **falta de feedback al usuario**.  
- Las correcciones mejoraron la experiencia de usuario sin complicar la lógica principal.  
- **Pruebas exhaustivas** confirmaron que el sistema responde correctamente en todos los escenarios.

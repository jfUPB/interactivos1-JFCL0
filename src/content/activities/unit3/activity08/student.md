```py
from microbit import *
import utime

# Estado inicial
estado = "configuracion"
tiempo = 20

while True:
    if estado == "configuracion":
        display.show(str(tiempo % 10))  # Muestra solo el último dígito del tiempo
        
        if button_a.was_pressed() and tiempo < 60:
            tiempo += 1
            uart.write("UP\n")
        elif button_b.was_pressed() and tiempo > 10:
            tiempo -= 1
            uart.write("DOWN\n")
        elif accelerometer.was_gesture("shake"):
            estado = "armado"
            uart.write("ARMED\n")
    
    elif estado == "armado":
        display.show(str(tiempo % 10))
        utime.sleep(1)
        if tiempo > 0:
            tiempo -= 1
            uart.write("TICK\n")
        if tiempo == 0:
            estado = "explosion"
            uart.write("EXPLODE\n")

    elif estado == "explosion":
        display.show(Image.SKULL)
        uart.write("EXPLODE\n")
        if pin0.is_touched():
            estado = "configuracion"
            tiempo = 20
            uart.write("RESET\n")
```

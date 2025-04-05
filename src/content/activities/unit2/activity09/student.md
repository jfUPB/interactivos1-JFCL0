#### Codigo del micro:bit 

```py
def mostrar_verde():
    basic.show_leds("""
        . # # # .
        # . . . #
        # . . . #
        # . . . #
        . # # # .
        """)
def mostrar_amarillo():
    basic.show_leds("""
    . . . . .
    . . . . .
    # # # # #
    . . . . .
    . . . . .
    """)
def mostrar_rojo():
    basic.show_leds("""
    # . . . #
    . # . # .
    . . # . .
    . # . # .
    # . . . #
    """)
```

#### Codigo en MicroPython

```py
from microbit import *
import utime

class Semaforo:
    def __init__(self,initState,interval,prevState):
        self.State = "Verde"
        self.prevState = ""
        self.startTime = 0
        self.interval = interval
    
    def update(self):
        
        if self.State == "Verde":
            self.startTime = utime.ticks.ms()
            self.State = "Amarillo"
            self.prevState = "Verde"
            mostrar_verde()
        elif self.State == "Amarillo":
            mostrar_amarillo()
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > self.interval:
                self.startTime = utime.ticks_ms()
                if prevState == "Verde":
                    self.State = "Rojo"
                    
                else:
                    self.State = "Verde"
                
        elif self.State == "Rojo":
            self.State = "Amarillo"
            self.prevState = "Rojo"
            mostrar_rojo()
 
Semaforo1 = (1000)


while true:
    Semaforo1.update()
```

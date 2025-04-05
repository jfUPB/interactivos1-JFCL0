```py
from microbit import *
import utime
from PIL import Image, ImageDraw

class semaforo:
    def __init__(self,initState,interval):
        self.state = "Verde"
        self.startTime = 0
        self.interval = interval
        self.SemState = initState
    
    def update(self):
        
        if self.state == "Verde:
            self.startTime = utime.ticks_ms()
            self.state = "Amarillo"
```

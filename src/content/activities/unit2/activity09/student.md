#### Codigo

```py
from microbit import *
import utime

class TrafficLight:
    def __init__(self):
        self.state = "RED"
        self.startTime = utime.ticks_ms()
        self.RED_TIME = 5000      
        self.GREEN_TIME = 5000     
        self.YELLOW_TIME = 2000   
        
    def update(self):
        currentTime = utime.ticks_ms()
        elapsed = utime.ticks_diff(currentTime, self.startTime)
        
        if self.state == "RED":
            
            display.clear()
            display.set_pixel(2, 2, 9)
            
           
            if elapsed > self.RED_TIME:
                self.state = "GREEN"
                self.startTime = currentTime
                
        elif self.state == "GREEN":
            
            display.clear()
            display.set_pixel(2, 4, 9)
            display.set_pixel(1, 4, 9)
            display.set_pixel(3, 4, 9)
            
           
            if elapsed > self.GREEN_TIME:
                self.state = "YELLOW"
                self.startTime = currentTime
                
        elif self.state == "YELLOW":
           
            display.clear()
            display.set_pixel(0, 2, 9)
            display.set_pixel(1, 2, 9)
            display.set_pixel(2, 2, 9)
            display.set_pixel(3, 2, 9)
            display.set_pixel(4, 2, 9)
            
           
            if elapsed > self.YELLOW_TIME:
                self.state = "RED"
                self.startTime = currentTime


semaphore = TrafficLight()

while True:
    semaphore.update()
```

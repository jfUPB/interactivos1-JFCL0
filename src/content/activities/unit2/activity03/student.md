#### Micro:bit 

Sistemas de entrada: Boton A, Boton B, Compass, Acelerometro.  
Sistemas de salida: Luces LED, Pines de accesorios, indicador de microfono y parlante.  

#### Funciones de MycroPython

Entradas:

- `botton.a.was.pressed` es una funcion que permite leer si el boton A esta siendo presionado, se puede utilizar de la siguiente manera:

  ```py
  while True:
    if (button.a.was.pressed):
      display.scroll(A)
  ```

- `botton.b.was.pressed` es una funcion que permite leer si el boton B esta siendo presionado, se puede utilizar de la siguiente manera:

  ```py
  while True:
    if (button.b.was.pressed):
      display.scroll(B)
  ```
- `display.read_light_level()` es una funcion que lee la cantidad de luz cayendo sobre el dispositivo, utilizando las luces LED que tiene en el display, se puede utilizar de la siguiente manera:

  ```py
  display.scroll(display.read_light_level())
  ```
Salidas:   

- `music.play` permite al micro:bit poner alguna de las canciones integradas en el dispositivo, se utiliza de la siguiente manera:

  ```py
  import music

  music.play(music.BA_DING)
  ```

- `music.pitch` permite al micro:bit soltar un sonido desde una frecuencia dada, se utiliza de la siguiente manera:

  ```py
  import music

  music.pitch(400)
  ```

- `display.show()` sirve para mostrar una de las imagenes programadas en el interior del micro:bit, se utiliza de la siguiente manera:

  ```py
  display.show(heart) //muestra un corazon desde los LEDS
  sleep(400)
  ```

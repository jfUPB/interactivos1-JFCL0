#### Experimento de testeo

Quiero comprobar que el microbit esta funcionando como deberia. Para comprobar que el boton A, B o el logo estan siendo presionados si y solo si el usuario los esta presionando, escribi el siguiente codigo:

```py
while True:
    sleep(1000)
    if button_a.is_pressed:
        display.show(Image.ANGRY)
        sleep(400)
    if button_b.is_pressed:
        display.show(Image.HEART)
        sleep(400)
    if pin_logo.is_touched():
        display.show(Image.SKULL)
        sleep(400)
```
Los resultados de este experimento fueron poco satisfactorios, pues el micro:bit sigue mostrando la cara de enojado incluso si nunca se ha tocado ninguno de los botones. El unico que responde de forma satisfactoria es el logo, pues si responde de una forma que se considera adecuada.

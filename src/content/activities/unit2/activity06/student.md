```js
input.onButtonPressed(Button.A, function() // detecta si A fue presionado
{
      basic.showIcon(IconNames.Angry) // dibuja el icono angry en los leds del micro:bit
      basic.clearScreen() // limpia la pantalla
})
input.onButtonPressed(Button.B, function() // detecta si B fue presionado
{
      basic.showIcon(IconNames.Sad) // dibuja el icono sad en los leds del micro:bit
      basic.clearScreen() // limpia la pantalla
})
input.onLogoEvent(TouchButtonEvent.Pressed, function() // detecta si el logo fue presionado
{
    basic.showIcon(IconNames.Happy) // dibuja el icono happy en los leds del micro:bit
    basic.clearScreen() // limpia la pantalla
})
```

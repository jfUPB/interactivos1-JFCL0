#### Microbit

- Describe qué pasa en el punto 15 y cómo crees que esto se logre.
R/ Desde el cable se le indica al computador que un boton esta siendo presionado. Cuando el codigo detecta esto, desde un condicional, hace que el circulo en pantalla sea rojo o amarillo.
```js
function draw() {

    if(port.availableBytes() > 0){
        let dataRx = port.read(1);
        if(dataRx == 'A'){
            fill('red');   
        }
        else if(dataRx == 'B'){
            fill('yellow'); 
```
- Describe qué pasa en el punto 16 y cómo crees que esto se logre.
R/ Desde un sensor (llevo buscando el nombre 15 minutos, me rindo) se detecta el movimiento, mandando una señal de este mediante el cable. Cuando este es analizado, se le indica mediante al codigo al pc hacer que el circulo sea verde.
```js
if(port.availableBytes() > 0){
        let dataRx = port.read(1);
        if(dataRx == 'A'){
            fill('red');   
        }
        else if(dataRx == 'B'){
            fill('yellow'); 
        }
        // Aqui
        else{
            fill('green'); 
        }
```
- Describe qué pasa en el punto 17 y cómo crees que esto se logre.
R/ Al presionar un boton en pantalla se le indica al computador que envie una señal al dispositivo de salida, siendo el simbolo del corazon y la carita feliz.
```js
function sendBtnClick() {
    port.write('h');
```

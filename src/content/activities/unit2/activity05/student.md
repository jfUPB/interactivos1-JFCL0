Setup del micro:bit

```js
input.onButtonPressed(Button.A, function () 
{
    
    basic.showString(texto)
})
```

Programa en p5.js

```js
function setup() 
{
  createCanvas(400, 400);
  myInput = createInput();
  myInput = position(0,200);
}

function draw() 
{
  background(220);
  
  let texto = myInput.value();
}
```

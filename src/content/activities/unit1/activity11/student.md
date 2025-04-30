```js
let port;
let connectBtn;

function setup() {
    createCanvas(400, 400);
    background(220);
    circlex = height / 2;
    circley = width / 2;
    port = createSerial();
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);
    ellipse(width / 2, height / 2, 100,100);
}

function draw() {

    if(port.availableBytes() > 0){
        let dataRx = port.read(1);
        if(dataRx == 'A'){
            circlex -= 3;   
        }
        if(dataRx == 'B'){
            circlex += 3; 
        }
        background(220);
        ellipse(circlex, circley, 100, 100);
        fill(0,0,255);
    }    


    if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    } 
    else {
        connectBtn.html('Disconnect');
    }
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);
    } else {
        port.close();
    }

}
```

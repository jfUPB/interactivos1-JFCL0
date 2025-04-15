```js
// Variables globales
let serial;
let portSelector;
let connectBtn;
let refreshBtn;
let statusDiv;
let timeDisplay;
let consoleDiv;
let btnA, btnB, btnArm, btnReset, btnDisarm;

function setup() {
  noCanvas();
  
  // Inicializar elementos del DOM
  portSelector = select('#port-selector');
  connectBtn = select('#connect-btn');
  refreshBtn = select('#refresh-ports');
  statusDiv = select('#status');
  timeDisplay = select('#time-display');
  consoleDiv = select('#console');
  btnA = select('#btn-a');
  btnB = select('#btn-b');
  btnArm = select('#btn-arm');
  btnReset = select('#btn-reset');
  btnDisarm = select('#btn-disarm');
  
  // Inicializar comunicación serial
  serial = new p5.SerialPort();
  
  // Configurar manejadores de eventos
  refreshBtn.mousePressed(refreshPorts);
  connectBtn.mousePressed(toggleConnection);
  
  btnA.mousePressed(() => sendCommand('A'));
  btnB.mousePressed(() => sendCommand('B'));
  btnArm.mousePressed(() => sendCommand('S'));
  btnReset.mousePressed(() => sendCommand('T'));
  btnDisarm.mousePressed(() => {
    sendCommand('A');
    setTimeout(() => sendCommand('B'), 300);
    setTimeout(() => sendCommand('A'), 600);
    setTimeout(() => sendCommand('S'), 900);
    logMessage('Enviando secuencia de desactivación: A-B-A-S');
  });
  
  // Configurar manejadores de eventos seriales
  serial.on('list', gotPortList);
  serial.on('data', serialDataReceived);
  serial.on('open', portOpened);
  serial.on('close', portClosed);
  serial.on('error', serialError);
  
  // Actualizar lista de puertos al iniciar
  refreshPorts();
}

function refreshPorts() {
  serial.list().then(ports => {
    portSelector.option('Seleccione un puerto', '');
    ports.forEach(port => {
      portSelector.option(port, port);
    });
  });
}

function gotPortList(ports) {
  portSelector.option('Seleccione un puerto', '');
  ports.forEach(port => {
    portSelector.option(port, port);
  });
}

function toggleConnection() {
  if (!serial.serialport || !serial.serialport.isOpen) {
    const port = portSelector.value();
    if (port && port !== 'Seleccione un puerto') {
      serial.open(port, {
        baudRate: 9600,
        dataBits: 8,
        parity: 'none',
        stopBits: 1,
        flowControl: 'none'
      });
    } else {
      logMessage('Error: Seleccione un puerto válido');
    }
  } else {
    serial.close();
  }
}

function portOpened() {
  logMessage('Conexión establecida con Micro:bit');
  statusDiv.html('Conectado');
  statusDiv.removeClass();
  statusDiv.addClass('connected');
  connectBtn.html('Desconectar');
  enableControls(true);
}

function portClosed() {
  logMessage('Conexión cerrada');
  statusDiv.html('Desconectado');
  statusDiv.removeClass();
  statusDiv.addClass('disconnected');
  connectBtn.html('Conectar Micro:bit');
  timeDisplay.html('--');
  enableControls(false);
}

function serialError(err) {
  logMessage('Error de conexión: ' + err);
  statusDiv.html('Error de conexión');
  statusDiv.removeClass();
  statusDiv.addClass('disconnected');
}

function serialDataReceived() {
  let data = serial.readStringUntil('\n');
  if (data) {
    data = data.trim();
    logMessage('Micro:bit → ' + data);
    
    if (data.startsWith('TIME:')) {
      const time = data.split(':')[1];
      timeDisplay.html(time + ' segundos');
    } else if (data === 'ARMED') {
      statusDiv.html('Bomba Activada!');
      statusDiv.removeClass();
      statusDiv.addClass('armed');
      logMessage('¡Bomba activada! Iniciando cuenta regresiva');
    } else if (data === 'DISARMED') {
      statusDiv.html('Bomba Desactivada');
      statusDiv.removeClass();
      statusDiv.addClass('connected');
      logMessage('¡Bomba desactivada con éxito!');
    } else if (data === 'EXPLODED') {
      statusDiv.html('¡EXPLOSIÓN!');
      statusDiv.removeClass();
      statusDiv.addClass('exploded');
      logMessage('¡BOOM! La bomba ha explotado');
    } else if (data === 'RESET') {
      statusDiv.html('Listo');
      statusDiv.removeClass();
      statusDiv.addClass('connected');
      logMessage('Sistema reiniciado, listo para nueva configuración');
    }
  }
}

function sendCommand(cmd) {
  if (serial.serialport && serial.serialport.isOpen) {
    serial.write(cmd);
    logMessage('Enviado comando: ' + cmd);
  } else {
    logMessage('Error: No conectado al Micro:bit');
  }
}

function enableControls(enabled) {
  btnA.attribute('disabled', !enabled);
  btnB.attribute('disabled', !enabled);
  btnArm.attribute('disabled', !enabled);
  btnReset.attribute('disabled', !enabled);
  btnDisarm.attribute('disabled', !enabled);
}

function logMessage(message) {
  const now = new Date();
  const timestamp = now.toLocaleTimeString();
  consoleDiv.html('[' + timestamp + '] ' + message + '<br>' + consoleDiv.html());
}
```

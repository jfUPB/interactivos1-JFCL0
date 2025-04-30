# Solución

## 1. Comparación inicial del código HTML

Código original vs modificado:
- Se añadió la librería p5.webserial.js
- Permite comunicación serial con el micro:bit via WebSerial API

## 2. Análisis de imágenes

Archivos cargados:
- 02.svg, 03.svg, 04.svg, 05.svg
Función:
- Módulos gráficos para dibujo en canvas
- Se seleccionan con teclas 6-9 (1-4 para colores)

## 3. Máquina de estados

Estados implementados:
1. WAIT_MICROBIT_CONNECTION
   - Espera conexión del dispositivo
   - Muestra botón "Connect to micro:bit"
2. RUNNING
   - Estado activo de dibujo
   - Reacciona a datos del micro:bit

## 4. Comunicación Serial

Configuración:
- Velocidad: 115200 baudios
- Formato datos: "x,y,aState,bState\n"
Flujo de conexión:
1. Usuario hace click en botón
2. Se abre puerto serial con port.open()
3. Cambia texto a "Disconnect"
4. Comienza recepción de datos

## 5. Protocolo de Comunicación

Estructura de datos:
- Valores separados por comas
- Terminación con \n
Ejemplo: "1024,512,true,false\n"
Procesamiento en p5.js:
1. port.readUntil("\n") para paquete completo
2. data.trim() elimina \n
3. split(",") divide valores
4. Validación de 4 valores

## 6. Experimentos Realizados

a) Centro de coordenadas:
- Sin ajuste: (0,0) = esquina superior izq.
- Con windowWidth/2: centrado horizontal
- Con windowHeight/2: centrado vertical

b) Estados de botones:
- Botón A (presionado): 
  * Genera nuevo tamaño (50-160px)
  * Guarda posición click (clickPosX/Y)
- Botón B (liberado):
  * Cambia color aleatorio
  * Alpha entre 80-100

c) Mensaje de error:
"No se están recibiendo 4 datos del micro:bit"
- Causas:
  * Paquete incompleto
  * Corrupción de datos
  * Desincronización temporal

## 7. Diferencias con Código Original

Eliminados/modificados:
- Eventos de mouse (click, drag)
- Función de barra espaciadora
- Algunos controles de teclado

Mantenidos:
- Teclas 1-9 para colores/formas
- Flechas para tamaño/velocidad
- Tecla S para guardar canvas
- Delete/Bksp para limpiar

## 8. Hallazgos Clave

- La comunicación serial es asíncrona
- Importancia de validar datos recibidos
- Necesidad de almacenar estados previos
- Máquina de estados mejora robustez
- Transformación de coordenadas esencial

## 9. Mejoras Sugeridas

1. Buffer para datos incompletos
2. Indicador visual de conexión
3. Más controles desde micro:bit
4. Historial de acciones (undo)
5. Mejor manejo de errores

## 10. Instrucciones de Uso

1. Conectar micro:bit (botón Connect)
2. Mover micro:bit para dibujar
3. Botón A: cambiar tamaño
4. Botón B: cambiar color
5. Teclas:
   - 1-4: Colores predefinidos
   - 6-9: Formas SVG
   - Flechas: Ajustes
   - S: Guardar
   - Delete: Limpiar

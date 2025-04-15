# Diseño de Máquina de Estados para Bomba Temporizada

## Diagrama de Estados

```
[INICIO]
  |
  v
+---------------------+   shake (ARMED)   +---------------------+
| MODO_CONFIGURACION  |------------------>|   BOMBA_ARMADA      |
|                     |                   |                     |
| - Muestra tiempo    |                   | - Cuenta regresiva  |
| - Botones UP/DOWN   |<------------------| - Muestra tiempo    |
|   ajustan tiempo    |    touch (RESET)  | - Al llegar a 0:    |
+---------------------+                   |   -> EXPLOSION      |
        ^                                 +---------------------+
        |                                         |
        |                                         | Tiempo = 0
        |                                         v
        |                                 +---------------------+
        +---------------------------------|     EXPLOSION       |
                                          |                     |
                                          | - Sonido explosión  |
                                          | - Muestra animación |
                                          +---------------------+
```

## Descripción Textual de la Máquina de Estados

### Estados Principales:

1. **MODO_CONFIGURACION**:
   - Estado inicial
   - Muestra el tiempo actual en la pantalla LED (ej. "20")
   - Permite ajustar el tiempo con botones:
     - **UP (Botón A)**: Incrementa tiempo (+1s, máximo 60s)
     - **DOWN (Botón B)**: Decrementa tiempo (-1s, mínimo 10s)
   - Transición:
     - **shake (ARMED)**: Pasa a estado BOMBA_ARMADA

2. **BOMBA_ARMADA**:
   - Inicia cuenta regresiva desde el tiempo configurado
   - Muestra el tiempo restante actualizado en pantalla
   - Transiciones:
     - **touch (RESET)**: Vuelve a MODO_CONFIGURACION (desarma)
     - **Tiempo = 0**: Pasa a estado EXPLOSION

3. **EXPLOSION**:
   - Estado final
   - Reproduce sonido de explosión (speaker)
   - Muestra animación en pantalla LED
   - Transición:
     - **touch (RESET)**: Vuelve a MODO_CONFIGURACION


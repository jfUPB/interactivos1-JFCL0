Aquí está el modelo gráfico de la bomba como diagrama de estados, siguiendo el estilo de la actividad 10:

## Diagrama de Estados de la Bomba (Versión Textual)

```
[INICIO]
  │
  ▼
+---------------------+   shake (S)    +---------------------+
|  MODO_CONFIGURACIÓN |--------------->|    BOMBA_ARMADA     |
|                     |                |                     |
| - Muestra tiempo    |                | - Cuenta regresiva  |
| - Ajusta con A/B    |<---+   +-------| - Muestra tiempo    |
+---------------------+    |   |       +---------------------+
  ▲   |  T (reset)         |   | Tiempo = 0        ▲
  |   |                    |   |                   |
  |   |  Secuencia A-B-A-S |   |                   |
  |   +-----------------------+                   |
  |                                               |
  |         +---------------------+               |
  +---------|     EXPLOSIÓN       |<--------------+
            |                     |
            | - Animación/Sonido  |
            | - Solo reset con T  |
            +---------------------+
```

## Explicación del Modelo

1. **Estados Principales**:
   - **MODO_CONFIGURACIÓN**: Estado inicial donde se ajusta el tiempo (10-60s)
   - **BOMBA_ARMADA**: Cuenta regresiva activa, acepta desactivación
   - **EXPLOSIÓN**: Estado final con animación

2. **Transiciones**:
   - `shake (S)`: Arma la bomba (config → armada)
   - `T (touch)`: Reinicia en cualquier estado
   - `Secuencia A-B-A-S`: Desarma la bomba (armada → config)
   - `Tiempo = 0`: Detona la bomba (armada → explosión)

3. **Eventos Concurrentes**:
   - Temporizador: Controla la cuenta regresiva
   - Entradas: Botones (A/B/T) y shake (S)
   - Serial: Comandos equivalentes a las entradas físicas

## Diagrama Visual (Descripción para draw.io)

1. **Elementos**:
   - Rectángulos redondeados para estados
   - Flechas con etiquetas para transiciones
   - Notas para acciones en cada estado

2. **Configuración en draw.io**:
   ```
   [Inicio] → [Modo Configuración]
     - On Entry: Mostrar tiempo
     - On A: +1s (max 60)
     - On B: -1s (min 10)
     - On S: → Bomba Armada

   [Modo Configuración] ←→ [Bomba Armada]
     - Transición S (shake)
     - Transición T (reset)
     - Transición secuencia A-B-A-S

   [Bomba Armada] → [Explosión]
     - Condición: tiempo = 0
     - Acción: animación/sonido

   [Explosión] → [Modo Configuración]
     - Condición: T (touch)
     - Acción: reset a 20s
   ```

## Diferencias con la Actividad 10

1. **Complejidad aumentada**:
   - Secuencia de desactivación (4 pasos)
   - Dos fuentes de entrada (física/serial)
   - Temporizador con ajuste dinámico

2. **Mecanismo de concurrencia**:
   - Sistema de eventos unificado
   - Priorización: secuencia > temporizador
   - Estados mutuamente excluyentes

3. **Pruebas**:
   - Vectores deben incluir ahora:
     1. Comandos seriales vs físicos
     2. Secuencia de desactivación
     3. Timeout de la secuencia

Este modelo captura toda la lógica de la bomba 3.0, mostrando claramente cómo maneja eventos concurrentes de múltiples fuentes mientras mantiene un flujo de estados controlado.

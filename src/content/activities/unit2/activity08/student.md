- Describe detalladamente cómo funciona este ejemplo. Usa chatGPT para indagar y profundizar en todas las dudas que puedas tener.  

    R/ El programa, primero, crea parametros para una clase pixel, los cuales utilizará mas tarde. Tras esto, mediante update(), crea un loop de 2 estados, desde el cual alterna los pixeles cada
     una determinada cantidad de tiempo, establecida al momento de crear el pixel. Tras esto, crea 2 pixeles, uno en la ubicacion 0,0, que cambia de estado cada 1 segundo, y un segundo pixel en la ubicacion 4,4, que cambia de estadi
     cada 0,5s.
  
- Del contexto del programa ¿Puedes identificar algún estado? Los estados son momentos del programa en los cuales este espera a que ocurra algo.  

   R/ El programa tiene 2 estados, Init y WaitTimeOut. En Init, se mantiene el efecto sobre el pixel que dejó WaitTimeOut, poniendo a correr el temporizador determinado al momento de crear el pixel. Tras esto, en WaitTimeOut, el
     programa cambia el estado del pixel, sea de encendido a apagado o viceversa.
  
- Del contexto del programa ¿Puedes identificar algún evento? Los eventos son aquellas cosas por las que el programa pregunta durante un estado.  

    R/ Este programa cambia constantemente entre estados sin necesitar un incentivo externo, es decir, no requiere de que se presione un botón o nada por el estilo, autoalimentandose puramente mediante el conteo del tiempo, haciendo que
    los eventos dentro de este mismo sean unicamente los cambios de estado, de Init a WaitTimeOut y viceversa.
  
- Del contexto del programa ¿Puedes identificar alguna acción? Las acciones son aquellas cosas que el programa ejecuta en respuesta a la ocurrencia de un evento.  

    R/ El programa ejecuta 2 acciones, encender y apagar la luz led del pixel que está actualizando.

# Nombre quipo:
Jujut
## Integrantes:
- David Galvis García: 000494860
- Maria Paulina Serna: 000448608
- Repositorio personal: https://github.com/paulinasernaal/Raspberry-pico.git
### Actividad 3:
- Coloca en el archivo el resultado del cambio de 100 a 500. Describe lo que viste:
  El cambio del LED de prendido a apagado, se daba mas lentamente al pasarlo a 500, pero cuando estaba en 100 era mas rapido.
### Actividad 6:
- ¿Cómo se ejecuta este programa?
  
  Lo que está haciendo el programa es definir primero unas constantes, y despues de que pasa cada segundo imprimi en el monitor serial el sesgundo en el que se encuentra.
- Pudiste ver este mensaje: `Serial.print("Task1States::WAIT_TIMEOUT\n");`. ¿Por qué crees que ocurre esto?

  Verificación del Cambio de Estado: Confirma que el código en el estado INIT se ha ejecutado completamente y que el estado de la máquina se ha actualizado correctamente a WAIT_TIMEOUT. Por que no nos aparece? 
- ¿Cuántas veces se ejecuta el código en el case Task1States::INIT?

  Se ejecuta una sola vez al inicio
### Actividad 7: 
  La función millis retorna el numero de segundos que han pasado desde que el arduino empezó a ejecutar el programa.
### Actividad 8:
```
  #include <Arduino.h>

// Definición de estados y variable de estado
enum class TaskStates {
    INIT,
    WAIT_1_SECOND,
    WAIT_2_SECONDS,
    WAIT_3_SECONDS
};
static TaskStates taskState = TaskStates::INIT;

// Definición de variables static (conservan su valor entre llamadas a taskTask)
static uint32_t lastTime = 0;

// Constantes
constexpr uint32_t ONE_SECOND = 1000;
constexpr uint32_t TWO_SECONDS = 2000;
constexpr uint32_t THREE_SECONDS = 3000;

void task() {
    uint32_t currentTime = millis();
    
    switch (taskState) {
        case TaskStates::INIT: {
            // Acciones:
            Serial.begin(115200);

            // Garantiza los valores iniciales para el siguiente estado.
            lastTime = currentTime;
            taskState = TaskStates::WAIT_1_SECOND;


            break;
        }
        case TaskStates::WAIT_1_SECOND: {
            // Evento
            if ((currentTime - lastTime) >= ONE_SECOND) {
                // Acciones:
                lastTime = currentTime;
                Serial.print("1 segundo\n");
                taskState = TaskStates::WAIT_2_SECONDS;
            }
            break;
        }
        case TaskStates::WAIT_2_SECONDS: {
            // Evento
            if ((currentTime - lastTime) >= ONE_SECOND) {
                // Acciones:
                lastTime = currentTime;
                Serial.print("2 segundos\n");
                taskState = TaskStates::WAIT_3_SECONDS;
            }
            break;
        }
        case TaskStates::WAIT_3_SECONDS: {
            // Evento
            if ((currentTime - lastTime) >= ONE_SECOND) {
                // Acciones:
                lastTime = currentTime;
                Serial.print("3 segundos\n");
                taskState = TaskStates::WAIT_1_SECOND; // Reinicia el ciclo
                
            }
            break;
        }
        default: {
            Serial.println("Error");
        }
    }
}

void setup() {
    task();
}

void loop() {
    task();
}
```
- ¿Cuáles son los estados del programa?

  - INIT
  - WAIT1SECOND
  - WAIT2SECOND
  - WAIT3SECOND
- ¿Cuáles son los eventos?
  
   if ((currentTime - lastTime) >= ONE_SECOND), Los tres if son los mismos y esos son los eventos
- ¿Cuáles son las acciones?
  lastTime = currentTime;
                Serial.print("1 segundo\n");
                taskState = TaskStates::WAIT_2_SECONDS;
  lastTime = currentTime;
                Serial.print("2 segundos\n");
                taskState = TaskStates::WAIT_3_SECONDS;
  lastTime = currentTime;
                Serial.print("3 segundos\n");
                taskState = TaskStates::WAIT_1_SECOND;
### Actividad 9:
```
  void task1(){
    enum class Task1States{
        INIT,
        WAIT_FOR_TIMEOUT
    };

    static Task1States task1State = Task1States::INIT;
    static uint32_t lastTime;
    static constexpr uint32_t INTERVAL = 1000;

    switch(task1State){
        case Task1States::INIT:{
            Serial.begin(115200);
            lastTime = millis();
            task1State = Task1States::WAIT_FOR_TIMEOUT;
            break;
        }

        case Task1States::WAIT_FOR_TIMEOUT:{
            // evento 1:            
            uint32_t currentTime = millis();
            if( (currentTime - lastTime) >= INTERVAL ){
                lastTime = currentTime;
                Serial.print("mensaje a 1Hz\n");
            }
            break;
        }

        default:{
            break;
        }
    }

}
void task2(){
    enum class Task2States{
        INIT,
        WAIT_FOR_TIMEOUT
    };

    static Task2States task2State = Task2States::INIT;
    static uint32_t lastTime;
    static constexpr uint32_t INTERVAL = 2000;

    switch(task2State){
        case Task2States::INIT:{
            Serial.begin(115200);
            lastTime = millis();
            task2State = Task2States::WAIT_FOR_TIMEOUT;
            break;
        }

        case Task2States::WAIT_FOR_TIMEOUT:{
            // evento 1:            
            uint32_t currentTime = millis();
            if( (currentTime - lastTime) >= INTERVAL ){
                lastTime = currentTime;
                Serial.print("mensaje a 0.5Hz\n");
            }
            break;
        }

        default:{
            break;
        }
    }

}
void task3(){
    enum class Task3States{
        INIT,
        WAIT_FOR_TIMEOUT
    };

    static Task3States task3State = Task3States::INIT;
    static uint32_t lastTime;
    static constexpr uint32_t INTERVAL = 4000;

    switch(task3State){
        case Task3States::INIT:{
            Serial.begin(115200);
            lastTime = millis();
            task3State = Task3States::WAIT_FOR_TIMEOUT;
            break;
        }

        case Task3States::WAIT_FOR_TIMEOUT:{
            // evento 1:            
            uint32_t currentTime = millis();
            if( (currentTime - lastTime) >= INTERVAL ){
                lastTime = currentTime;
                Serial.print("mensaje a 0.25Hz\n");
            }
            break;
        }

        default:{
            break;
        }
    }

}

void setup()
{
    task1();
    task2();
    task3();
}

void loop()
{
    task1();
    task2();
    task3();
}
  
```
### Actividad 11:
1. Analiza el programa. ¿Por qué enviaste la letra con el botón send? ¿Qué evento verifica si ha llegado algo por el puerto serial?
   Enviamos la letra con el botón Send porque es el comando que envia información a la tarjeta. El evento que verifica si ha llegado es if serial.available. 
2. [Abre](https://www.asciitable.com/) esta tabla.
   ...
3. Analiza los números que se ven debajo de las letras. Nota que luego de la r, abajo, hay un número. ¿Qué es ese número?
   Son diferentes lenguajes de datos
   
4. ¿Qué relación encuentras entre las letras y los números?
   Para copiar de forma normal algun caracter hay que colocarlo como otro formato, por ejemplo, hexadecimal.
   
5. ¿Qué es el 0a al final del mensaje y para qué crees que sirva?
   Es información en lenguaje hexadecimal y funciona para hacer un salto de linea.
   
6. Nota que luego de verificar si hay datos en el puerto serial se DEBE HACER UNA LECTURA del puerto. Esto se hace para retirar del puerto el dato que llegó. Si esto no se hace entonces parecerá que siempre tiene un datos disponible en el serial para leer. ¿Tiene sentido esto? Si no es así habla con el profe.

### Actividad 12: 
- ¿Cómo se declara un puntero?
  El puntero se declara por ejemplo: uint32_t *pvar;
  
- ¿Cómo se define un puntero? (cómo se inicializa)
  Se inicializa igualandolo a algo.
  
- ¿Cómo se obtiene la dirección de una variable?
  Se alamacena igualandolo a &var.
  
- ¿Cómo se puede leer el contenido de una variable por medio de un puntero?
  Es colocar el nombre del puntero con este simbolo *
  
- ¿Cómo se puede escribir el contenido de una variable por medio de un puntero?
  Se puede escribir con el comando Serial.print(*pvar);
  
### Actividad 13: 
    Resultado: Saca un mensaje por cada constante.

### Actividad 14: 
    ...
 
### Actividad 15: 
- ¿Por qué es necesario declarar `rxData` static? y si no es static ¿Qué pasa? ESTO ES IMPORTANTE, MUCHO.
   
  
- dataCounter se define static y se inicializa en 0. Cada vez que se ingrese a la función loop dataCounter se inicializa a 0? ¿Por qué es necesario declararlo static?
  
- Observa que el nombre del arreglo corresponde a la dirección del primer elemento del arreglo. Por tanto, usar en una expresión el nombre rxData (sin el operador []) equivale a &rxData[0].
  
- En la expresión `sum = sum + (pData[i] - 0x30);` observa que puedes usar el puntero pData para indexar cada elemento del arreglo mediante el operador [].
  
- Finalmente, la constante `0x30` en `(pData[i] - 0x30)` ¿Por qué es necesaria?

### Actividad 16: 
- ¿Qué pasa cuando hago un [Serial.available()](https://www.arduino.cc/reference/en/language/functions/communication/serial/available/)?
  Le estoy preguntando al programa si el puerto serial está disponible. 
  
- ¿Qué pasa cuando hago un [Serial.read()](https://www.arduino.cc/reference/en/language/functions/communication/serial/read/)?
  Lee la información recibida por el puerto serial.
  
- ¿Qué pasa cuando hago un Serial.read() y no hay nada en el buffer de recepción?
  No pasa nada, simplemente no recibe nada y no ejecuta nada.
  
- Un patrón común al trabajar con el puerto serial es este:
  ```
  **`if**(Serial.available() > 0)
  {
    int dataRx = Serial.read()
  }`
  ```
- ¿Cuántos datos lee Serial.read()?
  Solo lee un byte a la vez de buffer de recepción.
   
- ¿Y si quiero leer más de un dato? No olvides que no se pueden leer más datos de los disponibles en el buffer de recepción porque no hay más datos que los que tenga allí.
  Para leer mas de un dato se tendría que usar un ciclo.
  
- ¿Qué pasa si te envían datos por serial y se te olvida llamar Serial.read()?
  Pueden ocurrir diversos errores, por ejemplo que el programa crashea.
  
### Actividad 17: 

### Actividad 18: 

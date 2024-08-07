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

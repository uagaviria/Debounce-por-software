# Debounce por software
El debounce por software tiene la ventaja de no requerir componentes adicionales. Resolvemos el rebote únicamente modificando el código de nuestro programa.

Como desventaja, incrementa levemente el tiempo de ejecución y la complejidad del código. Además, si no aplicamos el código correctamente podemos ignorar interrupciones “verdaderas”.

La forma más sencilla de aplicar un debounce por software es comprobar el tiempo entre disparos de la interrupción. Si el tiempo es inferior a un determinado umbral de tiempo (threshold) simplemente ignoramos la interrupción. En definitiva, hemos definido una “zona muerta” en la que ignoramos las interrupciones generadas.

Para aplicar el debounce por software, modificamos la función ISR de la siguiente forma.

```C++
const int umbralTiempo = 150;
const int pinInterrupcion = 2;
volatile int contadorISR = 0;
int contador = 0;
long tiempoInicio = 0;

void setup()
{
  pinMode(pinInterrupcion, INPUT_PULLUP);
  Serial.begin(9600);
  attachInterrupt(digitalPinToInterrupt(pinInterrupcion), contarConRebote, FALLING);
}

void loop()
{
  if (contador != contadorISR)
  {
    contador = contadorISR;
    Serial.println(contador);
  }
}

void contarConRebote()
{
  if (millis() - tiempoInicio > umbralTiempo)
  {
    contadorISR++;
    tiempoInicio = millis();
  }
}

```
Un tiempo de 100-200ms es correcto para un pulsador pero en otros casos deberemos ajustar el tiempo de forma que eliminemos el rebote, pero no ignoremos dos posibles eventos cercanos “verdaderos”.

En un montaje real, lo mejor es emplear una combinación de ambos sistemas, a la vez que ajustamos correctamente el valor del condensados y los tiempos del filtro por software para adaptarlos a nuestro sistema.

# Trabajando con la libreria bounce2
A continuación se muestra un código de ejemplo para manejar cuatro pulsadores con flanco de bajada utilizando la librería Bounce2:

# Flanco de bajada
```C++
#include <Bounce2.h>

// Pines de los pulsadores
const int pinPulsador1 = 2;
const int pinPulsador2 = 3;
const int pinPulsador3 = 4;
const int pinPulsador4 = 5;

// Crear objetos Bounce para cada pulsador
Bounce boton1 = Bounce();
Bounce boton2 = Bounce();
Bounce boton3 = Bounce();
Bounce boton4 = Bounce();

// Variables para almacenar el estado de los pulsadores
bool estadoPulsador1 = false;
bool estadoPulsador2 = false;
bool estadoPulsador3 = false;
bool estadoPulsador4 = false;

void setup() {
  // Configurar los pines de los pulsadores como entradas con pull-up interno
  pinMode(pinPulsador1, INPUT_PULLUP);
  pinMode(pinPulsador2, INPUT_PULLUP);
  pinMode(pinPulsador3, INPUT_PULLUP);
  pinMode(pinPulsador4, INPUT_PULLUP);

  // Asignar los pines a los objetos Bounce
  boton1.attach(pinPulsador1);
  boton2.attach(pinPulsador2);
  boton3.attach(pinPulsador3);
  boton4.attach(pinPulsador4);

  // Establecer el intervalo de rebote para cada pulsador (en milisegundos)
  boton1.interval(25);
  boton2.interval(25);
  boton3.interval(25);
  boton4.interval(25);

  // Iniciar la comunicación serie para la depuración
  Serial.begin(9600);
}

void loop() {
  // Actualizar el estado de cada pulsador
  boton1.update();
  boton2.update();
  boton3.update();
  boton4.update();

  // Verificar si se ha producido un flanco de bajada en cada pulsador
  if (boton1.fell()) {
    estadoPulsador1 = !estadoPulsador1;
    Serial.println("Pulsador 1 accionado");
  }
  if (boton2.fell()) {
    estadoPulsador2 = !estadoPulsador2;
    Serial.println("Pulsador 2 accionado");
  }
  if (boton3.fell()) {
    estadoPulsador3 = !estadoPulsador3;
    Serial.println("Pulsador 3 accionado");
  }
  if (boton4.fell()) {
    estadoPulsador4 = !estadoPulsador4;
    Serial.println("Pulsador 4 accionado");
  }

  // Aquí puedes agregar acciones adicionales según el estado de los pulsadores
}
```
Explicación del Código
1. Definición de Pines: Los pines 2, 3, 4 y 5 están configurados como entradas con pull-up interno para los pulsadores.
2. Bounce Objects: Se crean objetos Bounce para cada pulsador.
3. Configuración Inicial (setup): Se configuran los pines y se asignan a los objetos Bounce. También se establece un intervalo de rebote de 25 milisegundos para cada pulsador.
4. Bucle Principal (loop): Se actualiza el estado de cada objeto Bounce y se verifica si ha ocurrido un flanco de bajada utilizando el método fell(). Si se detecta un flanco de bajada, se invierte el estado del pulsador correspondiente y se imprime un mensaje en el monitor serie.
Este código proporciona una base para manejar múltiples pulsadores con control de rebote utilizando la librería Bounce2, activando acciones en el flanco de bajada.

Código de Ejemplo
A continuación se muestra un código de ejemplo para manejar cuatro pulsadores con flanco de subida utilizando la librería Bounce2:

# Flanco de subida
A continuación se muestra un código de ejemplo para manejar cuatro pulsadores con flanco de subida utilizando la librería Bounce2:

```cpp
#include <Bounce2.h>

// Pines de los pulsadores
const int pinPulsador1 = 2;
const int pinPulsador2 = 3;
const int pinPulsador3 = 4;
const int pinPulsador4 = 5;

// Crear objetos Bounce para cada pulsador
Bounce boton1 = Bounce();
Bounce boton2 = Bounce();
Bounce boton3 = Bounce();
Bounce boton4 = Bounce();

// Variables para almacenar el estado de los pulsadores
bool estadoPulsador1 = false;
bool estadoPulsador2 = false;
bool estadoPulsador3 = false;
bool estadoPulsador4 = false;

void setup() {
  // Configurar los pines de los pulsadores como entradas con pull-up interno
  pinMode(pinPulsador1, INPUT_PULLUP);
  pinMode(pinPulsador2, INPUT_PULLUP);
  pinMode(pinPulsador3, INPUT_PULLUP);
  pinMode(pinPulsador4, INPUT_PULLUP);

  // Asignar los pines a los objetos Bounce
  boton1.attach(pinPulsador1);
  boton2.attach(pinPulsador2);
  boton3.attach(pinPulsador3);
  boton4.attach(pinPulsador4);

  // Establecer el intervalo de rebote para cada pulsador (en milisegundos)
  boton1.interval(25);
  boton2.interval(25);
  boton3.interval(25);
  boton4.interval(25);

  // Iniciar la comunicación serie para la depuración
  Serial.begin(9600);
}

void loop() {
  // Actualizar el estado de cada pulsador
  boton1.update();
  boton2.update();
  boton3.update();
  boton4.update();

  // Verificar si se ha producido un flanco de subida en cada pulsador
  if (boton1.rose()) {
    estadoPulsador1 = !estadoPulsador1;
    Serial.println("Pulsador 1 accionado");
  }
  if (boton2.rose()) {
    estadoPulsador2 = !estadoPulsador2;
    Serial.println("Pulsador 2 accionado");
  }
  if (boton3.rose()) {
    estadoPulsador3 = !estadoPulsador3;
    Serial.println("Pulsador 3 accionado");
  }
  if (boton4.rose()) {
    estadoPulsador4 = !estadoPulsador4;
    Serial.println("Pulsador 4 accionado");
  }

  // Aquí puedes agregar acciones adicionales según el estado de los pulsadores
}
```

# Explicación del Código
1. Definición de Pines: Los pines 2, 3, 4 y 5 están configurados como entradas con pull-up interno para los pulsadores.
2. Bounce Objects: Se crean objetos Bounce para cada pulsador.
3. Configuración Inicial (setup): Se configuran los pines y se asignan a los objetos Bounce. También se establece un intervalo de rebote de 25 milisegundos para cada pulsador.
4. Bucle Principal (loop): Se actualiza el estado de cada objeto Bounce y se verifica si ha ocurrido un flanco de subida utilizando el método rose(). Si se detecta un flanco de subida, se invierte el estado del pulsador correspondiente y se imprime un mensaje en el monitor serie.
Este código proporciona una base para manejar múltiples pulsadores con control de rebote utilizando la librería Bounce2, activando acciones en el flanco de subida.

# Presionar un pulsador y cuente solo una vez cada vez que se presione
Para crear un código que detecte la presión de un pulsador y cuente solo una vez cada vez que se presione, sin importar cuánto tiempo se mantenga presionado, puedes usar la librería Bounce2 para gestionar el rebote del pulsador. Aquí tienes un ejemplo:

```C++
#include <Bounce2.h>

// Pines de los pulsadores
const int pinPulsador1 = 2;
const int pinPulsador2 = 3;
const int pinPulsador3 = 4;
const int pinPulsador4 = 5;

// Crear objetos Bounce para cada pulsador
Bounce boton1 = Bounce();
Bounce boton2 = Bounce();
Bounce boton3 = Bounce();
Bounce boton4 = Bounce();

// Variables para almacenar el estado de los pulsadores
int contadorPulsador1 = 0;
int contadorPulsador2 = 0;
int contadorPulsador3 = 0;
int contadorPulsador4 = 0;

void setup() {
  // Configurar los pines de los pulsadores como entradas con pull-up interno
  pinMode(pinPulsador1, INPUT_PULLUP);
  pinMode(pinPulsador2, INPUT_PULLUP);
  pinMode(pinPulsador3, INPUT_PULLUP);
  pinMode(pinPulsador4, INPUT_PULLUP);

  // Asignar los pines a los objetos Bounce
  boton1.attach(pinPulsador1);
  boton2.attach(pinPulsador2);
  boton3.attach(pinPulsador3);
  boton4.attach(pinPulsador4);

  // Establecer el intervalo de rebote para cada pulsador (en milisegundos)
  boton1.interval(25);
  boton2.interval(25);
  boton3.interval(25);
  boton4.interval(25);

  // Iniciar la comunicación serie para la depuración
  Serial.begin(9600);
}

void loop() {
  // Actualizar el estado de cada pulsador
  boton1.update();
  boton2.update();
  boton3.update();
  boton4.update();

  // Verificar si se ha producido un flanco de bajada en cada pulsador
  if (boton1.fell()) {
    contadorPulsador1++;
    Serial.print("Pulsador 1 accionado, Contador: ");
    Serial.println(contadorPulsador1);
  }
  if (boton2.fell()) {
    contadorPulsador2++;
    Serial.print("Pulsador 2 accionado, Contador: ");
    Serial.println(contadorPulsador2);
  }
  if (boton3.fell()) {
    contadorPulsador3++;
    Serial.print("Pulsador 3 accionado, Contador: ");
    Serial.println(contadorPulsador3);
  }
  if (boton4.fell()) {
    contadorPulsador4++;
    Serial.print("Pulsador 4 accionado, Contador: ");
    Serial.println(contadorPulsador4);
  }

  // Aquí puedes agregar acciones adicionales según el estado de los pulsadores
}
```

# Ejemplo de Código sin la libreria Bounce2

proporciono un ejemplo simplificado utilizando un solo botón para contar una vez cada vez que se presione, sin utilizar la librería Bounce2. Este código maneja el rebote del botón de manera básica utilizando un tiempo de espera entre pulsaciones para evitar contar múltiples veces por un solo evento de presión.

# Código de Ejemplo
```C++
// Pin del pulsador
const int pinPulsador = 2;

// Variables para almacenar el estado y el contador del pulsador
bool estadoAnteriorPulsador = HIGH; // Estado anterior del pulsador (HIGH = no presionado)
int contadorPulsaciones = 0; // Contador de pulsaciones

// Variables para manejar el debounce (anti-rebote)
unsigned long tiempoUltimaPulsacion = 0; // Tiempo de la última pulsación
const unsigned long debounceDelay = 50; // Tiempo de espera en milisegundos

void setup() {
  // Configurar el pin del pulsador como entrada con pull-up interno
  pinMode(pinPulsador, INPUT_PULLUP);

  // Iniciar la comunicación serie para la depuración
  Serial.begin(9600);
}

void loop() {
  // Leer el estado actual del pulsador
  bool estadoActualPulsador = digitalRead(pinPulsador);

  // Verificar si ha pasado el tiempo de debounce y si el estado ha cambiado
  if (estadoActualPulsador == LOW && estadoAnteriorPulsador == HIGH && millis() - tiempoUltimaPulsacion > debounceDelay) {
    // Registrar la pulsación válida
    contadorPulsaciones++;
    
    // Imprimir información en el monitor serie
    Serial.print("Pulsaciones: ");
    Serial.println(contadorPulsaciones);

    // Actualizar el tiempo de la última pulsación
    tiempoUltimaPulsacion = millis();
  }

  // Actualizar el estado anterior del pulsador
  estadoAnteriorPulsador = estadoActualPulsador;
}
```
# Explicación del Código
Definición de Pines: pinPulsador se define como el pin al que está conectado el pulsador (en este caso, el pin 2).
Variables: estadoAnteriorPulsador se utiliza para almacenar el estado anterior del pulsador (para detectar cambios), y contadorPulsaciones se utiliza para contar las pulsaciones válidas.
Debounce Delay: debounceDelay se establece en 50 milisegundos para manejar el rebote del pulsador.
Configuración Inicial (setup): Se configura pinPulsador como una entrada con pull-up interno y se inicia la comunicación serie para la depuración.
Bucle Principal (loop):
Se lee el estado actual del pulsador (estadoActualPulsador).
Se verifica si el pulsador ha sido presionado (estadoActualPulsador == LOW), si el estado anterior era alto (estadoAnteriorPulsador == HIGH), y si ha pasado el tiempo de debounce (millis() - tiempoUltimaPulsacion > debounceDelay).
Si se cumplen estas condiciones, se incrementa contadorPulsaciones, se imprime en el monitor serie y se actualiza tiempoUltimaPulsacion para evitar contar múltiples veces por un solo evento de presión.
Se actualiza estadoAnteriorPulsador con estadoActualPulsador para la siguiente iteración del bucle.
Este código te permitirá contar una sola vez cada vez que presiones el botón, evitando problemas de rebote comunes en los pulsadores físicos.

# Crear el codigo con una libreria
Sí, podemos organizar el código anterior en una librería para poder llamarlo desde el programa principal de Arduino de manera más modular y organizada. A continuación te mostraré cómo estructurar la librería y cómo usarla en tu programa principal.

# Creación de la Librería
1. Crear un nuevo archivo de cabecera (header file): Este archivo contendrá la declaración de la clase y los métodos de la librería. Guarda este archivo como ContadorPulsador.h

```c++
#ifndef ContadorPulsador_h
#define ContadorPulsador_h

#include <Arduino.h>

class ContadorPulsador {
public:
  ContadorPulsador(int pin);
  void setup();
  void loop();

private:
  int _pin;
  bool _estadoAnteriorPulsador;
  int _contadorPulsaciones;
  unsigned long _tiempoUltimaPulsacion;
  const unsigned long _debounceDelay = 50;
};

#endif
```

2. Crear un archivo de implementación (source file): Este archivo contendrá la implementación de los métodos de la clase. Guarda este archivo como ContadorPulsador.cpp

```c++
#include "ContadorPulsador.h"

ContadorPulsador::ContadorPulsador(int pin) {
  _pin = pin;
}

void ContadorPulsador::setup() {
  pinMode(_pin, INPUT_PULLUP);
  _estadoAnteriorPulsador = HIGH;
  _contadorPulsaciones = 0;
  _tiempoUltimaPulsacion = 0;
  Serial.begin(9600);
}

void ContadorPulsador::loop() {
  bool estadoActualPulsador = digitalRead(_pin);

  if (estadoActualPulsador == LOW && _estadoAnteriorPulsador == HIGH && millis() - _tiempoUltimaPulsacion > _debounceDelay) {
    _contadorPulsaciones++;
    Serial.print("Pulsaciones: ");
    Serial.println(_contadorPulsaciones);
    _tiempoUltimaPulsacion = millis();
  }

  _estadoAnteriorPulsador = estadoActualPulsador;
}
```

# Uso de la Librería en el Programa Principal
Ahora, puedes usar la librería ContadorPulsador en tu programa principal de Arduino (sketch).

```c++
#include <ContadorPulsador.h>

// Instancia de la clase ContadorPulsador
ContadorPulsador contadorPulsador(2); // Pin del pulsador

void setup() {
  contadorPulsador.setup();
}

void loop() {
  contadorPulsador.loop();
  // Aquí puedes agregar otras instrucciones para el loop principal si las necesitas
}

```

# Explicación
1. Librería (ContadorPulsador.h): En este archivo, se define la clase ContadorPulsador que encapsula la funcionalidad de contar pulsaciones de un botón con debounce. Se declaran los métodos setup() y loop() que se implementan en el archivo ContadorPulsador.cpp.

2. Implementación (ContadorPulsador.cpp): Aquí se implementan los métodos de la clase ContadorPulsador, incluyendo la inicialización (setup()) y el bucle de control (loop()).

3. Uso en el Programa Principal: En el programa principal de Arduino, se incluye la librería ContadorPulsador.h y se crea una instancia de ContadorPulsador. En el setup(), se llama al método setup() de la instancia para configurar el pin del pulsador. En el loop(), se llama al método loop() para manejar el conteo de pulsaciones del botón.

4. Este enfoque hace que tu código sea más modular y fácil de mantener, especialmente si necesitas agregar más funcionalidades o manejar múltiples pulsadores en tu proyecto Arduino.


# Incrementar mas pines en el código

Para incrementar la capacidad de manejar más pines de pulsadores en la librería ContadorPulsador, puedes hacer algunos ajustes para permitir la configuración dinámica de múltiples pines. Aquí te muestro cómo podrías modificar la librería para manejar varios pines de pulsadores:

1. Modificación de la Librería ContadorPulsador
Modificar el Archivo de Cabecera (ContadorPulsador.h)

```c++
#ifndef ContadorPulsador_h
#define ContadorPulsador_h

#include <Arduino.h>

// Definir la cantidad máxima de pines de pulsadores que quieres manejar
#define MAX_PULSADORES 4

class ContadorPulsador {
public:
  ContadorPulsador(int pin[], int cantidad);
  void setup();
  void loop();

private:
  int _pin[MAX_PULSADORES];
  int _cantidadPulsadores;
  bool _estadoAnteriorPulsador[MAX_PULSADORES];
  int _contadorPulsaciones[MAX_PULSADORES];
  unsigned long _tiempoUltimaPulsacion[MAX_PULSADORES];
  const unsigned long _debounceDelay = 50;
};

#endif


```

2. Modificar el Archivo de Implementación (ContadorPulsador.cpp)

#include "ContadorPulsador.h"

```c++
ContadorPulsador::ContadorPulsador(int pin[], int cantidad) {
  _cantidadPulsadores = cantidad;
  for (int i = 0; i < _cantidadPulsadores; i++) {
    _pin[i] = pin[i];
  }
}

void ContadorPulsador::setup() {
  for (int i = 0; i < _cantidadPulsadores; i++) {
    pinMode(_pin[i], INPUT_PULLUP);
    _estadoAnteriorPulsador[i] = HIGH;
    _contadorPulsaciones[i] = 0;
    _tiempoUltimaPulsacion[i] = 0;
  }
  Serial.begin(9600);
}

void ContadorPulsador::loop() {
  for (int i = 0; i < _cantidadPulsadores; i++) {
    bool estadoActualPulsador = digitalRead(_pin[i]);

    if (estadoActualPulsador == LOW && _estadoAnteriorPulsador[i] == HIGH && millis() - _tiempoUltimaPulsacion[i] > _debounceDelay) {
      _contadorPulsaciones[i]++;
      Serial.print("Pulsaciones Pulsador ");
      Serial.print(i + 1);
      Serial.print(": ");
      Serial.println(_contadorPulsaciones[i]);
      _tiempoUltimaPulsacion[i] = millis();
    }

    _estadoAnteriorPulsador[i] = estadoActualPulsador;
  }
}
```

# Uso en el Programa Principal

Para utilizar la librería con varios pines de pulsadores, modifica tu programa principal de esta manera:

```c++
#include <ContadorPulsador.h>
// Definir los pines de los pulsadores
int pinesPulsadores[] = {2, 3, 4, 5}; // Por ejemplo, pines 2, 3, 4 y 5

// Definir la cantidad de pulsadores
int cantidadPulsadores = sizeof(pinesPulsadores) / sizeof(pinesPulsadores[0]);

// Instancia de la clase ContadorPulsador
ContadorPulsador contadorPulsador(pinesPulsadores, cantidadPulsadores);

void setup() {
  contadorPulsador.setup();
}

void loop() {
  contadorPulsador.loop();
  // Aquí puedes agregar otras instrucciones para el loop principal si las necesitas
}
```

# Retrazaria nuestra libreria el arduino? 

Explicación
1. Definición de Pines: pinesPulsadores es un arreglo que contiene los pines a los que están conectados los pulsadores.
Cantidad de Pulsadores: cantidadPulsadores se calcula como el tamaño del arreglo pinesPulsadores.
2. Librería Modificada: Se ha modificado la librería ContadorPulsador para aceptar un arreglo de pines y una cantidad de pulsadores como parámetros en el constructor. Esto permite manejar múltiples pines de pulsadores de manera dinámica.
3. Uso en el Programa Principal: En el programa principal de Arduino, se instancia ContadorPulsador con el arreglo pinesPulsadores y cantidadPulsadores. Luego, se llama a setup() en el setup() principal y a loop() en el loop() principal para manejar el conteo de pulsaciones para cada pulsador individualmente.
Este enfoque te permite ampliar fácilmente el número de pines de pulsadores que deseas manejar en tu proyecto Arduino, manteniendo un código organizado y modular utilizando la librería ContadorPulsador.

si deseas manejar más pines de pulsadores utilizando la librería ContadorPulsador, simplemente necesitas ajustar la constante MAX_PULSADORES en el archivo de cabecera (ContadorPulsador.h)

El programa descrito, que utiliza la librería ContadorPulsador para gestionar el conteo de pulsaciones de un solo pulsador, no introduce un retraso significativo en el funcionamiento general del Arduino. Sin embargo, es importante entender cómo podría afectar el rendimiento del microcontrolador en términos de la eficiencia del código y la gestión del tiempo.

Consideraciones sobre el Retraso
Debounce y Rendimiento: El debounce implementado en el código (_debounceDelay) introduce un pequeño retraso (50 milisegundos en este caso) para evitar contar múltiples pulsaciones debido al rebote mecánico de los pulsadores. Este retraso es corto y generalmente no afecta negativamente el rendimiento general del Arduino, especialmente si no hay otras tareas críticas que dependan de tiempos muy precisos.

Iteración sobre Pulsadores: Si se manejan múltiples pulsadores (aunque en este ejemplo se maneja solo uno), la iteración sobre cada pulsador en el método loop() puede consumir tiempo adicional, dependiendo de la cantidad de pulsadores y de la complejidad de otras operaciones que puedas agregar dentro del bucle.

Comunicación Serie: El uso de Serial.print() para mostrar el número de pulsaciones en el monitor serie también puede introducir un pequeño retraso, especialmente si se utiliza a una velocidad de baudios alta (como en este caso, 9600 baudios). A velocidades de baudios más altas, la transmisión de datos por serie puede consumir más recursos del microcontrolador.

Impacto General
En términos generales, el programa descrito no debería causar problemas de rendimiento significativos en Arduino, siempre y cuando se gestione correctamente y se mantenga dentro de las capacidades del microcontrolador. Arduino Mega, por ejemplo, tiene más recursos (como memoria y puertos de E/S) en comparación con otros modelos de Arduino, lo que le permite manejar más tareas sin problemas.

Sin embargo, es importante tener en cuenta las siguientes prácticas para optimizar el rendimiento y evitar posibles problemas:

Evitar Bloqueos: Asegúrate de no utilizar delay() u otras funciones que bloqueen el flujo del programa por largos períodos, ya que esto podría hacer que el Arduino no responda a eventos externos o deje de ejecutar otras partes importantes del código.

Optimización del Código: Si planeas manejar muchas tareas simultáneamente o utilizar muchas interrupciones, considera optimizar tu código para minimizar el uso de recursos y mejorar la eficiencia.

Monitoreo y Depuración: Utiliza herramientas de monitoreo (como el monitor serie) para comprender mejor el rendimiento de tu código y detectar posibles cuellos de botella o problemas de rendimiento.

En resumen, el programa que maneja un solo pulsador utilizando la librería ContadorPulsador no debería causar un retraso significativo en el Arduino, siempre y cuando se implemente y maneje adecuadamente.
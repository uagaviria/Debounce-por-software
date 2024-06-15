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

Explicación del Código
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
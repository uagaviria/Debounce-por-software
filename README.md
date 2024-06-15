# Debounce por software
El debounce por software tiene la ventaja de no requerir componentes adicionales. Resolvemos el rebote únicamente modificando el código de nuestro programa.

Como desventaja, incrementa levemente el tiempo de ejecución y la complejidad del código. Además, si no aplicamos el código correctamente podemos ignorar interrupciones “verdaderas”.

La forma más sencilla de aplicar un debounce por software es comprobar el tiempo entre disparos de la interrupción. Si el tiempo es inferior a un determinado umbral de tiempo (threshold) simplemente ignoramos la interrupción. En definitiva, hemos definido una “zona muerta” en la que ignoramos las interrupciones generadas.

Para aplicar el debounce por software, modificamos la función ISR de la siguiente forma.

```C++
const int timeThreshold = 150;
const int intPin = 2;
volatile int ISRCounter = 0;
int counter = 0;
long startTime = 0;

void setup()
{
  pinMode(intPin, INPUT_PULLUP);
  Serial.begin(9600);
  attachInterrupt(digitalPinToInterrupt(intPin), debounceCount, FALLING);
}

void loop()
{
  if (counter != ISRCounter)
  {
    counter = ISRCounter;
    Serial.println(counter);
  }
}

void debounceCount()
{
  if (millis() - startTime > timeThreshold)
  {
    ISRCounter++;
    startTime = millis();
  }
}

```
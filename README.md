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
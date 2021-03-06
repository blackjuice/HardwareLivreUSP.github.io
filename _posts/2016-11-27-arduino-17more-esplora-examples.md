---
layout:     post
title:      "Tutoriais Arduino #17: Placas - Esplora: Exemplos"
date:       2016-11-27
author:     "Leonardo Haddad Carlos"
img:        "post_img/arduino_oscomm.png"
---

## Mais Exemplos para a Esplora

Usar os sensores de entrada e atuadores de saída construídos na placa Esplora é um pouco diferente do que usar as entradas e saídas gerais em outras placas Arduino. Como resultado, os esboços de exemplo incluídos com o Arduino IDE precisam de uma pequena modificação antes de poder usá-los com a Esplora. Este guia explica como fazer isso.

## 1 | Adicione a Biblioteca Esplora

Você precisa adicionar a biblioteca da Esplora em qualquer esboço que você queira usar na Esplora. Para isso, escolha `Import Library...`> `Esplora` no menu `Sketch` e o IDE adicionará automaticamente a seguinte linha de código na parte superior do seu esboço:

```sh
#include <Esplora.h>
```

## 2 | Diferenças nas entradas e saídas digitais

As outras placas Arduino têm dois tipos de entradas: entradas digitais, que têm apenas dois estados, HIGH ou LOW, e entradas analógicas, que têm um intervalo variável de estados, normalmente de 0 a 1023. Os botões na Esplora são entradas digitais, E os outros sensores são entradas analógicas. Para adaptar os exemplos regulares ao Esplora, você pode substituir as entradas de botão para as entradas digitais, e os outros sensores para as entradas analógicas.

As entradas digitais nas outras placas Arduino também podem ser usadas como saídas, então elas precisam ser declaradas como entrada ou saída usando um comando chamado `pinMode()`. Como todas as entradas ou saídas do Esplora são dedicadas à função que carregam no nome, você não precisa usar o comando pinMode.

Sempre que você vir o comando `digitalRead()` em um exemplo do Arduino, substitua o comando `Esplora.readButton()`. Escolha o botão que deseja ler. Abaixo, você pode ver o exemplo original de `DigitalReadSerial()` encontrado em `File`> `Examples`> `01.Basics`> `DigitalReadSerial`) e a versão modificada que funciona na Esplora usando o Switch 1:

### Exemplo original

```sh
// digital pin 2 has a pushbutton attached to it. Give it a name:
int pushButton = 2;                        // you don't need this line for the Esplora
 
// the setup routine runs once when you press reset:
void setup() {
  // initialize serial communication at 9600 bits per second:
  Serial.begin(9600);
  // make the pushbutton's pin an input:
  pinMode(pushButton, INPUT);       // you don't need this line for the Esplora
}
 
// the loop routine runs over and over again forever:
void loop() {
  // read the input pin:
  int buttonState = digitalRead(pushButton); // this line needs to change
  // print out the state of the button:
  Serial.println(buttonState);
  delay(1);        // delay in between reads for stability
}
```
[Obter este código][getcode-pboriginal]

### Exemplo modificado para usar na Esplora

```sh
#include <Esplora.h>     // you need to include the Esplora Library
// the setup routine runs once when you press reset:
void setup() {
  // initialize serial communication at 9600 bits per second:
  Serial.begin(9600);
}
 
// the loop routine runs over and over again forever:
void loop() {
  // read the input pin:
  int buttonState = Esplora.readButton(SWITCH_2);  // Esplora.readButton replaces digitalRead()
  // print out the state of the button:
  Serial.println(buttonState);
  delay(1);        // delay in between reads for stability
}
```
[Obter este código][getcode-pbesplora]

A maioria dos exemplos de `digitalRead()` do Arduino são escritos com a suposição de que o botão ou interruptor conectado a eles retornará HIGH quando pressionado e LOW quando não pressionado. Os botões da Esplora ficam LOW quando pressionados e HIGH quando não pressionados. Então, se você quer ler quando os botões do Esplora são pressionados, leia para LOW em vez de HIGH. Se você quiser ler quando eles não são pressionados, leia para HIGH em vez de LOW.

Para a maioria dos exemplos do Arduino, os LEDs são usados como saídas digitais, o que significa que só é possível ativá-los ou desativá-los (HIGH ou LOW). O LED RGB no Esplora, no entanto, é usado como uma saída analógica, o que significa que você pode definir o seu brilho de 0 a 255. Para fazê-lo agir como uma saída digital, defina o seu nível como 255 para HIGH e 0 para LOW.

Abaixo, você verá o exemplo Button (encontrado em `File`> `02.Digital`> `Button`) em suas formas original e modificada:

### Exemplo original

Algumas linhas mudarão. As constantes de pino e os comandos `pinMode()` desaparecerão porque não são mais necessários.

```sh
// constants won't change. They're used here to
// set pin numbers:
const int buttonPin = 2;     // the number of the pushbutton pin
const int ledPin =  13;      // the number of the LED pin
 
// variables will change:
int buttonState = 0;         // variable for reading the pushbutton status
 
void setup() {
  // initialize the LED pin as an output:
  pinMode(ledPin, OUTPUT);      
  // initialize the pushbutton pin as an input:
  pinMode(buttonPin, INPUT);    
}
 
void loop(){
  // read the state of the pushbutton value:
  buttonState = digitalRead(buttonPin);
 
  // check if the pushbutton is pressed.
  // if it is, the buttonState is HIGH:
  if (buttonState == HIGH) {    
    // turn LED on:    
    digitalWrite(ledPin, HIGH);  
  }
  else {
    // turn LED off:
    digitalWrite(ledPin, LOW);
  }
}
```
[Obter este código][getcode-btnoriginal]

### Exemplo modificado para usar na Esplora

```sh
#include <Esplora.h>   // you need to include the Esplora library

int buttonState = 0;   // variable for reading the pushbutton status
 
void setup() {
 // nothing to set up
}
 
void loop(){
  // read the state of the pushbutton value:
  // Esplora.readButton replaces digitalRead()
  buttonState = Esplora.readButton(SWITCH_1);
 
  // check if the pushbutton is pressed.
  // if it is, the buttonState is LOW:
  if (buttonState == LOW) {     // Button is pushed when LOW, not HIGH
    // turn LED on:
    Esplora.writeRed(255);  // Esplora.writeRed() replaces digitalWrite()
  }
  else {
    // turn LED off:
    Esplora.writeRed(0);  // Esplora.writeRed() replaces digitalWrite()
  }
}
```
[Obter este código][getcode-btnesplora]

Você não precisa usar apenas o canal vermelho do LED; você pode usar qualquer canal RGB como saída digital desta maneira. Da mesma forma, você pode usar quaisquer botões para substituir entradas digitais.

## 3 | Diferença nas entrada e saídas analógicas

As alterações aos exemplos analógicos são semelhantes às digitais. Não há comandos `pinMode()` para remover, no entanto, porque as entradas analógicas nas outras placas Arduino são entradas por padrão.

Aqui está outro exemplo, desta vez substituindo uma entrada analógica por uma das entradas analógicas da Esplora. O exemplo `ReadAnalogVoltage` (encontrado em `File`> `01.Basics`> `ReadAnalogVoltage`) lê uma entrada analógica e informa a tensão no pino. Abaixo, você substituirá o comando `analogRead()` pela leitura do sensor de luz:

### Exemplo original

```sh
// the setup routine runs once when you press reset:
void setup() {
  // initialize serial communication at 9600 bits per second:
  Serial.begin(9600);
}
 
// the loop routine runs over and over again forever:
void loop() {
  // read the input on analog pin 0:
  int sensorValue = analogRead(A0);    // You need to change this line
  // Convert the analog reading (which goes from 0 - 1023) to a voltage (0 - 5V):
  float voltage = sensorValue * (5.0 / 1023.0);
  // print out the value you read:
  Serial.println(voltage);
}
```
[Obter este código][getcode-inputoriginal]

### Exemplo modificado para usar na Esplora

```sh
#include <Esplora.h>     // you need to include the Esplora Library
// the setup routine runs once when you press reset:
void setup() {
  // initialize serial communication at 9600 bits per second:
  Serial.begin(9600);
}
 
// the loop routine runs over and over again forever:
void loop() {
  // read the input on analog pin 0:
  int sensorValue = Esplora.readLightSensor();  //Esplora.readLightSensor() replaces analogRead()
  // Convert the analog reading (which goes from 0 - 1023) to a voltage (0 - 5V):
  float voltage = sensorValue * (5.0 / 1023.0);
  // print out the value you read:
  Serial.println(voltage);
}
```
[Obter este código][getcode-inputnesplora]

A maioria dos sensores analógicos na Esplora pode ser substituída por instruções`analogRead()`, porque eles são simplesmente isso: sensores analógicos. O joystick é feito de dois potenciômetros, assim você pode pensar em cada um de seus eixos como um `analógicoRead()`. Da mesma forma, o acelerômetro pode ser pensado em três canais `analógicosRead()`, um para cada eixo.

No entanto, o sensor de temperatura da Esplora é diferente dos outros sensores analógicos. O comando [`Esplora.readTemperature()`][cmd-temp] não lhe dá simplesmente a leitura analógica como fazem os outros comandos do sensor. Em vez disso, converte a leitura do sensor em Celsius ou Fahrenheit. Assim, você não pode simplesmente substituir o sensor de temperatura por um comando `analogRead()`.

O comando [`analogWrite()`][cmd-write] em outras Arduinos funciona somente em determinados pinos. Sua faixa é de 0 a 255, assim como os comandos [`Esplora.writeRed()`][cmd-red], [`Esplora.writeGreen()`][cmd-green] e [`Esplora.writeBlue()`][cmd-blue]. Assim, você pode substituir as instruções de `analogWrite()` por qualquer um desses três comandos. Observe que `analogWrite()` leva dois parâmetros, o número do pino e um valor de brilho, enquanto os comandos `Esplora.writeRed()` e relacionados só levam um parâmetro, o brilho. Abaixo você pode ver o exemplo `Fade` (encontrado em `File`> `Examples`> `01.Basics`> `Fade`) em suas formas original e modificada:

## Exemplo original

O comando `pinMode()` será removido, pois não é necessário.

```sh
nt led = 9;           // the pin that the LED is attached to
int brightness = 0;    // how bright the LED is
int fadeAmount = 5;    // how many points to fade the LED by
 
// the setup routine runs once when you press reset:
void setup()  {
  // declare pin 9 to be an output:
  pinMode(led, OUTPUT);
}
 
// the loop routine runs over and over again forever:
void loop()  {
  // set the brightness of pin 9:
  analogWrite(led, brightness);    
 
  // change the brightness for next time through the loop:
  brightness = brightness + fadeAmount;
 
  // reverse the direction of the fading at the ends of the fade:
  if (brightness == 0 || brightness == 255) {
    fadeAmount = -fadeAmount ;
  }    
  // wait for 30 milliseconds to see the dimming effect    
  delay(30);                            
}
```
[Obter este código][getcode-writeoriginal]

### Exemplo modificado para usar na Esplora

```sh
#include <Esplora.h>   // you need to include the Esplora library
 
int brightness = 0;    // how bright the LED is
int fadeAmount = 5;    // how many points to fade the LED by
 
// the setup routine runs once when you press reset:
void setup()  {
 // nothing to set up
}
 
// the loop routine runs over and over again forever:
void loop()  {
  // set the brightness of the blue channel:
  Esplora.writeBlue(brightness);    // Esplora.writeBlue() replaces analogWrite()
 
  // change the brightness for next time through the loop:
  brightness = brightness + fadeAmount;
 
  // reverse the direction of the fading at the ends of the fade:
  if (brightness == 0 || brightness == 255) {
    fadeAmount = -fadeAmount ;
  }    
  // wait for 30 milliseconds to see the dimming effect    
  delay(30);                            
}
```
[Obter este código][getcode-writeesplora]

## 4 | Comunicação com o computador via USB

A comunicação serial através via USB usando os comandos [`Serial.read()`][cmd-sread], [`Serial.write()`][cmd-swrite], [`Serial.print()`][cmd-print] e [`Serial.println()`][cmd-println] devem funcionar na Esplora da mesma forma que em outras placas. No entanto, a saída serial da Esplora funciona ligeiramente mais rápido do que a da Uno, então você pode querer adicionar um pequeno atraso para esboços que não fazem nada além de ler um sensor e imprimir em série o resultado, de modo a não preencher o buffer da entrada serial do seu computador. Quando o buffer serial do seu computador se enche, o Monitor Serial é executado muito mais lentamente e você experimentar um atraso quando alternar entre a janela dele e a janela principal do IDE. Um atraso de 1 milissegundo já é suficiente.

Você também pode usar as [bibliotecas de Mouse e Teclado USB][mouse-keyboard] na Esplora. Os exemplos encontrados em `File`> `Examples`> `09.USB` funcionarão apenas após aplicar as modificações de entradas e saídas digitais e analógicas que acabamos de descrever acima. Há um exemplo para a Esplora chamado [`EsploraJoystickMouse`][joystick], que permite que você também use o joystick como um controlador de mouse.

## 5 | Comunicação com outros dispositivos

As outros Arduinos oferecem duas outras formas de comunicação serial, SPI (a usando [biblioteca SPI][spilib]) e [I2C (usando a biblioteca Wire)][wireref]. A Esplora pode se comunicar via SPI usando o cabeçalho (header) ICSP que também é usado para a programação serial em circuito (opcional) da placa. Os pinos do conector ICSP são dispostos como segue. O pino 1 é o pino mais próximo do ponto branco da placa Esplora. É o pino inferior direito se você está segurando o Esplora com o conector USB virado para cima:

<p style="text-align: center;">
    <img src="{{ site.baseurl }}/post_img/arduinotutorials/esplora_spi.jpg" style="margin: 0 auto; max-height: 390px;" />
Para conectar um dispositivo SPI à Esplora, você terá que fazer seu próprio cabo para este conector.
</p>

A Esplora não expõe pinos para fornecer comunicação I2C e, assim, você não será capaz de usar exemplos que usam a biblioteca Wire com a Esplora.

Geralmente, se você precisa de conectividade SPI ou I2C, é melhor usar outro modelo do Arduino.

## 6 | Após a Esplora

Depois de ter dominado a Esplora, se você estiver procurando por outras placas Arduino para tentar, o próximo melhor passo é a [Arduino Uno][uno], que é o coração da linha Arduino. Permite que você conecte seus próprios circuitos de sensores e atuadores, ou shields add-on para capacidades expandidas. Você também pode querer considerar a [Arduino Leonardo][leonardo]. É baseada no mesmo processador que a Esplora, e pode igualmente funcionar como um teclado ou mouse USB. Ela também oferece todas as funcionalidades das placas regulares Arduino.

License
----

O texto do guia de iniciação do Arduino está publicado sob a licença [Creative Commons Attribution-ShareAlike 3.0][ccasa3]. Os exemplos de código do guia são disponibilizados para o domínio público.

Link para a página original: [Getting Started Guide - Boards - Esplora: Examples][originalpage].

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)


   [placeholder]: <>
   [leonardo]: <https://www.arduino.cc/en/Main/ArduinoBoardLeonardo>
   [uno]: <https://www.arduino.cc/en/Main/ArduinoBoardUno>
   [wireref]: <https://www.arduino.cc/en/Reference/Wire>
   [spilib]: <https://www.arduino.cc/en/Reference/SPI>
   [joystick]: <https://www.arduino.cc/en/Tutorial/EsploraJoystickMouse>
   [mouse-keyboard]: <https://www.arduino.cc/en/Reference/MouseKeyboard>
   [cmd-println]: <https://www.arduino.cc/en/Serial/Println>
   [cmd-print]: <https://www.arduino.cc/en/Serial/Print>
   [cmd-swrite]: <https://www.arduino.cc/en/Serial/Write>
   [cmd-sread]: <https://www.arduino.cc/en/Serial/Read>
   [getcode-writeesplora]: <https://www.arduino.cc/en/Guide/ArduinoEsploraExamples?action=sourceblock&num=9>
   [getcode-writeorigina]: <https://www.arduino.cc/en/Guide/ArduinoEsploraExamples?action=sourceblock&num=8>
   [cmd-blue]: <https://www.arduino.cc/en/Reference/EsploraWriteBlue>
   [cmd-green]: <https://www.arduino.cc/en/Reference/EsploraWriteGreen>
   [cmd-red]: <https://www.arduino.cc/en/Reference/EsploraWriteRed>
   [cmd-write]: <https://www.arduino.cc/en/Reference/AnalogWrite>
   [cmd-temp]: <https://www.arduino.cc/en/Reference/EsploraReadTemperature>
   [getcode-inputnesplora]: <https://www.arduino.cc/en/Guide/ArduinoEsploraExamples?action=sourceblock&num=7>
   [getcode-inputoriginal]: <https://www.arduino.cc/en/Guide/ArduinoEsploraExamples?action=sourceblock&num=6>
   [getcode-btnesplora]: <https://www.arduino.cc/en/Guide/ArduinoEsploraExamples?action=sourceblock&num=5>
   [getcode-btnoriginal]: <https://www.arduino.cc/en/Guide/ArduinoEsploraExamples?action=sourceblock&num=4>
   [getcode-pbesplora]: <https://www.arduino.cc/en/Guide/ArduinoEsploraExamples?action=sourceblock&num=3>
   [getcode-pboriginal]: <https://www.arduino.cc/en/Guide/ArduinoEsploraExamples?action=sourceblock&num=2>
   [reference]: <https://www.arduino.cc/en/Reference/HomePage>
   [tutexamples]: <https://www.arduino.cc/en/Tutorial/HomePage>
   [unohub]: <https://create.arduino.cc/projecthub/products/arduino-uno-genuino-uno>
   [troubleshooting]: </2016/11/25/arduino-10troubleshooting/>
   [environment]: </2016/11/21/arduino-7environment/>
   [stepsxp]: <https://www.arduino.cc/en/Guide/UnoDriversWindowsXP>
   [firststeps]: </2016/11/20/arduino-1start/>
   [originalpage]: <https://www.arduino.cc/en/Guide/ArduinoEsplora>
   [ccasa3]: <https://creativecommons.org/licenses/by-sa/3.0>
   [arduino]: <https://www.arduino.cc>

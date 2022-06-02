# Práctica 5a

El objetivo de la practica es comprender el funcionamiento de los buses sistemas de
comunicación entre periféricos; estos elementos pueden ser internos o externos al
procesador.
## Introducción:

La función del bus es establecer la conexión lógica entre los subsistemas que componen el computador. Los buses definen su capacidad de acuerdo a la frecuencia máxima de envío y ancho de datos, estos valores suelen ser proporcionales. Hay diferentes tipos de buses:

- Bus paralelo: gran cantidad de datos con frecuencia moderada (se envían datos en bytes).
- Bus serie: los datos son enviados bit a bit y se reconstruyen al llegar al destino.
- Bus I2C: fue desarrollado para la comunicación interna de dispositivos electrónicos. Requiere 2 cables para su funcionamiento (uno señal de reloj CLK y otro para el envio de datos SDA). Tiene una arquitectura de tipo maestro-esclavo (los esclavos no pueden iniciar comunicación).

---
## Código:

```
#include <Arduino.h>
#include <Wire.h>
 
 
void setup()
{
  Wire.begin();
 
  Serial.begin(115200);
  while (!Serial);             // Leonardo: wait for serial monitor
  Serial.println("\nI2C Scanner");
}
 
 
void loop()
{
  byte error, address;
  int nDevices;
 
  Serial.println("Scanning...");
 
  nDevices = 0;
  for(address = 1; address < 127; address++ )
  {
    // The i2c_scanner uses the return value of
    // the Write.endTransmisstion to see if
    // a device did acknowledge to the address.
    Wire.beginTransmission(address);
    error = Wire.endTransmission();
 
    if (error == 0)
    {
      Serial.print("I2C device found at address 0x");
      if (address<16)
        Serial.print("0");
      Serial.print(address,HEX);
      Serial.println("  !");
 
      nDevices++;
    }
    else if (error==4)
    {
      Serial.print("Unknown error at address 0x");
      if (address<16)
        Serial.print("0");
      Serial.println(address,HEX);
    }    
  }
  if (nDevices == 0)
    Serial.println("No I2C devices found\n");
  else
    Serial.println("done\n");
 
  delay(5000);           // wait 5 seconds for next scan
}
```
---
## Describir la salida por el puerto serie i funcionamiento:


Esta primera parte de la práctica tiene la funcion de decir en que dirección está el dispositivo I2C conectado. 

Mostrará salidas diferentes dependiendo de la cantidad de dispositivos que hayan conectados.

+ Si algún error en el dispositivo I2C conectado la salida será:"Unknown error at address 0x"
```
 else if (error==4)
    {
      Serial.print("Unknown error at address 0x");
      if (address<16)
        Serial.print("0");
      Serial.println(address,HEX);
    }    
```
+ Si hay algun dispositivo I2C conectado la salida será:"I2C device found at address 0x"
```
if (error == 0)
    {
      Serial.print("I2C device found at address 0x");
      if (address<16)
        Serial.print("0");
      Serial.print(address,HEX);
      Serial.println("  !");
 
      nDevices++;
    }
}
```
+ Por último, si no se encuentra ningun dispositivo I2C conectado la salida será: "No I2C devices found\n"

```
if (nDevices == 0)
    Serial.println("No I2C devices found\n");
  else
    Serial.println("done\n");
 
  delay(5000);           // wait 5 seconds for next scan
}
```
# PDM Physical Computing

Digital Design & Emergent Media
[lsuPathways](http://lsupathways.org)
[Programming Digital Media](http://pdm.lsupathways.org)

## Introduction

This framework is made for simplifying communication between p5 sketches and arduino.  It provides two libraries, one for p5 and one for arduino, that simplify and unify the interface for serial communications between the two platforms.


## Arduino

Setup the Sketch by including the PDMSerial.h library

~~~
#include "PDMSerial.h"
// #include <PDMSerial.h>   // use if PDMSerial is in your libraries folder

PDMSerial pdm;
~~~

Sampling sensor data and transmitting

~~~
  int pressureValue = analogRead(pressurePin);
  pdm.transmitSensor("pressure",pressureValue);
  // add more transmissions here, then send the end command
  pdm.transmitSensor("end");
~~~

Receiving Data

~~~
boolean newData = pdm.checkSerial();
  
if(newData) {
  if(pdm.getName().equals(String("led"))) {
    digitalWrite(outPin, pdm.getValue());
  } else if (pdm.getName().equals(String("fade"))) {
    analogWrite(outPWMPin, pdm.getValue());
  }
}
~~~


## P5

~~~
let serialPDM;    // Variable to hold instance of serialport library
let portName = '/dev/cu.usbmodem14101';    // Fill in your serial port name here

let sensors;

function setup() {
    // Setup the serial port for communication
  serialPDM = new PDMSerial(portName);
  
    // Alias for the sensor Data . You can also just use serialPDM.sensorData...
  sensors = serialPDM.sensorData;
  
  createCanvas(800,600);
}
~~~

Transmit Data

~~~
serialPDM.transmit('led',1);
~~~

Receiving Data

~~~
sensors.pressure
~~~



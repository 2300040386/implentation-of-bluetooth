# implentation-of-bluetooth

This design implements a Bluetooth-controlled home automation system using an Arduino, an HC-05 Bluetooth module, and a relay module, allowing smartphone control of devices. The deliverables include a circuit diagram, Arduino code, and a demonstration of the system's functionality.
1. Components:
•	Arduino Board: (e.g., Arduino Uno) - The microcontroller for processing data and controlling relays.
•	HC-05 Bluetooth Module: For wireless communication with a smartphone.
•	Relay Module: (e.g., 4-channel relay module) - To switch AC devices on and off.
•	Smartphone: With a Bluetooth-enabled app to send commands.
•	Power Supply: To power the Arduino and relay module.
•	Jumper Wires: To connect the components.
•	Breadboard (Optional): For prototyping and testing. 
2. Circuit Diagram:
•	Arduino to HC-05:
•	Arduino RX pin to HC-05 TX pin.
•	Arduino TX pin to HC-05 RX pin.
•	Arduino 3.3V pin to HC-05 VCC pin.
•	Arduino GND pin to HC-05 GND pin.
•	Arduino to Relay Module:
•	Arduino digital pins (e.g., D2, D3, D4, D5) to Relay module IN1, IN2, IN3, IN4 pins.
•	Relay module VCC pin to Arduino 5V pin.
•	Relay module GND pin to Arduino GND pin.
•	Relay Module to AC Device:
•	Relay module COM pin to AC device's power line.
•	Relay module NO pin to AC device's power line.
•	Relay module NC pin to AC device's power line
Code 
#include<reg51.h>
sbit Fan=P2^0;
sbit Light=P2^1;
sbit TV=P2^2;
 char str;
 char Charin=0;
void delay(int time)
{
 unsigned int i,j;
 for(i=0;i<time;i++)
 for(j=0;j<1275;j++);
}
void Serialwrite(char byte)
{
  SBUF=byte;
  while(!TI);
  TI=0;
}
void Serialprintln(char *p)
{
  while(*p)
  {
    Serialwrite(*p);
    p++;
  }
  Serialwrite(0x0d);
}
void Serialbegin()
{
   TMOD=0x20;
   SCON=0x50;
   TH1=0xfd;
   TR1=1;
}
void main()
{
  P2=0x00;
  Serialbegin();
  Serialprintln("System Ready...");
  delay(50);
  while(1)
  {
    while(!RI);
    Charin=SBUF;
    str=Charin;
    RI=0;
      if(str=='1')
      {
        Fan=1;
        Serialprintln(" Fan ON");
        delay(50);
      }
      else if(str=='2')
      {
        Fan=0;
        Serialprintln(" Fan OFF");
        delay(50);
      }
       else if(str=='3')
      {
        Light=1;
        Serialprintln(" Light ON");
        delay(50);
      }
       else if(str=='4')
      {
        Light=0;
        Serialprintln(" Light OFF");
        delay(50);
      }
       else if(str=='5')
      {
        TV=1;
        Serialprintln(" TV ON");
        delay(50);
      }
       else if(str=='6')
      {
        TV=0;
        Serialprintln(" TV OFF");
        delay(50);
      }
      str=0;
  }
}

Circuit digram
![image](https://github.com/user-attachments/assets/bf6ff4e7-ed10-49ee-8dcb-fb77630c72e4)

 
Explanation of diagram 
    Circuit connections of this project are very simple. Bluetooth module’s Rx and Tx pins are directly connected to the  Tx and Rx pins of Microcontroller. Three 5 volt relays are used as a switch for turning On and Off the home appliances running on AC mains. And a relay driver ULN2003 is used for driving relays. Fan, Light and TV are connected at P2.1, P2.2 and P2.3 via relays and relay driver. An 11.0592 MHz Crystal oscillator is used in this circuit for generating clock signal for microcontroller. And a 5 volt voltage regulator LM7805 is used for provide 5 volt for the whole circuit


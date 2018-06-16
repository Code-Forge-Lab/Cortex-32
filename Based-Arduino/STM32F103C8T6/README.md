## Specification ##

- ARM32 Cortex-M3
- 72 Mhz
- 33 I/O pins
- SRAM       : 20KiB  (512b reserved by bootloader)
- Raw Flash  : 64Kib
- Flash      : 64k, if _lucky_ **128KiB** (sector size: 4x1024)
- Option RAM : 16b
- System RAM : 2KiB
- 12-bit(4096) Analog Read , _PWM_. 

# More Details #

Kernel: ARM 32-bit Cortex ™ -M3 CPU
- Up to 72MHz operating frequency
- Single cycle multiplication and hardware division

Memory
- from 64K or 128K bytes of Flash program memory
- up to 20K bytes of SRAM

Clock, reset and power management
- 2.0 ~ 3.6 V power supply and I / O pins
- Power-on / Power-down Reset (POR / PDR), Programmable Voltage Monitor (PVD)
- 4 ~ 16MHz crystal oscillator
- Built-in factory-adjusted 8MHz RC oscillator
- Built-in calibrated 40kHz RC oscillator
- the PLL that generates the CPU clock
- 32kHz RTC oscillator with calibration function

Low power consumption
- sleep, stop and standby modes
- V BAT powers the RTC and back-up registers

2 12-bit analog-to-digital converters, 1us conversion time (up to 16)
- Conversion range: 0 to 3.6V
- Double sampling and hold function
- temperature sensor

DMA:
- 7-channel DMA controller
- Supported peripherals: timers, ADC, SPI, I 2 C and USART

Up to 80 fast I / O ports
- 26/37/51/80 I / O ports, all I / O ports can be mapped to 16 external interrupts; almost all ports can tolerate 5V signals

Debug mode
- Serial single line debug (SWD) and JTAG interface

Up to 7 timers
- 3 16-bit timers, each with up to 4 channels for input capture / output compare / PWM or pulse count and incremental encoder inputs
- 1 16-bit PWM controller with dead-zone control and emergency brake for motor control
- 2 watchdog timers (standalone and window type)
- System time timer: 24-bit self-decrement counter

Up to 9 communication interfaces
- Up to 2 I 2 C interfaces (SMBus / PMBus supported)
- Up to 2 USART interfaces (ISO7816 interface, LIN, IrDA interface and modem control) Up to 2 SPI interfaces (18Mbit / s)
- CAN interface (2.0B active)
- USB 2.0 full speed interface
CRC calculation unit, 96-bit chip unique code

Package included:

   1*STM32F103C8T6 Micro USB controller STM32 Development ARM Learning Board

# Connection #

* Requared: Serial-to-USB-module (3.3V level, e.g. CH340)


* Serial-USB       to       STM32
* GND              ->       GND
* 5v               ->       5v
* RX               ->       PA9
* TX               ->       PA10


![connection to computer](http://grauonline.de/wordpress/wp-content/uploads/arduino_stm32f103c8t6.jpg)
# Analog Digital #
![blue pill information](https://github.com/Code-Forge-Lab/Cortex-32/blob/master/Based-Arduino/STM32F103C8T6/stm32f103c8t-PinOut.gif)

# More Information #

![blue pill information2 ](https://github.com/Code-Forge-Lab/Cortex-32/blob/master/Based-Arduino/STM32F103C8T6/stm32f103c8t6_pinout_voltage01.png)
# Example #

 > **_Blink_**

```c
// Set up led fash and Serial comunication.
#define pinLED PC13

void setup() {
  Serial.begin(9600);
  pinMode(pinLED, OUTPUT);
  Serial.println("START");  
}

void loop() {
  digitalWrite(pinLED, HIGH);
  delay(1000);
  digitalWrite(pinLED, LOW);
  delay(500);
  Serial.println("Hello World");  
}
```




# Example #


> **Serial Send Instruction To Microcontroler , _Read Analog Values_**


```c
// the setup function runs once when you press reset or power the board
void setup() {

    Serial.begin(9600);
    pinMode(PC13, OUTPUT);
    pinMode(PB11, OUTPUT); // output to external led
    pinMode(PB1, INPUT_ANALOG); // read analog
}

 char incomingByte[20] ; // for incoming serial data



// the loop function runs over and over again forever
void loop() {

    

   if (Serial.available()) {
//      give signal when is active         
        digitalWrite(PC13, HIGH);


//     read the incoming byte:
//        incomingByte = Serial.read(); // bug with readBytes as missing first letter if bouth is reading 

//     read whole array of char           //char     , lenght
      Serial.readBytes(incomingByte, 20);

//    // say what you got:
        Serial.print("I received: ");
        Serial.println(incomingByte );
        
    
        
        delay(200);
        digitalWrite(PC13, LOW); 
    }


    // condition what to do if getting any instruction
    if ( strcmp (incomingByte, "alive")== 0  ) 
       Serial.print ("Command 'alive' accepted");
       
    if (strcmp (incomingByte,"responde") == 0 ) // compare char if math
       Serial.print ("Command 'responde' accepted");   
// turn on led
    if (strcmp (incomingByte,"on") == 0 ) 
       {
          digitalWrite(PB11, 4095); // output 3.5v max.
        }

 // turn off led
    if (strcmp (incomingByte,"off") == 0 ) 
       {
          digitalWrite(PB11, LOW);
       }    
     
     clearChar (incomingByte,20); 
    
   
    
   // Capture serial data if even is sleeping
   delay(1000);    


   Serial.println (" input "+String (analogRead (PB1 )) + " "); // only can tolerate 3.4v max.
   Serial.println ("-"); 
     
                 // wait for a second
}

void clearChar ( char *characters , int size ){

         
        for ( int x = 0; x <  size;  x++){
//             Serial.print (", "+String (x) + " -= " + String(characters [x]));
             // remove each character to nill
             characters[x]  = '\0';
          }
  }
``` 
  
 



## Specification ##

- ARM32 Cortex-M3
- 72 Mhz
- 33 I/O pins
- SRAM       : 20KiB  (512b reserved by bootloader)
- Raw Flash  : 64Kib
- Flash      : 128KiB (sector size: 4x1024)
- Option RAM : 16b
- System RAM : 2KiB
- 12-bit(4096) Analog Read , _PWM_. 

# Connection #

* Requared: Serial-to-USB-module (3.3V level, e.g. CH340)


* Serial-USB       to       STM32
* GND              ->       GND
* 5v               ->       5v
* RX               ->       PA9
* TX               ->       PA10


-------------------------------------
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
          digitalWrite(PB11, 4095);
        }

 // turn off led
    if (strcmp (incomingByte,"off") == 0 ) 
       {
          digitalWrite(PB11, LOW);
       }    
     
     clearChar (incomingByte,20); 
    
   
    
   // Capture serial data if even is sleeping
   delay(1000);    


   Serial.println (" input "+String (analogRead (PB1 )) + " ");
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
  
 
## ------------------------------------- ##
pinLED PC13

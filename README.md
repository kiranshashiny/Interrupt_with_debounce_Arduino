# Interrupt_with_debounce_Arduino

This code is a mix of Interrupts and also catching debounce ( which is not in the regular Arduino File Examples )

This code catches any multiple triggers of button press, and thinks of it as just one button push, instead of multiple pushes on the switch.

Typically, a push button kicks in multiple triggers and thereby sounding like multiple interrupts when a push button is pushed and leading to multiple occurences while just one has occured.


The code here is an integration of the Interrupt ( on Pin 2) and also catching multiple triggers /switch bounce when a push button is pressed and received on pin 2 of the Arduino.

Here each time the button is pushed, a counter is incremented by 10. Two pushes means the counter starts ticking downwards from 20 down to 0 and exits counting.

This code was developed for kitchen timer.


The delay in the loop is to track every second.

The delay in the interrupt function blink() is to track push button interrupts and if one occurs in less than 200 ms , then think of it as just 1.

The main focus of this code is in the interrupt handler function blink(), which is called when I press the push button.

Here a flag is set ``` int flag ``` , which is used in the loop() function to start ticking the time down counting once every second.

Note : ignore the part of the code where the LED is turned on or off.




```

const byte ledPin = 13;
const byte interruptPin = 2;
volatile byte state = LOW;
int counter = 0;
int flag = 0;
unsigned long previousMillis = 0;        // will store last time LED was updated

// constants won't change :
const long interval = 1000;    
int first_time = 1;

void setup() {
  pinMode(ledPin, OUTPUT);
  pinMode(interruptPin, INPUT_PULLUP);
  attachInterrupt(digitalPinToInterrupt(interruptPin), blink, CHANGE);
  Serial.begin (9600);
  if ( first_time ) {
     Serial.println ("hello ");
     Serial.println (counter);
     first_time=0;
     counter=0;
  }
}

void loop() {
  digitalWrite(ledPin, state);
  if (flag) {
    unsigned long currentMillis = millis();
    if (currentMillis - previousMillis >= interval) {
      // save the last time you blinked the LED
      previousMillis = currentMillis;
      Serial.println ( counter--);
      if ( counter == 0 ){ 
          flag =0;
          Serial.println ("Exiting, ");
      }
    }
  }
}

void blink() {
  state = !state;
  static unsigned long last_interrupt_time = 0;
  unsigned long interrupt_time = millis();
  // If interrupts come faster than 200ms, assume it's a bounce and ignore
  if (interrupt_time - last_interrupt_time > 200) 
  {
    counter= counter+10;
    flag=1;
  }
  last_interrupt_time = interrupt_time;
}

```


![image](https://github.com/kiranshashiny/Interrupt_with_debounce_Arduino/assets/14288989/beb76022-1062-4a54-bd34-fdf6bcb39f37)



References :

https://www.arduino.cc/reference/en/language/functions/external-interrupts/attachinterrupt/

Uno has only 2 interrupts Pin2 and Pin 3

![image](https://github.com/kiranshashiny/Interrupt_with_debounce_Arduino/assets/14288989/b0de7ce2-ef68-4dba-af3c-b404d6a210c1)

![image](https://github.com/kiranshashiny/Interrupt_with_debounce_Arduino/assets/14288989/91d53824-2070-4d8d-a85b-fc82f17bcfbf)

Some logic I used found in the internet for catching multiple interrupts and think of it as just 1.

![image](https://github.com/kiranshashiny/Interrupt_with_debounce_Arduino/assets/14288989/b75ef690-b9dd-4667-a72a-ef7ac5c19fed)


Parts :

Arduino Uno Board

momentary button or switch

10k ohm resistor

hook-up wires

breadboard

![image](https://github.com/kiranshashiny/Interrupt_with_debounce_Arduino/assets/14288989/cd87cd64-05f3-459a-819c-b26862c2988e)

![image](https://github.com/kiranshashiny/Interrupt_with_debounce_Arduino/assets/14288989/3363acf5-af35-4626-80cc-67e6822c96e7)


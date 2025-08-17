# Dual Arduino Serial LED Control (00, 01, 10, 11) 🔄💡

This project demonstrates **serial communication** between **two Arduino boards** to control **two LEDs** using **two pushbuttons** on the transmitter side.  
Each **button combination** sends a unique binary state (00, 01, 10, 11) to control LED behavior on the receiver side.

---

## 🔧 Project Overview

- **Button States → LED States**:
  - **00** → Both LEDs OFF  
  - **01** → LED2 ON, LED1 OFF  
  - **10** → LED1 ON, LED2 OFF  
  - **11** → Both LEDs ON (simulated in Tinkercad via quick double press detection)  
- Uses **Serial communication** between two Arduino UNO boards.  
- State detection on transmitter, command parsing on receiver.  
- Supports **quick double press** to simulate "both pressed" in Tinkercad.

---
## 📊 Truth Table

| Push1 | Push2 | Binary | Decimal | LED1 | LED2 |
|-------|-------|--------|---------|------|------|
| 0     | 0     | 00     | 0       | OFF  | OFF  |
| 0     | 1     | 01     | 1       | OFF  | ON   |
| 1     | 0     | 10     | 2       | ON   | OFF  |
| 1     | 1     | 11     | 3       | ON   | ON   |

---

## 🧾 Components Required

| Component        | Quantity |
|------------------|----------|
| Arduino UNO      | 2        |
| LEDs             | 2        |
| 220Ω Resistors   | 2        |
| Pushbuttons      | 2        |
| Breadboard       | 1–2      |
| Jumper Wires     | As needed |

---

## 🔗 Tinkercad Simulation
*([Tinkercad link here](https://www.tinkercad.com/things/jDaSM5b5693-dual-arduino-multi-button-serial-led-control?sharecode=J1aUh02R7goNrKJNKjJYbh4EAwQDwyWI2IFTZazfLZ0))*

---

## 🧠 Arduino 1 & Arduino 2 Code
```cpp
//Transmitter code:
int push1 = 13;
int push2 = 12;
int b1state = 0;
int b2state = 0;

unsigned long lastPressTime = 0;
int lastButton = 0; 
unsigned long pressGap = 500; 

void setup() {
  pinMode(push1, INPUT);
  pinMode(push2, INPUT); 
  Serial.begin(9600);
}

void loop() {
  b1state = digitalRead(push1);
  b2state = digitalRead(push2);
  int state = 0;
  // quick double press (one button then the other)
  if (b1state == HIGH && lastButton == 2 && millis() - lastPressTime <= pressGap) {
    state = 3; 
  }
  else if (b2state == HIGH && lastButton == 1 && millis() - lastPressTime <= pressGap) {
    state = 3; 
  }
  else if (b1state == HIGH) {
    state = 2;
    lastButton = 1;
    lastPressTime = millis();
  }
  else if (b2state == HIGH) {
    state = 1;
    lastButton = 2;
    lastPressTime = millis();
  }
  else {
    state = 0;
  }

  Serial.println(state);
  delay(100);
}

// Receiver code:
int led1 = 7;
int led2 = 8;
int state = 0;

void setup() {
  pinMode(led1, OUTPUT);
  pinMode(led2, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  if (Serial.available() > 0) {
    state = Serial.parseInt();

   switch (state) {
      case 0: 
        digitalWrite(led1, LOW);
        digitalWrite(led2, LOW);
        break;
      case 1: 
        digitalWrite(led1, LOW);
        digitalWrite(led2, HIGH);
        break;
      case 2: 
        digitalWrite(led1, HIGH);
        digitalWrite(led2, LOW);
        break;
      case 3: // Both ON
        digitalWrite(led1, HIGH);
        digitalWrite(led2, HIGH);
        break;
    }
  }
  delay(100);
}







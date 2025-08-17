# RGB LED Control by press duration 🎨🔘

This project demonstrates how to control an **RGB LED** using just a **single pushbutton** — detecting **short press**, **long press**, and **double press** events with Arduino.  
Different presses light up different colors.

---

## 🔧 Project Overview

- **Short Press** → Turns **Red** LED ON  
- **Long Press** (> 500ms) → Turns **Green** LED ON  
- **Double Short Press** → Turns **Blue** LED ON  
- Button press timing is detected using `millis()` (no blocking delays)  
- Serial Monitor displays press details and actions

---

## 🧾 Components Required

| Component        | Quantity |
|------------------|----------|
| Arduino UNO      | 1        |
| RGB LED (Common Cathode/Anode) | 1 |
| 220Ω Resistors   | 3        |
| Pushbutton       | 1        |
| Breadboard       | 1        |
| Jumper Wires     | As needed |

---

## 🔗 Tinkercad Simulation
*(https://www.tinkercad.com/things/6LFSl2TEm3A-rgb-control-with-press-duration?sharecode=MMjhzE9i8hTeE2BPRCLl8vyZt19CypjFA0ffPKa4Vq8)*

---

## 🧠 Arduino Code

```cpp
int buttonPin = 3;     
int redPin    = 9;     
int greenPin  = 10;    
int bluePin   = 11;    

unsigned long pressStartTime = 0;
unsigned long lastPressTime  = 0;
int pressCount = 0;
bool buttonState     = LOW;
bool lastButtonState = LOW;

const unsigned long pressThreshold  = 500; // ms
const unsigned long doublePressGap  = 400; // ms

void setup() {
  pinMode(buttonPin, INPUT);
  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);
  
  Serial.begin(9600);
  delay(200);
  Serial.println("System Ready: Waiting for button press...");
}

void allOff() {
  digitalWrite(redPin, LOW);
  digitalWrite(greenPin, LOW);
  digitalWrite(bluePin, LOW);
}

void loop() {
  buttonState = digitalRead(buttonPin);

  if (buttonState == HIGH && lastButtonState == LOW) {
    pressStartTime = millis();
    Serial.println("Button Pressed");
  }

  if (buttonState == LOW && lastButtonState == HIGH) {
    unsigned long pressDuration = millis() - pressStartTime;
    Serial.print("Button Released. Duration: ");
    Serial.print(pressDuration);
    Serial.println(" ms");

    if (pressDuration > pressThreshold) {
      Serial.println("Detected: Long Press : GREEN LED");
      allOff();
      digitalWrite(greenPin, HIGH);
      pressCount = 0;
    } else {
      Serial.println("Detected: Short Press");
      pressCount++;
      lastPressTime = millis();
    }
  }

  if (pressCount > 0 && (millis() - lastPressTime > doublePressGap)) {
    if (pressCount == 1) {
      Serial.println("Action: Single Short Press : RED LED");
      allOff();
      digitalWrite(redPin, HIGH);
    } else if (pressCount == 2) {
      Serial.println("Action: Double Short Press : BLUE LED");
      allOff();
      digitalWrite(bluePin, HIGH);
    }
    pressCount = 0;
  }

  lastButtonState = buttonState;
}


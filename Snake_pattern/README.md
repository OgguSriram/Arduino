# Snake Pattern LED Blinker 🐍💡

This project demonstrates a snake-like LED blinking pattern using an Arduino. LEDs blink in a forward sequence, followed by a reverse sequence, creating a visually engaging flowing effect.

---

## 🔧 Project Overview

- Lights up a series of LEDs in a sequence (forward and backward)
- Simulates a "snake-like" motion
- Great for learning about arrays, loops, and timing in Arduino

---

## 🧾 Components Required

| Component        | Quantity |
|------------------|----------|
| Arduino UNO      | 1        |
| LEDs             | 6        |
| 1kΩ Resistors    | 6        |
| Breadboard       | 1        |
| Jumper Wires     | As needed |

---

## 🔗 Tinkercad Simulation

[🔗 Click here to open the Tinkercad simulation](https://www.tinkercad.com/things/2sbGfTPynfJ-snake-pattern?sharecode=-FLjZsSrmNw0s8cPDhHI7mw-hyTD6Lch5zJ_zAs1PXY)


---

## 🧠 Arduino Code

```cpp
int leds[] = {13, 11, 9, 8, 7, 4};
int ledCount = 6;

void setup() {
  for (int i = 0; i < ledCount; i++) {
    pinMode(leds[i], OUTPUT);
  }
}

void loop() {
  // Forward direction
  for (int i = 0; i < ledCount - 1; i++) {
    digitalWrite(leds[i], HIGH);
    digitalWrite(leds[i + 1], HIGH);
    delay(200);
    digitalWrite(leds[i], LOW);
  }

  // Backward direction
  for (int i = ledCount - 1; i > 0; i--) {
    digitalWrite(leds[i], HIGH);
    digitalWrite(leds[i - 1], HIGH);
    delay(200);
    digitalWrite(leds[i], LOW);
  }
}

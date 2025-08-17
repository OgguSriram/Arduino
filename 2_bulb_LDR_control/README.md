# LDR-Based Dual Bulb Control Using Arduino

This project demonstrates a **light-dependent dual bulb control system** using an **LDR sensor**, **Arduino Uno**, and **NPN transistors**. One bulb functions as a digital switch (ON/OFF), while the second bulb is controlled via PWM for brightness adjustment based on ambient light.

---

## 🛠️ Hardware Components

- Arduino Uno
- LDR (Light Dependent Resistor)
- 2x NPN Transistors 
- 2x 5V DC Bulbs
- 4x 1kΩ Resistors
- Breadboard & Jumper Wires

---

## ⚙️ Circuit Overview

### Functionality:

- **LDR** reads ambient light level.
- **Bulb 1 (L1)**: Controlled by a digital pin through a transistor. Turns ON when it’s dark and OFF when it's bright.
- **Bulb 2 (L2)**: Brightness is controlled using **PWM output**, varying according to light intensity (brighter in dark).

---

## 🔗 Tinkercad Simulation
 (https://www.tinkercad.com/things/0zcX8n0nIIC-2-x-bulbs-ldr?sharecode=IGjJ2q-GUOQTwJ9SuqnpdCVEZV9bG2RnrNjh1m_HoWM)


---

## 🧠 Code Explanation

```cpp
int ldrPin = A0;         
int ledPin = 8;          
int bulbPWM = 9;        
int threshold = 500;     

void setup() {
  pinMode(ledPin, OUTPUT);
  pinMode(bulbPWM, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int ldrValue = analogRead(ldrPin);
  Serial.print("LDR Value: ");
  Serial.println(ldrValue);

  // Digital control: ON/OFF
  if (ldrValue < threshold) {
    digitalWrite(ledPin, HIGH);  // Turn ON when dark
  } else {
    digitalWrite(ledPin, LOW);   // Turn OFF when bright
  }

  // PWM control: Brightness adjustment
  int brightness = map(ldrValue, 0, 1023, 255, 0);  // Inverse mapping
  analogWrite(bulbPWM, brightness);

  delay(500);  // Delay to reduce serial flooding
}

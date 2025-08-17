# Temperature Display on LCD with Pushbutton 🌡️🔘

This project reads temperature values from a sensor connected to Arduino, and displays the readings in **Celsius** and **Fahrenheit** on a **16x2 LCD**.  
The LCD only updates and shows the temperature when the **pushbutton** is pressed.

---

## 🔧 Project Overview

- Uses **Analog Temperature Sensor** connected to Arduino  
- Reads sensor values and converts them to **Celsius** and **Fahrenheit**  
- Displays results on **16x2 LCD (I2C)**  
- Pushbutton acts as a **trigger** to display values  
- LCD clears after each reading to keep it clean  

---

## 🧾 Components Required

| Component              | Quantity |
|------------------------|----------|
| Arduino UNO            | 1        |
| 16x2 LCD (I2C)         | 1        |
| Analog Temperature Sensor (e.g., LM35) | 1 |
| Pushbutton             | 1        |
| 10kΩ Resistor (for button pull-down) | 1 |
| Breadboard             | 1        |
| Jumper Wires           | As needed |

---

## 🔗 Tinkercad Simulation
*( https://www.tinkercad.com/things/a7j0t5zKjMJ-i2c-lcd?sharecode=8O9kYGy1Yexvb19AGFKen0-ZZaRaWlS6SQJn8gM9XnA)*

---

## 🧠 Arduino Code

```cpp
#include <Adafruit_LiquidCrystal.h>
int push = 2;
Adafruit_LiquidCrystal lcd(0);

void setup()
{
  lcd.begin(16, 2);
  pinMode(push, INPUT);
}

void loop()
{
  int pushstate = digitalRead(push);
  if (pushstate == 1) {
    float temp = analogRead(A0);
    float realtemp = ((temp * (5.0 / 1023.0)) - 0.50) * 100;

    lcd.setCursor(0, 0);
    lcd.print(realtemp);
    lcd.print(" C");

    // Convert to Fahrenheit
    float faren = (((9.0 / 5.0) * realtemp) + 32.0);
    lcd.setCursor(0, 1);
    lcd.print(faren);
    lcd.print(" F");

    delay(1000);
    lcd.clear();
  }
}

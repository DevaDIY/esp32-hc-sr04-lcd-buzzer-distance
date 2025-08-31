# ESP32 Distance Meter with HC-SR04 + LCD I2C + Buzzer

โปรเจกต์นี้สาธิตการสร้าง **เครื่องวัดระยะทาง (Ultrasonic Distance Meter)** โดยใช้บอร์ด **ESP32** ร่วมกับ **HC-SR04**, **LCD I2C (16×2)** และ **Buzzer** เพื่อแสดงผลระยะทางแบบเรียลไทม์และแจ้งเตือนเมื่อวัตถุอยู่ในระยะใกล้

---

## ✨ Features
- วัดระยะทางด้วย **HC-SR04 Ultrasonic Sensor**
- แสดงผลค่าระยะทางบน **LCD I2C (16x2)**
- มีเสียง **Buzzer** เตือนเมื่อระยะน้อยกว่า 15 cm
- ใช้ **Arduino IDE** และไลบรารี `LiquidCrystal_I2C`

---

## 🛠️ Hardware Required
- ESP32 DevKit (เช่น ESP32-DevKitV1)
- HC-SR04 Ultrasonic Sensor
- LCD I2C (16×2)
- Active Buzzer
- Breadboard + Jumper Wires

---

## ⚡ Wiring Diagram
| Device        | Pin on ESP32 |
|---------------|--------------|
| HC-SR04 Trig  | GPIO 18      |
| HC-SR04 Echo  | GPIO 19      |
| Buzzer        | GPIO 5       |
| LCD SDA       | GPIO 21      |
| LCD SCL       | GPIO 22      |
| VCC & GND     | 5V, GND      |

---

## อ่านบทความเต็ม

👉 [สร้างเครื่องวัดระยะด้วย ESP32 + HC-SR04 แสดงผล LCD พร้อมเสียงเตือน](https://devadiy.com/esp32-hc-sr04-lcd-buzzer-distance/)

👉 [รวมโครงงาน ESP32 และ Smart Farm](https://devadiy.com/)

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

const int trigPin = 18;
const int echoPin = 19;
const int buzzerPin = 5;

LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(buzzerPin, OUTPUT);

  lcd.init();
  lcd.backlight();
  lcd.print("Ultrasonic Dist");
  Serial.begin(115200);
}

void loop() {
  long duration;
  float distance;

  digitalWrite(trigPin, LOW); delayMicroseconds(5);
  digitalWrite(trigPin, HIGH); delayMicroseconds(5);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = (duration / 2.0) / 29.1;

  lcd.setCursor(0, 1);
  lcd.print("DT: ");
  lcd.print(distance);
  lcd.print(" cm ");

  if (distance < 7) { digitalWrite(buzzerPin, HIGH); delay(50); digitalWrite(buzzerPin, LOW); delay(50); }
  else if (distance < 15) { digitalWrite(buzzerPin, HIGH); delay(200); digitalWrite(buzzerPin, LOW); delay(200); }
  else { digitalWrite(buzzerPin, LOW); delay(100); }
}

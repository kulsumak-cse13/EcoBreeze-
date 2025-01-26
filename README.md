#include <LiquidCrystal.h>

// LCD পিন সংযোগ
LiquidCrystal lcd(7, 8, 9, 10, 11, 12);

// পিন ডেফিনেশন
int airSensor = A0; // MQ135 সেন্সর
int redLED = 3, yellowLED = 4, greenLED = 5; // LED পিন
int relayPin = 6; // রিলে পিন

void setup() {
  lcd.begin(16, 2);
  lcd.print("EcoBreeze Ready!");
  delay(2000);
  lcd.clear();

  pinMode(airSensor, INPUT);
  pinMode(redLED, OUTPUT);
  pinMode(yellowLED, OUTPUT);
  pinMode(greenLED, OUTPUT);
  pinMode(relayPin, OUTPUT);
  digitalWrite(relayPin, LOW);
}

void loop() {
  int airQuality = analogRead(airSensor); // সেন্সরের ডেটা পড়া
  
  // LCD-তে দূষণের মান প্রদর্শন
  lcd.setCursor(0, 0);
  lcd.print("Air Quality:");
  lcd.setCursor(0, 1);
  lcd.print(airQuality);

  // দূষণের মান অনুযায়ী LED এবং ফ্যান নিয়ন্ত্রণ
  if (airQuality > 300) { // বেশি দূষণ
    digitalWrite(redLED, HIGH);
    digitalWrite(yellowLED, LOW);
    digitalWrite(greenLED, LOW);
    digitalWrite(relayPin, HIGH); // ফ্যান চালু
  } else if (airQuality > 150) { // মাঝারি দূষণ
    digitalWrite(redLED, LOW);
    digitalWrite(yellowLED, HIGH);
    digitalWrite(greenLED, LOW);
    digitalWrite(relayPin, HIGH); // ফ্যান চালু
  } else { // বিশুদ্ধ বাতাস
    digitalWrite(redLED, LOW);
    digitalWrite(yellowLED, LOW);
    digitalWrite(greenLED, HIGH);
    digitalWrite(relayPin, LOW); // ফ্যান বন্ধ
  }

  delay(1000); // প্রতি সেকেন্ডে আপডেট
}


#include <Wire.h>
#include <LiquidCrystal_I2C.h>

// I2C LCD at address 0x27 with 16 columns and 2 rows
LiquidCrystal_I2C lcd(0x27, 16, 2);

// Time variables
int hourVal = 15;
int minuteVal = 45;
int secondVal = 0;
int day = 23;
int month = 6;
int year = 2025;

unsigned long prevMillis = 0;

void setup() {
  lcd.begin();
  lcd.backlight();
}

void loop() {
  // Simulate time using millis()
  unsigned long currentMillis = millis();
  if (currentMillis - prevMillis >= 1000) {
    prevMillis = currentMillis;
    secondVal++;
    if (secondVal >= 60) {
      secondVal = 0;
      minuteVal++;
    }
    if (minuteVal >= 60) {
      minuteVal = 0;
      hourVal++;
    }
    if (hourVal >= 24) {
      hourVal = 0;
      // You can add day increment logic here if needed
    }
  }

  // Display time on LCD
  lcd.setCursor(0, 0);
  lcd.print("Time: ");
  print2digit(hourVal);
  lcd.print(":");
  print2digit(minuteVal);
  lcd.print(":");
  print2digit(secondVal);

  // Display date on LCD
  lcd.setCursor(0, 1);
  lcd.print("Date: ");
  lcd.print(day);
  lcd.print("/");
  lcd.print(month);
  lcd.print("/");
  lcd.print(year);

  delay(100); // slight delay for stability
}
// Helper to print 2-digit numbers
void print2digit(int value) {
  if (value < 10) lcd.print("0");
  lcd.print(value);
}

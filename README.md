# digital-nootice-board-
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <SoftwareSerial.h>

#define LCD_ADDR 0x27  // Change to 0x3F if your LCD uses a different address
LiquidCrystal_I2C lcd(LCD_ADDR, 16, 2); // 16x2 LCD
SoftwareSerial BT(0, 1); // RX, TX

void setup() {
    lcd.init();
    lcd.backlight(); // Turn on backlight
    lcd.setCursor(0, 0);
    lcd.print("Waiting");
    
    BT.begin(9600); // Bluetooth module baud rate
}

void loop() {
    if (BT.available()) {
        lcd.clear();
        lcd.setCursor(0, 0);
        
        String receivedText = "";
        while (BT.available()) {
            char c = BT.read();
            receivedText += c;
            delay(5); // Small delay to allow proper reception
        }
        
        lcd.print(receivedText);
    }
}

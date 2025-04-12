#include <Wire.h>
#include <LiquidCrystal_I2C.h>

#define BUTTON_PIN 2  // Push button connected to digital pin 2

int count = 0;  // Counter variable
bool lastButtonState = HIGH;  // Stores the last button state
unsigned long lastDebounceTime = 0;  // Stores last debounce time
const int debounceDelay = 50;  // Debounce time in milliseconds

// Initialize LCD with I2C address (replace with detected address)
LiquidCrystal_I2C lcd(0x27, 20, 4);  

void setup() {
    pinMode(BUTTON_PIN, INPUT_PULLUP);  // Enable internal pull-up resistor
    Serial.begin(9600);
    
    // Initialize LCD
    lcd.init();
    lcd.backlight();
    lcd.clear();
    
    lcd.setCursor(0, 0);
    lcd.print("Button Press Counter");
    lcd.setCursor(0, 1);
    lcd.print("Press button...");
}

void loop() {
    bool buttonState = digitalRead(BUTTON_PIN);
    
    Serial.print("Button State: ");
    Serial.println(buttonState);  // Check if button is detected

    if (buttonState == LOW && lastButtonState == HIGH) {  
        if (millis() - lastDebounceTime > debounceDelay) {  
            count++;  // Increment count
            
            // Update LCD
            lcd.clear();
            lcd.setCursor(0, 0);
            lcd.print("Button Count:");
            lcd.setCursor(0, 1);
            lcd.print(count);
            
            // Print to Serial Monitor
            Serial.print("Button Pressed! Count: ");
            Serial.println(count);

            lastDebounceTime = millis();
        }
    }
    
    lastButtonState = buttonState;  // Update last state
}

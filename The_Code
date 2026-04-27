#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <DHT.h>

#define DHT11_PIN   11      
#define DHTTYPE     DHT11

LiquidCrystal_I2C lcd(0x27, 16, 2);
DHT dht(DHT11_PIN, DHTTYPE);

float temperature = 0;
float humidity    = 0;
unsigned long lastDHTRead = 0;
bool lcdOK = false;
bool fastMode = false;        
bool lastButtonState = true;

const byte digits[10] = {
    0b11000000, 0b11111001, 0b10100100, 0b10110000,
    0b10011001, 0b10010010, 0b10000010, 0b11111000,
    0b10000000, 0b10010000
};

#define RED_BIT     0
#define GREEN_BIT   1
#define YELLOW_BIT  2
#define SEC_LED_BIT 4
#define BUTTON_BIT  2       
void showNumber(int num) {
    PORTB = digits[num % 10];
    PORTA = digits[num / 10];
}

void allLedsOff() {
    PORTD &= ~(0b00010111);
}

void checkButton() {
    bool currentState = (PINC >> BUTTON_BIT) & 1;
    if (currentState == false && lastButtonState == true) {
        fastMode = !fastMode;
        delay(200);
    }
    lastButtonState = currentState;
}

int getDelay() {
    return fastMode ? 100 : 500;
}

void updateLCD() {
    if (!lcdOK) return;
    if (millis() - lastDHTRead < 2000) return;
    lastDHTRead = millis();

    float t = dht.readTemperature();
    float h = dht.readHumidity();

    if (isnan(t) || isnan(h)) {
        lcd.setCursor(0, 0);
        lcd.print("Sensor Failed   ");
        lcd.setCursor(0, 1);
        lcd.print("                ");
        return;
    }

    temperature = t;
    humidity    = h;

    lcd.setCursor(0, 0);
    lcd.print("Temp: ");
    lcd.print(temperature, 1);
    lcd.print((char)223);
    lcd.print("C   ");

    lcd.setCursor(0, 1);
    lcd.print("Humi: ");
    lcd.print(humidity, 1);
    lcd.print("%   ");
}

void setup() {
    DDRA  = 0xFF;
    DDRB  = 0xFF;
    DDRD |= 0b00010111;

    DDRC  &= ~(1 << BUTTON_BIT);
    DDRC  &= ~(1 << 5);
    PORTC |=  (1 << BUTTON_BIT);

    allLedsOff();

    Wire.begin();
    Wire.beginTransmission(0x27);
    if (Wire.endTransmission() == 0) {
        lcdOK = true;
        lcd.begin();
        lcd.backlight();
    }

    dht.begin();
    delay(2000);
}

void loop() {

    allLedsOff();
    PORTD |= (1 << GREEN_BIT);

    for (int i = 40; i >= 0; i--) {
        checkButton();
        if (i <= 4 && i > 0) {
            PORTD &= ~(1 << GREEN_BIT);
            PORTD |=  (1 << YELLOW_BIT);
        } else if (i == 0) {
            PORTD &= ~(1 << GREEN_BIT);
            PORTD &= ~(1 << YELLOW_BIT);
        } else {
            PORTD &= ~(1 << YELLOW_BIT);
            PORTD |=  (1 << GREEN_BIT);
        }
        showNumber(i);
        updateLCD();
        delay(getDelay());
    }

    allLedsOff();
    PORTD |= (1 << RED_BIT);

    for (int i = 40; i >= 0; i--) {
        checkButton();
        if (i <= 4 && i > 0) {
            PORTD &= ~(1 << RED_BIT);
            PORTD &= ~(1 << YELLOW_BIT);
            PORTD |=  (1 << SEC_LED_BIT);
        } else if (i == 0) {
            PORTD &= ~(1 << RED_BIT);
            PORTD &= ~(1 << YELLOW_BIT);
            PORTD &= ~(1 << SEC_LED_BIT);
        } else {
            PORTD &= ~(1 << YELLOW_BIT);
            PORTD &= ~(1 << SEC_LED_BIT);
            PORTD |=  (1 << RED_BIT);
        }
        showNumber(i);
        updateLCD();
        delay(getDelay());
    }
}

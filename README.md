# Temperature-display-for-espresso-machine
Arduino code for adding a thermocouple with display to an espresso machine.

#include <SPI.h>
#include "Adafruit_MAX31855.h"
#include <LiquidCrystal.h>

int thermoCLK = 3;
int thermoCS = 4;
int thermoDO = 5;
//lcd 11 is digital pin 10
//lcd 12 is dig pin 9
//lcd 13 is dig pin 8
//lcd 14 is dig pin 7

Adafruit_MAX31855 thermocouple(thermoCLK, thermoCS, thermoDO);
LiquidCrystal lcd(12, 11, 10, 9, 8, 7);

void setup() {
  Serial.begin(9600); //for sending stuff to serial monitor
  lcd.begin(16, 2);
  lcd.print("MAX31855 test");
  // wait for MAX chip to stabilize
  delay(500);
}

void loop() {
//for troubleshooting/reading pot values 
//  int sensorValue = analogRead(A0);
//  Serial.println(sensorValue);
//  delay(1);
  
  //LCD stuff
  lcd.setCursor(0, 0);
  lcd.print(" broke  "); //half of the screen is dead...
  //lcd.println(thermocouple.readInternal());
  lcd.print(millis()/3600000); //hours
  lcd.print(":");
  lcd.print(millis()/60000); //minutes
  lcd.print(":");
  //seconds = millis/1000;
  //if(seconds == 60, seconds = 0);
  lcd.print(millis()/1000); //seconds
  lcd.print("  ");
  double c = thermocouple.readFarenheit();
   lcd.setCursor(0, 1);
   if (isnan(c)) 
   {
     lcd.print("T/C Problem");
   } 
   else 
   {
     lcd.print(" broke  F= "); 
     lcd.print(c);
     lcd.print("  "); 
   }
  delay(1000);

}

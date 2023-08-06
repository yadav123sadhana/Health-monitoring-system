# Health-monitoring-system
#include <LiquidCrystal.h>

LiquidCrystal lcd(12, 11, 5, 4, 3, 2);
const float thi=179;
const float tlo=175;
const int hbhi=100;
const int hblo=60;

const int bphi=120;
const int bplo=90;
 
const int BUZZER_PIN = 6; 
const int DISTANCE_THRESHOLD = 50; 
float duration_us, distance_cm;

void setup() {
  
  pinMode(A0,INPUT);
  pinMode(A1,INPUT);
  pinMode(7,OUTPUT);
  pinMode(8,OUTPUT);
  pinMode(10, OUTPUT);
  pinMode(9, INPUT);
  pinMode(13, OUTPUT);
  pinMode(BUZZER_PIN, OUTPUT);
  Serial.begin(9600);
  lcd.begin(16, 2);
  
  lcd.print("T hb dist  bp ");
}

void loop() {
 
  int SensorVal=analogRead(A0);
  int hbeat=analogRead(A1);
  int bp=analogRead(A2);
  
  float voltage = (SensorVal/1023.0) * 5.0;
  float temperatureC = (voltage - .5) * 100;
  
  digitalWrite(10, HIGH);
  delayMicroseconds(10);
  digitalWrite(10, LOW);
  duration_us = pulseIn(9, HIGH);
   distance_cm = 0.017 * duration_us;
  
  if (distance_cm < DISTANCE_THRESHOLD)  // distance
   {
    lcd.setCursor(4, 2);
    digitalWrite(BUZZER_PIN, HIGH);
    Serial.print("\nKeep distance\n");
    Serial.print("distance: ");
    Serial.print(distance_cm);
    Serial.println(" cm");
    lcd.print(distance_cm);
   }
  else 
  {
    lcd.setCursor(4, 2);
    digitalWrite(BUZZER_PIN, LOW); 
    Serial.print("distance: ");
    Serial.print(distance_cm);
    Serial.println(" cm");
    lcd.print(distance_cm);
  }

  delay(500);
  
  
   if (SensorVal<=tlo)        //temperature
   {
  
    lcd.setCursor(0, 2);
    digitalWrite(7, HIGH); 
    Serial.print("Low temp ");
    Serial.println(temperatureC);
    lcd.print("L ");
   }
   else if (SensorVal>=thi)
  {
   lcd.setCursor(0, 2);
   digitalWrite(7, HIGH);
   Serial.print("High Temp "); 
   Serial.println(temperatureC);
   lcd.print("H "); 
  } 
  else
  {
   lcd.setCursor(0, 2); 
   digitalWrite(7,LOW);
   Serial.print(" Normal Temp "); 
   Serial.println(temperatureC);
   lcd.print("N "); 
  }
  
  if (hbeat<hblo)     //heart beat
   {
  
    lcd.setCursor(2, 2);
    digitalWrite(8, HIGH); 
    Serial.print("Low Beat ");
    Serial.println(hbeat);
    lcd.print("L ");
   }
   else if (hbeat>hbhi)
  {
   lcd.setCursor(2, 2);
   digitalWrite(8, HIGH);
   Serial.print("High Beat "); 
   Serial.println(hbeat);
   lcd.print("H "); 
  } 
  else
  {
   lcd.setCursor(2, 2); 
   digitalWrite(8,LOW);
   Serial.print(" Normal Beat "); 
   Serial.println(hbeat);
   lcd.print("N "); 
  }
  
  if (bp<bplo)    //Bloodpressure
   {
  
    lcd.setCursor(11, 2);
    digitalWrite(13, HIGH); 
    Serial.print("Low BP ");
    Serial.println(bp);
    lcd.print("L ");
   }
   else if (bp>bphi)
  {
   lcd.setCursor(11, 2);
   digitalWrite(13, HIGH);
   Serial.print("High BP "); 
   Serial.println(bp);
   lcd.print("H "); 
  } 
  else
  {
   lcd.setCursor(11, 2); 
   digitalWrite(13,LOW);
   Serial.print("Normal BP "); 
   Serial.println(bp);
   lcd.print("N "); 
  }

}
 

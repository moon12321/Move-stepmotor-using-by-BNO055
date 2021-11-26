#include <Wire.h>
#include <Adafruit_Sensor.h>
#include <Adafruit_BNO055.h>
#include <utility/imumaths.h>
#include <SoftwareSerial.h>
  
Adafruit_BNO055 bno = Adafruit_BNO055(55);

int Index;
/* SoftwareSerial espSerial(2, 3); // RX, TX
String str; */

void setup(void) 
{
  Serial.begin(9600);
  delay(9100); // 초기 딜레이 시간
  Serial.println("Orientation Sensor Test"); Serial.println("");
  pinMode(10, OUTPUT);//모터 A
  pinMode(9, OUTPUT); //모터 A
  pinMode(8, OUTPUT); //모터 A
  pinMode(6, OUTPUT); //모터 B
  pinMode(5, OUTPUT); //모터 B
  pinMode(4, OUTPUT); //모터 B


  digitalWrite(10,LOW);
  digitalWrite(6,LOW);
  
  /* Initialise the sensor */
  if(!bno.begin())
  {
    /* There was a problem detecting the BNO055 ... check your connections */
    Serial.print("Ooops, no BNO055 detected ... Check your wiring or I2C ADDR!");
    while(1);
  }
  
  delay(1000);
    
  bno.setExtCrystalUse(true);

  digitalWrite(8,HIGH);
  digitalWrite(4,HIGH);
  
  for(Index = 0; Index < 1220; Index++) //1220 세바퀴
  {
    digitalWrite(9,HIGH);
    digitalWrite(5,HIGH);
    delayMicroseconds(1000);
    digitalWrite(9,LOW);
    digitalWrite(5,LOW);
    delayMicroseconds(1000);
  }
  
}

void loop(void) 
{
  /* Get a new sensor event */ 
  sensors_event_t event; 
  bno.getEvent(&event);

  /* Display the floating point data */
  Serial.print("X: ");
  Serial.print(event.orientation.x, 4);
  Serial.print("\tY: ");
  Serial.print(event.orientation.y, 4);
  Serial.print("\tZ: ");
  Serial.print(event.orientation.z, 4);
  Serial.println("");
  
  if(event.orientation.y>2&&event.orientation.y <52) {

    if(event.orientation.z>167) {
      
      digitalWrite(8,HIGH); //왼쪽
      digitalWrite(4,LOW);

      for(Index = 0; Index < 900; Index++) 
      {
        digitalWrite(9,HIGH);
        digitalWrite(5,HIGH);
        delayMicroseconds(500);
        digitalWrite(9,LOW);
        digitalWrite(5,LOW);
        delayMicroseconds(500);
      }
    
      digitalWrite(8,LOW);
      digitalWrite(4,HIGH);
    
      for(Index = 0; Index < 900; Index++) 
      {
        digitalWrite(9,HIGH);
        digitalWrite(5,HIGH);
        delayMicroseconds(500);
        digitalWrite(9,LOW);
        digitalWrite(5,LOW);
        delayMicroseconds(500);
      }

      
    }

    else if(event.orientation.z<11.75){

       digitalWrite(4,HIGH); //오른쪽
       digitalWrite(8,LOW);

       for(Index = 0; Index < 900; Index++) 
       { 
          digitalWrite(5,HIGH);
          digitalWrite(9,HIGH);
          delayMicroseconds(500);
          digitalWrite(5,LOW);
          digitalWrite(9,LOW);
          delayMicroseconds(500);
       }
    
       digitalWrite(4,LOW);
       digitalWrite(8,HIGH);
    
       for(Index = 0; Index < 900; Index++) 
       {
          digitalWrite(5,HIGH);
          digitalWrite(9,HIGH);
          delayMicroseconds(500);
          digitalWrite(5,LOW);
          digitalWrite(9,LOW);
          delayMicroseconds(500);
       }
     }
      
  }
}

# smart-parking-
#include <Servo.h> //includes the servo library
#include <Wire.h> 
const int led_r0=8,led_g0=9,led_r1=10,led_r2=11,led_r3=12,led_r4=13,led_g1=14,led_g2=15,led_g3=16,led_g4=17; 
Servo myservo;
 
#define ir_enter 1
#define ir_back 2
 
#define ir_car1 3
#define ir_car2 4
#define ir_car3 5
#define ir_car4 6
 
int S1=0, S2=0, S3=0, S4=0;
int flag1=0, flag2=0; 
int slot = 4;  
 
void setup(){
Serial.begin(9600);
// initialize digital pins as input.
pinMode(led_g0,  OUTPUT);
pinMode(led_g1,  OUTPUT);
pinMode(led_g2,  OUTPUT);
pinMode(led_g3,  OUTPUT);
pinMode(led_g4,  OUTPUT);
pinMode(led_r0,  OUTPUT);
pinMode(led_r1,  OUTPUT);
pinMode(led_r2,  OUTPUT);
pinMode(led_r3,  OUTPUT);
pinMode(led_r4,  OUTPUT);
pinMode(ir_car1, INPUT);
pinMode(ir_car2, INPUT);
pinMode(ir_car3, INPUT);
pinMode(ir_car4, INPUT);
 
pinMode(ir_enter, INPUT);
pinMode(ir_back, INPUT);
  
myservo.attach(9); // Servo motor pin connected to D9
myservo.write(90); // sets the servo at 0 degree position
 
Read_Sensor();
 
int total = S1+S2+S3+S4;
slot = slot-total; 
}
 
void loop()
{
 
 Read_Sensor();
 
  if(S1==1)
   {
    digitalWrite(led_r1,HIGH);
    digitalWrite(led_g1,LOW);
   }
  else
   {
    digitalWrite(led_r1,LOW);
    digitalWrite(led_g1,HIGH);
   }
 
  if(S2==1)
   {
    digitalWrite(led_r2,HIGH);
    digitalWrite(led_g2,LOW);
    }
  else
   {
    digitalWrite(led_r2,LOW);
    digitalWrite(led_g2,HIGH);
    }
 
  if(S3==1)
   {
    digitalWrite(led_r3,HIGH);
    digitalWrite(led_g3,LOW);
    }
  else
   {
    digitalWrite(led_r3,LOW);
    digitalWrite(led_g3,HIGH);
    }
 
  if(S4==1)
   {
    digitalWrite(led_r4,HIGH);
    digitalWrite(led_g4,LOW);
    }
  else
   {
    digitalWrite(led_r4,LOW);
    digitalWrite(led_g4,HIGH);
    }
    
// Servo Motor Control
  if(digitalRead (ir_enter) == 0 && flag1==0) 
   {
    if(slot>0)
     {
      flag1=1;
      if(flag2==0)
       {
        myservo.write(180); 
        slot = slot-1;
        }
     }
    else
     {
    digitalWrite(led_r0,HIGH);
    digitalWrite(led_g0,LOW);
      }   
   }
 
  if(digitalRead (ir_back) == 0 && flag2==0)
   {
    flag2=1;
    if(flag1==0)
     {
      myservo.write(180); // sets the servo at 180 degree position
      slot = slot+1;
      }
   }
 
  if(flag1==1 && flag2==1)
   {
    delay (1000);
    myservo.write(90); // sets the servo at 90 degree position
    flag1=0, flag2=0;
    }
    delay(1);
}
 
void Read_Sensor()
{
 S1=0, S2=0, S3=0, S4=0;
 if(digitalRead(ir_car1) == 0){S1=1;} 
 if(digitalRead(ir_car2) == 0){S2=1;} 
 if(digitalRead(ir_car3) == 0){S3=1;} 
 if(digitalRead(ir_car4) == 0){S4=1;} 
}

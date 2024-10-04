// #include <MsTimer2.h>
//#include "BuvInit.h"
#include "avr_can.h"

bool wasSleeping = false;
bool ignitionState = false;
bool lightState = false;
bool reverseState = false;
float battery = 0.0;

void gotFrame(CAN_FRAME *frame) 
{
  if (frame->id == 0x271) {
    ignitionState = (frame->data.bytes[0] & 0x01);
    Serial.println("KEY: " + String(ignitionState));
    digitalWrite(PIN_PA0, ~ignitionState);
  } else if (frame->id == 0x351) {
    reverseState = (frame->data.bytes[0] & 0x02);
    Serial.println("REVERSE: " + String(reverseState));
    digitalWrite(PIN_PA1, ~reverseState);
  } else if (frame->id == 0x635) {
    lightState = (frame->data.bytes[0] & 0x5C);
    Serial.println("LIGHT: " + String(lightState));
    digitalWrite(PIN_PA2, ~lightState);
  } else if (frame->id == 0x571) {
    battery = (frame->data.bytes[0] * 0.05) + 5;
    Serial.println("BATTERY: " + String(battery) + "V");
  }
  digitalWrite(PIN_PE3, ~digitalRead(PIN_PE3));
}

void setup()
{

  Serial.begin(115200);
  
  pinMode(PIN_PA0, OUTPUT);
  pinMode(PIN_PA1, OUTPUT);
  pinMode(PIN_PA2, OUTPUT);
  digitalWrite(PIN_PA0, HIGH);
  digitalWrite(PIN_PA1, HIGH);
  digitalWrite(PIN_PA2, HIGH);
  
  pinMode(PIN_PE3, OUTPUT);
  digitalWrite(PIN_PE3, HIGH);

  pinMode(PIN_PB7, OUTPUT);
  digitalWrite(PIN_PB7, LOW);

  Can0.begin(CAN_BPS_125K);
  Serial.println("Can Begin"); 
  Can0.setNumTXBoxes(0);

  Can0.setGeneralCallback(gotFrame);
  Serial.println("Can Callback on");  
}

void loop(){

}

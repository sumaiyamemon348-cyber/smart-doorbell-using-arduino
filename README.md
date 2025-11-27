#include <SoftwareSerial.h>

SoftwareSerial BT(10, 11); // Bluetooth: RX=10, TX=11

const int PIR = 2;     // PIR sensor
const int LED = 8;     // LED indicator
const int BUZZER = 9;  // Buzzer

int visitors = 0;
bool detected = false;

void setup() {
  pinMode(PIR, INPUT);
  pinMode(LED, OUTPUT);
  pinMode(BUZZER, OUTPUT);

  Serial.begin(9600);
  BT.begin(9600);

  Serial.println("Smart Doorbell Ready!");
  BT.println("Smart Doorbell Ready!");
}

void loop() {
  bool motion = digitalRead(PIR); // check PIR sensor

  if (motion && !detected) {
    detected = true;
    visitors++;

    // Alert: LED + Buzzer
    digitalWrite(LED, HIGH);
    digitalWrite(BUZZER, HIGH);
    delay(500);
    digitalWrite(LED, LOW);
    digitalWrite(BUZZER, LOW);

    // Send message
    Serial.print("Visitor Detected! Total: ");
    Serial.println(visitors);
    BT.print("Visitor Detected! Total: ");
    BT.println(visitors);
  }

  if (!motion) detected = false; // reset detection
}

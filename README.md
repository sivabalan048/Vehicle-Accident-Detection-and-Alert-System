# Vehicle-Accident-Detection-and-Alert-System
This project detects accidents using an accelerometer and sends SMS alerts with GPS coordinates via GSM module, ensuring timely emergency response.
#include <SoftwareSerial.h>

// Accelerometer analog pins (ADXL335 for example)
#define X_PIN A0
#define Y_PIN A1
#define Z_PIN A2

// GSM module pins
SoftwareSerial gsm(7, 8); // RX, TX (connect GSM TX to pin 7, GSM RX to pin 8)

// Threshold for accident detection (adjust experimentally)
const int threshold = 400; 

void setup() {
  Serial.begin(9600);
  gsm.begin(9600);

  delay(1000);
  Serial.println("System Ready");
}

void loop() {
  int xVal = analogRead(X_PIN);
  int yVal = analogRead(Y_PIN);
  int zVal = analogRead(Z_PIN);

  Serial.print("X: "); Serial.print(xVal);
  Serial.print(" Y: "); Serial.print(yVal);
  Serial.print(" Z: "); Serial.println(zVal);

  // Accident detection logic:
  if (abs(xVal - 512) > threshold || abs(yVal - 512) > threshold || abs(zVal - 512) > threshold) {
    sendSMS();
    delay(10000); // wait 10 seconds before next alert
  }

  delay(500);
}

void sendSMS() {
  Serial.println("Accident detected! Sending SMS...");
  gsm.println("AT+CMGF=1");    // SMS text mode
  delay(500);
  gsm.println("AT+CMGS=\"+911234567890\""); // Replace with emergency number
  delay(500);
  gsm.print("Accident detected! Please check the vehicle location.");
  gsm.write(26); // Ctrl+Z to send
  delay(5000);
  Serial.println("SMS Sent.");
}

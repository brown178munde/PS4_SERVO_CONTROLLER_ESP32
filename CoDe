#include <Bluepad32.h>
#include <ESP32Servo.h>

const int SERVO_PIN = 13;  //the pin which controls ur servo
Servo myServo;
ControllerPtr myController = nullptr;

void onConnectedController(ControllerPtr ctl) {
  Serial.println("Controller connected.");
  myController = ctl;
}

void onDisconnectedController(ControllerPtr ctl) {
  Serial.println("Controller disconnected.");
  if (myController == ctl) myController = nullptr;
}

void setup() {
  Serial.begin(115200);
  BP32.setup(&onConnectedController, &onDisconnectedController);
  BP32.enableVirtualDevice(false);

  ESP32PWM::allocateTimer(0);
  ESP32PWM::allocateTimer(1);
  myServo.setPeriodHertz(50);
  myServo.attach(SERVO_PIN, 500, 2400);

  Serial.println("Ready to control servo with PS4 joystick.");
}
//highly recommended to mess around the code and optimise all the buttons on controller
void loop() {
  if (BP32.update() && myController && myController->isConnected()) {
    int joyY = myController->axisY();
    int angle = constrain(map(joyY, -511, 512, 180, 0), 0, 180);
    myServo.write(angle);
    Serial.printf("Joystick Y: %4d → Servo Angle: %3d\n", joyY, angle);

    if (myController->x()) {
      myController->playDualRumble(0, 100, 0x80, 0x40);
    }
  }
  delay(15);
}

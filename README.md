# Team-graph-tree-Quantum-STEM-school-WRO-FutureEngineers-2024

This repository documents our extensive journey in participating in the World Robotics Olympiad. Here, you will find detailed and comprehensive information on every step of the development and creation of our self-driving robot. From the initial brainstorming sessions and design phase to the challenges we encountered and the solutions we devised, this repository provides a thorough overview of our project. It includes technical specifications, coding intricacies, hardware selections, testing procedures, and performance evaluations. Whether you're a fellow competitor, a robotics enthusiast, or an educator, we hope this detailed account will provide valuable insights and inspiration.


# Content 

* `Car photos` Contais photos which we took while whole procces
* `Our photos` Contains photos of two the most beautiful mans in the world
* `videos` Contains video file of driving tests
* `schemes` Contains circuit and wokwi models of the robot
* `src` Contains code of control software for all components which were programmed to participate in the competition
# Introduction

Welcome to the exciting world of robotics! We proudly present our self-driving robot, a fascinating project built on the Arduino platform. This robot integrates essential components to autonomously navigate its environment, demonstrating basic principles of self-driving technology. Here’s a closer look at what powers our robot:

Key Components
Arduino Microcontroller: The brain of our robot, responsible for processing inputs from sensors and controlling the motor driver.
Motor Driver: Enables precise control of the motors, allowing the robot to move forward, backward, and make turns.
Battery Pack: Supplies the necessary power to the entire system, ensuring the robot can operate autonomously.
Distance Sensors: Two sensors (VL53L0X) placed on either side of the robot to measure distances and help it navigate by avoiding obstacles and staying on course.
Servo Motor: Provides steering control to change the direction of the robot.
How It Works
Sensor Input: The distance sensors continuously measure the distance to objects on either side of the robot.
Processing: The Arduino processes these distance measurements to determine the robot’s position relative to its surroundings.
Control: Based on the sensor inputs, the Arduino sends signals to the motor driver and servo motor, adjusting the speed and direction of the motors to keep the robot moving forward and centered.
Navigation: By combining forward motion and controlled turns, the robot can navigate a predetermined path, such as moving in a square.
This project is a fantastic starting point for anyone interested in robotics, programming, and electronics. It offers a hands-on experience in building and programming a self-driving system, fostering a deeper understanding of the technologies behind autonomous vehicles.

Project Code
Here is the complete code for our self-driving robot, incorporating the features and functions described:

  #include "Adafruit_VL53L0X.h"
  #include <Servo.h>

  // Address for each sensor
  #define LOX1_ADDRESS 0x30
  #define LOX2_ADDRESS 0x31

  // Pins for sensor shutdown
  #define SHT_LOX1 5
  #define SHT_LOX2 6

  // Motor connections
  #define motorPin 7
bool flag, flag1;
  // Servo connections
  #define servoPin 3

  // Define maximum and minimum servo angles
  #define MAX_ANGLE 125
  #define MIN_ANGLE 55

  // Define motor speed
  int MOTOR_SPEED = 150; // Adjust speed as needed
  int timer, timer1;
  Adafruit_VL53L0X lox1 = Adafruit_VL53L0X();
  Adafruit_VL53L0X lox2 = Adafruit_VL53L0X();
  VL53L0X_RangingMeasurementData_t measure1, measure2;
  Servo servoMotor;

  // Proportional constant
  const float Kp = 0.2;  // This value might need tuning

  // Function prototypes
  void setID();
  void readDualSensors();
  void follow();

  void setup() {
    Serial.begin(115200);

    pinMode(SHT_LOX1, OUTPUT);
    pinMode(SHT_LOX2, OUTPUT);

    digitalWrite(SHT_LOX1, LOW);
    digitalWrite(SHT_LOX2, LOW);

    setID();

    pinMode(motorPin, OUTPUT);
    servoMotor.attach(servoPin);
  }

  void loop() {
    follow(); // Call the follow function in a loop
  }

  void setID() {
    digitalWrite(SHT_LOX1, HIGH);
    digitalWrite(SHT_LOX2, LOW);

    if (!lox1.begin(LOX1_ADDRESS)) {
      Serial.println(F("Failed to initialize first VL53L0X sensor"));
      while (1);
    }

    digitalWrite(SHT_LOX2, HIGH);

    if (!lox2.begin(LOX2_ADDRESS)) {
      Serial.println(F("Failed to initialize second VL53L0X sensor"));
      while (1);
    }
  }

  void readDualSensors() {
    lox1.rangingTest(&measure1, false);
    lox2.rangingTest(&measure2, false);
  }

  void follow() {
    int timer = millis();

    readDualSensors(); // Update sensor readings

    int distLeft = measure2.RangeMilliMeter;
    int distRight = measure1.RangeMilliMeter;
    
    // Calculate error
    int error = distRight - distLeft;
    
    // Calculate the angle to steer towards the center
    int angle = 90 + Kp * error;
  MOTOR_SPEED = 200;

    // Ensure the angle is within defined limits
    angle = constrain(angle, MIN_ANGLE, MAX_ANGLE);
    
    // Adjust servo angle
    servoMotor.write(angle);
    
    // Drive motor forward
    analogWrite(motorPin, MOTOR_SPEED);
  int count = 0;
    
  if (!flag1) {
    int timer1 = millis();
    flag1 = true;
  }
  if (distRight > 2000 || distLeft > 2000 && flag == false){
    count++;
    flag = true;
  } else if (timer - timer1 >= 1500) {
    flag = false; 
    flag1 = false;
  }
  if (count >= 11) {
    MOTOR_SPEED = 0;
  }

  }


# Team meaning 

In the vast expanse of the digital landscape, we, the intrepid programmers, have embarked on a journey to not only write code but to etch our names in the annals of tech core with a blend of humor and skill. Armed with keyboards and creativity, we traverse the realms of algorithms and syntax, infusing our creations with a dash of mischief and a hint of danger. With each line of code, we weave a tapestry of wit and expertise, captivating users and peers alike. Our code speaks not just of functionality, but of personality—each bug squashed, each feature implemented, a testament to our ingenuity and resolve. So let it be known, from the depths of nested loops to the heights of cloud computing, that we are not just programmers, but architects of amusement, crafting digital wonders that entertain, enlighten, and inspire

# Team info
Members:
Sanzhar Oral (Engineer)
Amiraly Bekturganov (Programmer)
Coach:
Yerzhan Ayezov (The best mentor and computer science teacher)

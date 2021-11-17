// code written by Aaron Ancheta and Madeleine Doi

/* 
*************************************************************************************************************
We used user circuits' readUltrasonicDistance function and based our loop function off of their project "Ultrasonic Distance Sensor in Arduino With Tinkercad" 
https://www.instructables.com/Ultrasonic-Distance-Sensor-Arduino-Tinkercad/
*************************************************************************************************************
*/

#include <Servo.h> // include the Servo library

// define constants
#define redLED 13 // defines the red LED on pin 13 on Arduino
#define yellowLED 12 // defines the yellow LED on pin 12 on Arduino
#define greenLED 11 // defines the green LED on pin 11 on Arduino

#define piezo1 10 // defines the first piezo on pin 10 on Arduino

#define servo 8 // defines the servo on pin 8 on Arduino

#define sensor1echo 7 // defines the first ultrasonic sensor on pin 7 on Arduino
#define sensor1trig 5 // defines the first ultrasonic sensor on pin 5 on Arduino

#define sensor2echo 6 // defines the second ultrasonic sensor on pin 6 on Arduino
#define sensor2trig 4 // defines the second ultrasonic sensor on pin 4 on Arduino
Servo myservo; // creates servo object

// global variables for distances for ultrasonic sensors
int distanceThreshold = 0;
int cm = 0;
int cm2 = 0;
int goingUp = 0;
int goingDown = 0;

void setup()
{
    // initializes pins 9 - 13 as outputs
    pinMode(redLED, OUTPUT);
    pinMode(yellowLED, OUTPUT);
    pinMode(greenLED, OUTPUT);
    pinMode(piezo1, OUTPUT);
    
    // initialize pin 8 as servo
    myservo.attach(servo);

    Serial.begin(9600); // serial communication

    // initializes pins 6 - 7 as inputs
    pinMode(sensor1trig, OUTPUT);
    pinMode(sensor1echo, INPUT);
    pinMode(sensor2trig, OUTPUT);
    pinMode(sensor2echo, INPUT);

    digitalWrite(greenLED, HIGH); // turns on green light

}

/*
*************************************************************************************************************
Ultrasonic distance code based on user circuits' project "Ultrasonic Distance Sensor in Arduino With Tinkercad" 
https://www.instructables.com/Ultrasonic-Distance-Sensor-Arduino-Tinkercad/
*************************************************************************************************************
*/

/* infinite loop to check:
        how many cars are going up / down
        when to raise / lower gate
*/
void loop()
{
    leftLaneThreshold = 3.5; // sets left lane threshold to 3.5 cm (if within this threshold, the car is on the left)
    rightLaneThreshold = 7.5; // sets right lane threshold to 7.5 cm (if within this threshold, the car is on the right)

    // reads the ultrasonic distance of the object from the sensors 
    cm=0.01723*readUltrasonicDistance(sensor1trig, sensor1echo);
    cm2=0.01723*readUltrasonicDistance(sensor2trig, sensor2echo);

    // checks if a car is entering from either side of the road 
    if ((cm < leftLaneThreshold && cm != 0) || (cm2 > leftLaneThreshold && cm2 < rightLaneThreshold))
    {
        cars++; // adds one to the cars variable 
        delay(500); // waits 0.5 seconds 

        // if there were no cars previously, puts down the rail
        if (cars == 1)
        {
            sequence1();
        }

    }

    // checks if a car is leaving from either side of the road 
    if ((cm2 < leftLaneThreshold && cm2 != 0) || (cm > leftLaneThreshold && cm < rightLaneThreshold))
    {
        // if that is the only car left, puts up the rail
        if (cars == 1)
        {
            sequence(2);
        }

        // checks if there are cars on the road and subtracts from the variable
        if (cars > 0)
        {
            cars--;
        }
    }
}

/*
*************************************************************************************************************
Entire function written by user circuits in their project "Ultrasonic Distance Sensor in Arduino With Tinkercad"
https://www.instructables.com/Ultrasonic-Distance-Sensor-Arduino-Tinkercad/
*************************************************************************************************************
*/

// function gives us a distance of a moving object from the ultrasonic sensor
long readUltrasonicDistance(int triggerPin, int echoPin)
{
    pinMode(triggerPin, OUTPUT);  // Clear the trigger
    digitalWrite(triggerPin, LOW);
    delayMicroseconds(2);

    // Sets the trigger pin to HIGH state for 10 microseconds
    digitalWrite(triggerPin, HIGH);
    delayMicroseconds(10);
    digitalWrite(triggerPin, LOW);
    pinMode(echoPin, INPUT);

    // Reads the echo pin, and returns the sound wave travel time in microseconds
    return pulseIn(echoPin, HIGH);
}


// function for when vehicle passes first sensor
// function activates when there are initially no cars, but a car passing is detected 
void sequence1(void)
{
    digitalWrite(greenLED, LOW); // turns off green light

    // flashes yellow light and sounds alarm for 3 seconds
    for (int i = 0; i < 3; i++)
    {
        tone(piezo1, 3000, 400);
        digitalWrite(yellowLED, HIGH);
        delay(500);
        
        tone(piezo1, 3000, 400);
        digitalWrite(yellowLED, LOW);
        delay(500);
    }
  
    // turns on red light and after 1.5 seconds, gate goes down
    digitalWrite(redLED, HIGH);
    delay(1500);
    myservo.write(180);
}

// function for when vehicle passes second sensor
// functon activates when last vehicle passes, indicating there are no more cars coming and it is safe to go
void sequence2(void)
{
    digitalWrite(redLED, LOW);

    // flashes yellow light and sounds alarm for 3 seconds
    for (int i = 0; i < 3; i++)
    {
        tone(piezo1, 3000, 400);
        digitalWrite(yellowLED, HIGH);
        delay(500);
        
        tone(piezo1, 3000, 400);
        digitalWrite(yellowLED, LOW);
        delay(500);
    }

    // gate goes up, and after 1.5 seconds, green light turns on
    myservo.write(0);
    delay(1500);
    digitalWrite(greenLED, HIGH);
}
    


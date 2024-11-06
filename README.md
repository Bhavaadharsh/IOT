# IOT
const int ultrasonicTrigPin = 2;
const int ultrasonicEchoPin = 3;
const int irSensorPin = 8;
const int buzzerPin = 4; // Choose a suitable digital pin for the buzzer
int peopleCount = 0;
const int ultrasonicThreshold = 50; // Adjust this threshold based on your setup
unsigned long lastCountTime = 0;
const unsigned long countInterval = 1000; // Adjust the interval as needed (1 second in this case)
bool ultrasonicState = LOW;
bool irSensorState = LOW;
void setup() {
pinMode(ultrasonicTrigPin, OUTPUT);
pinMode(ultrasonicEchoPin, INPUT);
pinMode(irSensorPin, INPUT);

pinMode(buzzerPin, OUTPUT); // Set the buzzer pin as an output
Serial.begin(9600);
}
void loop() {
// Ultrasonic sensor to count people in
digitalWrite(ultrasonicTrigPin, LOW);
delayMicroseconds(2);
digitalWrite(ultrasonicTrigPin, HIGH);
delayMicroseconds(10);
digitalWrite(ultrasonicTrigPin, LOW);
unsigned long duration = pulseIn(ultrasonicEchoPin, HIGH);
int distance = duration / 29 / 2; // Convert duration to centimeters
// Check if there&#39;s a change in ultrasonic sensor state
if (distance &lt; ultrasonicThreshold &amp;&amp; ultrasonicState == LOW) {
if (peopleCount &lt; 5) {
peopleCount++;
if (peopleCount == 5) {
// Play a sound when the count reaches 5
tone(buzzerPin, 1000, 1000); // Choose a suitable frequency and duration
}
}
ultrasonicState = HIGH;
} else if (distance &gt;= ultrasonicThreshold &amp;&amp; ultrasonicState == HIGH) {
ultrasonicState = LOW;
}
if (digitalRead(irSensorPin) == LOW &amp;&amp; irSensorState == HIGH) {
if (peopleCount &gt; 0) {
peopleCount--;
}
irSensorState = LOW;
} else if (digitalRead(irSensorPin) == HIGH &amp;&amp; irSensorState == LOW) {
irSensorState = HIGH;
}
Serial.print(&quot;People Count: &quot;);
Serial.println(peopleCount);
delay(1000); // Adjust the delay as needed
}

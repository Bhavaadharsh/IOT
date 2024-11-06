# DWSCRIPTION
One of the primary functions of a crowd detector system is to ensure accurate and
efficient monitoring. It achieves this through the use of sophisticated sensors and
algorithms that are capable of distinguishing between individual people. These sensors
can include infrared motion detectors, cameras with object recognition capabilities, or
even advanced lidar systems. The ability to differentiate between individuals allows the
system to maintain an accurate count, even in scenarios where multiple people may be
entering or exiting simultaneously. We see many public transports getting overcrowded.
But one specific public transport is the bus. Often, we see buses traveling fully packed
with people, very congested for them to stand, buy tickets and travel peacefully. To
prevent this situation, we are bringing an enhanced crowd detector that monitors the
number of passengers entering and exiting the bus. In a normal system, the crowd
detector just counts the number of people entering into a specific area. But in our system,
it not only counts the number of people entering into the area, it also subtracts when
people exit from that area. By doing this, we will have a perfect count of the number of
people entering and exiting that particular area. Additionally, we are going to keep a
threshold value. An alarm sound will be heard, once the people count reaches the
threshold, acknowledging the bus driver so that he can manually open the door to let
people in. As we mentioned before, our system is going to target private buses in the
specified area. It has a count of the number of people entering and exiting the bus.




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

# Ultrasonic Sensor with ESP32 and LED Control

## Project Overview

This project demonstrates how to use an HC-SR04 ultrasonic sensor with an ESP32 to measure distance and control an LED based on the detected distance. The LED will turn on when an object is within 200 cm of the sensor and turn off otherwise.

## Components Required

- ESP32 Development Board
- HC-SR04 Ultrasonic Sensor
- LED
- 220Ω Resistor
- Jumper wires

## Circuit Diagram

![image](https://github.com/user-attachments/assets/8d1b5134-4672-495d-84cb-608a5ebf5348)


### Connections:

1. **HC-SR04 Ultrasonic Sensor:**
   - **VCC** to **3.3V** of ESP32
   - **GND** to **GND** of ESP32
   - **Trig** to **GPIO5** of ESP32
   - **Echo** to **GPIO18** of ESP32

2. **LED:**
   - **Anode (long leg)** to **GPIO25** of ESP32 (through a 220Ω resistor)
   - **Cathode (short leg)** to **GND** of ESP32

```cpp
#define TRIG_PIN 5   // Pin connected to the Trig pin of the ultrasonic sensor
#define ECHO_PIN 18  // Pin connected to the Echo pin of the ultrasonic sensor
#define LED_PIN 25   // Pin connected to the LED (through a resistor)

void setup() {
  Serial.begin(115200);   // Initialize serial communication at 115200 baud rate
  pinMode(TRIG_PIN, OUTPUT); // Set TRIG_PIN as an output
  pinMode(ECHO_PIN, INPUT);  // Set ECHO_PIN as an input
  pinMode(LED_PIN, OUTPUT);  // Set LED_PIN as an output
  digitalWrite(LED_PIN, LOW); // Ensure the LED is off initially
}

void loop() {
  // Send a 10 microsecond pulse to the trigger pin
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  
  // Measure the pulse duration from the echo pin
  long duration = pulseIn(ECHO_PIN, HIGH);
  
  // Calculate the distance in centimeters
  long distance = duration * 0.034 / 2;
  
  // Print the distance to the serial monitor
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");
  
  // Turn on the LED if the object is within 10 cm
  if (distance < 200) {
    digitalWrite(LED_PIN, HIGH); // Turn on the LED
  } else {
    digitalWrite(LED_PIN, LOW);  // Turn off the LED
  }
  
  delay(500); // Wait for 0.5 second before the next reading
}
```

### Explanation:

- **Pin Definitions:**
  - `TRIG_PIN`, `ECHO_PIN`, and `LED_PIN` define the GPIO pins connected to the ultrasonic sensor's trigger, echo pins, and the LED.

- **Setup Function:**
  - `Serial.begin(115200)`: Initializes serial communication at a baud rate of 115200 for debugging purposes.
  - `pinMode(TRIG_PIN, OUTPUT)`: for Setting the trigger pin as an output.
  - `pinMode(ECHO_PIN, INPUT)`: for Setting the echo pin as an input.
  - `pinMode(LED_PIN, OUTPUT)`: for Setting the LED pin as an output.
  - `digitalWrite(LED_PIN, LOW)`: to Ensures the LED is off initially.

- **Loop Function:**
  - Sends a 10 microsecond pulse to the trigger pin.
  - Measures the duration of the echo pulse.
  - Calculates the distance in centimeters.
  - Prints the distance to the serial monitor.
  - Turns the LED on if an object is within 200 cm of the sensor and off otherwise.

   **Run the Simulation:**
  
   **More than 200 cm**
  
   ![image](https://github.com/user-attachments/assets/95ceee58-282e-428c-9249-f27f8b97ade5)

   **less than 200 cm**
  
   ![image](https://github.com/user-attachments/assets/289cc539-a838-49e6-9fff-d150dcc7b1ab)


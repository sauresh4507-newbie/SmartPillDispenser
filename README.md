# Smart Pill Dispenser

An Arduino Uno based pill dispenser that reminds the patient to take medicine on time, releases one dose at a time, and stops the alarm only when the patient confirms with a button. Built as the CS3402 Internet of Things Lab mini project at R V University and simulated in Tinkercad Circuits.

## Problem Statement

Many patients, especially elderly people and those on long-term treatment, forget to take their medicines on time or take a wrong or double dose. Missed doses reduce the effectiveness of treatment, while overdoses can be dangerous. Normal pill boxes give no alert, phone alarms are easy to ignore, and commercial automatic dispensers are expensive. This project provides a simple, low-cost alternative.

## Features

- Visual alert with a blinking LED
- Audible alert with a buzzer
- Push button for the patient to confirm the dose
- Servo motor releases one dose and then closes the compartment
- Simple one-button interface for elderly users
- Easy to extend with IoT features

## Components Used

| Component | Quantity | Purpose |
|---|---|---|
| Arduino Uno R3 | 1 | Microcontroller |
| Micro servo (SG90) | 1 | Rotates the pill compartment |
| Push button | 1 | Dose confirmation |
| Red LED | 1 | Visual alert |
| Piezo buzzer | 1 | Audible alert |
| 220 Ω resistor | 1 | Limits LED current |
| Breadboard and jumper wires | - | Connections |

## Pin Connections

| Component | Arduino Pin |
|---|---|
| LED (via 220 Ω resistor) | Digital 8 |
| Buzzer (+) | Digital 2 |
| Push button | A0 (INPUT_PULLUP, other leg to GND) |
| Servo signal | Digital 9 |
| Servo power | 5V |
| Servo ground | GND |

All LED, buzzer and button ground connections go to the breadboard GND rail, which is connected to an Arduino GND pin.

## How It Works

1. After the set time interval, the LED blinks and the buzzer beeps.
2. The alarm continues until the patient presses the button.
3. On button press, the alarm stops and the servo turns to 90° to release the pill.
4. After 3 seconds, the servo returns to 0° (closed) and the cycle repeats.

## Code

```cpp
#include <Servo.h>

Servo myServo;

int led = 8;
int buzzer = 2;
int button = A0;

void setup() {
  pinMode(led, OUTPUT);
  pinMode(buzzer, OUTPUT);
  pinMode(button, INPUT_PULLUP);   // button goes to GND
  myServo.attach(9);
  myServo.write(0);                // closed position
}

void loop() {
  delay(5000);                     // wait for pill time (demo: 5 s)

  while (digitalRead(button) == HIGH) {   // alarm until button pressed
    digitalWrite(led, HIGH);
    tone(buzzer, 1000);
    delay(300);
    digitalWrite(led, LOW);
    noTone(buzzer);
    delay(300);
  }

  myServo.write(90);               // open dispenser
  delay(3000);
  myServo.write(0);                // close again
}
```

## How to Run

1. Open the circuit in Tinkercad Circuits (link below) or build it using the pin table above.
2. Open the Code panel, switch to Text mode and paste the code.
3. Click **Start Simulation**.
4. After 5 seconds the LED and buzzer start. Hold the button for about a second to stop the alarm and open the dispenser.

To change the reminder interval, edit `delay(5000)`. The value is in milliseconds.

Tinkercad project link: _add your link here_

## Future Scope

- DS3231 real-time clock for fixed medicine times (for example 8 AM, 2 PM, 9 PM)
- ESP8266 / ESP32 Wi-Fi module to send missed-dose alerts to a caregiver's phone
- LCD display showing the next dose and remaining pills
- Multiple compartments for different medicines
- Mobile app and cloud logging of dose history

## Team Members

| S. No | Name | USN |
|---|---|---|
| 1 | | |
| 2 | | |
| 3 | | |

## Course

CS3402 Internet of Things Lab, B.Sc. (Hons) V Semester, School of Computer Science and Engineering, R V University
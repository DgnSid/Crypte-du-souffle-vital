# The Crypt of Vital Breath

## Project Overview

You step cautiously into an **ancient crypt**, where **the air is heavy** and laden with **an eerie silence**. Before you, a stone altar covered with forgotten symbols. In the center lies a **mysterious artifact** that seems to be asleep: a small crystal connected to a strange sound box.
You approach it and read the inscription engraved on the stone:
“**Only the breath of the stars will reveal the melody.**”
Suddenly, a faint glow emanates from the crystal. It's an ancient sensor, designed to react to **ambient temperature**. But the burning air of the crypt has sealed it in a state of dormancy.
You understand then: **if you can cool the artifact by blowing on it, it will reveal its sacred song...**


## How it works :
### How the challenge works
- The **DHT11 sensor** measures temperature continuously.
- If **Temperature < 22°C** → The system detects the abnormality.
- The buzzer activates to play the **"Frère Jacques ”** melody to indicate that the challenge has been solved.
- If the temperature is high, nothing happens.

### Bonus operation

- **Limited Time Mode:** The temperature must drop below 22°C in less than 30 seconds, otherwise the crypt closes and the melody is lost!
- **Mystic Sound Mode:** Adds a second, hidden melody that can only be played at 20°C or below.
- **Ancient Clues Mode:** Displays live temperature on the serial monitor to help adventurers adjust their breath.
- **Reverse Mode:** The Sacred Flame Test: Modifies the code to detect a high temperature (e.g. rubbing the sensor with your fingers to raise it to 30°C and trigger another melody).

---

## Hardware List

| Component | Model | Quantity | 
|----------------------|---------------|----------|
| Arduino Card | Arduino UNO | 1 |
| Temperature Sensor | DHT11 | 1 |
| Buzzer | Passive Buzzer | 1 |
| Breadboard | - | 1 |
| Jumpers | Several |

---

## Technical Specifications

### Arduino UNO board

![UNO](https://github.com/user-attachments/assets/644a792a-c2f4-405f-9d0d-362339074568)

- **Microcontroller** : ATmega328P  
- **Operating voltage** : 5V  
- **Recommended input voltage** : 7-12V  
- **Max current per I/O pin** : 40 mA  
- **Clock frequency**: 16 MHz  

### DHT11 Temperature Sensor
![DHT11](https://github.com/user-attachments/assets/170e7369-e1af-4794-9324-2e684ab54d0c)
 
- **Supply voltage** : 3.3V - 5V  
- **Temperature range** : 0°C to 50°C  
- **Accuracy**: ±2°C  
- **Humidity range**: 20% to 90% RH  

### Passive Buzzer
![arduino-buzzer-module](https://github.com/user-attachments/assets/d2fe0f64-855a-465e-b720-73299437702b)

- **Operating voltage**: 3.3V - 5V  
- **Nominal current**: 10-30 mA  
- **Resonance frequency**: ~2 kHz  

### Breadboard
![Breadboard](https://github.com/user-attachments/assets/5e749c06-f7fd-4c5f-b3c3-e49f244e7c6b)
    
- **Max current per row** : 1A  
- **Max voltage**: 300V  

### Jumpers
![Fils de connexions](https://github.com/user-attachments/assets/77709f66-400b-4776-93cd-d72303767f2f)
  
- **Max current supported** : 1-3A  
- **Length** : 10-30 cm  
---

## Software and Programming

- **Software**: Arduino IDE
- **Programming language**: Arduino (C++)

---

## Schematics
### Project electrical diagram
![schema-electrique](https://github.com/user-attachments/assets/5b78b15b-5821-45eb-a97f-c6357a9135c1)


### Project synoptic diagram
<img width="502" alt="schema-synoptique" src="https://github.com/user-attachments/assets/172bbfb3-9e3a-497a-bf9d-2f47a37df4c9" />


---

## Arduino codes
### Arduino Challenge Code

```cpp
#include <dht11.h>

#define DHT11PIN 7
#define SPEAKER_PIN 8

dht11 DHT11;

// Fréquences des notes en Hz
#define NOTE_DO  262
#define NOTE_RE  294
#define NOTE_MI  330
#define NOTE_FA  349
#define NOTE_SOL 392
#define NOTE_LA  440

// Mélodie "Frère Jacques"
int melody[] = {
  NOTE_DO, NOTE_RE, NOTE_MI, NOTE_DO,
  NOTE_DO, NOTE_RE, NOTE_MI, NOTE_DO,
  NOTE_MI, NOTE_FA, NOTE_SOL,
  NOTE_MI, NOTE_FA, NOTE_SOL
};

// Durées des notes (en ms)
int noteDurations[] = {500, 500, 500, 500, 500, 500, 500, 500, 500, 500, 1000, 500, 500, 1000};

void playMelodyFJ() {
  int length = sizeof(melody) / sizeof(melody[0]);
  for (int i = 0; i < length; i++) {
    tone(SPEAKER_PIN, melody[i], noteDurations[i]);
    delay(noteDurations[i] * 1.30);
  }
}

void setup() {
  Serial.begin(9600);
  pinMode(SPEAKER_PIN, OUTPUT);
}

void loop() {
  float temperature = (float) DHT11.temperature;
  Serial.print("Température (°C) : ");
  Serial.println(temperature, 2);
  delay(1000);

  if (temperature < 22) {
    playMelodyFJ();
  } else {
    noTone(SPEAKER_PIN);
  }
}
```
### Bonus Arduino code

```cpp
#include <dht11.h>
#define DHT11PIN 7
#define SPEAKER_PIN 8

dht11 DHT11;

// Fréquences des notes en Hz
#define NOTE_DO  262
#define NOTE_RE  294
#define NOTE_MI  330
#define NOTE_FA  349
#define NOTE_SOL 392
#define NOTE_LA  440

// Mélodie de "Frère Jacques"
int melody[] = {
  NOTE_DO, NOTE_RE, NOTE_MI, NOTE_DO,
  NOTE_DO, NOTE_RE, NOTE_MI, NOTE_DO,
  NOTE_MI, NOTE_FA, NOTE_SOL,
  NOTE_MI, NOTE_FA, NOTE_SOL,
  NOTE_SOL, NOTE_LA, NOTE_SOL, NOTE_FA, NOTE_MI, NOTE_DO,
  NOTE_SOL, NOTE_LA, NOTE_SOL, NOTE_FA, NOTE_MI, NOTE_DO,
  NOTE_DO, NOTE_SOL, NOTE_DO,
  NOTE_DO, NOTE_SOL, NOTE_DO
};

// Durées des notes (en ms)
int noteDurations[] = {
  500, 500, 500, 500,
  500, 500, 500, 500,
  500, 500, 1000,
  500, 500, 1000,
  250, 250, 250, 250, 500, 500,
  250, 250, 250, 250, 500, 500,
  250, 250, 500,
  250, 250, 500
};

// Fréquences des notes
#define NOTE1_DO  262
#define NOTE1_RE  294
#define NOTE1_MI  330
#define NOTE1_FA  349
#define NOTE1_SOL 392
#define NOTE1_LA  440
#define NOTE1_SI  494
#define NOTE1_DO_HIGH  523  // Do de l'octave supérieure

// Mélodie "Happy Birthday"
int melody1[] = {
  NOTE1_SOL, NOTE1_SOL, NOTE1_LA, NOTE1_SOL, NOTE1_DO_HIGH, NOTE1_SI,
  NOTE1_SOL, NOTE1_SOL, NOTE1_LA, NOTE1_SOL, NOTE1_RE, NOTE1_DO_HIGH,
  NOTE1_SOL, NOTE1_SOL, NOTE1_SOL, NOTE1_MI, NOTE1_DO_HIGH, NOTE1_SI, NOTE1_LA,
  NOTE1_FA, NOTE1_FA, NOTE1_MI, NOTE1_DO_HIGH, NOTE1_RE, NOTE1_DO_HIGH
};

// Durées des notes (plus rapides)
int noteDurations1[] = {
  300, 300, 600, 600, 600, 900,
  300, 300, 600, 600, 600, 900,
  300, 300, 300, 300, 300, 300, 600,
  300, 300, 300, 300, 600, 900
};


// Fréquences des notes
#define NOTE2_DO  262
#define NOTE2_RE  294
#define NOTE2_MI  330
#define NOTE2_FA  349
#define NOTE2_SOL 392
#define NOTE2_LA  440
#define NOTE2_SI  494

// Mélodie "Au clair de la lune"
int melody2[] = {
  NOTE2_DO, NOTE2_RE, NOTE2_MI, NOTE2_RE, NOTE2_DO,
  NOTE2_MI, NOTE2_RE, NOTE2_RE, NOTE2_DO, NOTE2_DO,
  NOTE2_RE, NOTE2_MI, NOTE2_RE, NOTE2_DO, NOTE2_MI,
  NOTE2_RE, NOTE2_RE, NOTE2_DO, NOTE2_RE, NOTE2_RE,
  NOTE2_RE, NOTE2_LA, NOTE2_LA, NOTE2_RE, NOTE2_DO,
  NOTE2_SI, NOTE2_LA, NOTE2_SOL, NOTE2_DO, NOTE2_DO,
  NOTE2_RE, NOTE2_MI, NOTE2_RE, NOTE2_DO, NOTE2_MI,
  NOTE2_RE, NOTE2_RE, NOTE2_DO
};

// Durées des notes (en millisecondes)
int noteDurations2[] = {
  500, 250, 250, 250, 700, // Première phrase
  250, 250, 250, 250, 700, // Deuxième phrase
  250, 250, 250, 250, 250, // Troisième phrase
  250, 250, 250, 250, 700, // Quatrième phrase
  250, 250, 250, 250, 700, // Cinquième phrase
  250, 250, 250, 250, 700, // Sixième phrase
  250, 250, 250, 250, 700, // Septième phrase
  250, 250, 250, 250, 1000  // Huitième phrase
};


void playMelodyAU() {
  int length = sizeof(melody2) / sizeof(melody2[0]);
  for (int i = 0; i < length; i++) {
    tone(SPEAKER_PIN, melody2[i], noteDurations2[i]);
    delay(noteDurations2[i] * 1.30); // Pause entre les notes
    float temperature = (float) DHT11.temperature;
    Serial.print("Temperature (C): ");
    Serial.println(temperature, 2);
    if (temperature < 30 ){
      loop(); 
    }
  }
}

void playMelodyHB() {
  int length = sizeof(melody1) / sizeof(melody1[0]);
  for (int i = 0; i < length; i++) {
    tone(SPEAKER_PIN, melody1[i], noteDurations1[i]);
    delay(noteDurations1[i] * 1.20); // Pause réduite entre les notes
  }
}

void playMelodyFJ() {
  int length = sizeof(melody) / sizeof(melody[0]);
  for (int i = 0; i < length; i++) {
    tone(SPEAKER_PIN, melody[i], noteDurations[i]);

    delay(noteDurations[i] * 1.30); // Petite pause entre les notes
  }
   // Pause avant de rejouer la mélodie
}

void playMelody1() {
  int length = sizeof(melody) / sizeof(melody[0]);
  for (int i = 0; i < length; i++) {
    tone(SPEAKER_PIN, melody[i], noteDurations[i]);

    delay(noteDurations[i] * 1.30); // Petite pause entre les notes
    float temperature = (float) DHT11.temperature;
    Serial.print("Temperature (C): ");
    Serial.println(temperature, 2);
    if (temperature <= 20 ){
      playMelodyHB();
      break; 
    }
    
  } 
}


void setup() {
  Serial.begin(9600);
  pinMode(SPEAKER_PIN, OUTPUT);
}

void loop() {
   float temperature = (float) DHT11.temperature;
  Serial.print("Temperature (C): ");
  Serial.println(temperature, 2);
  delay(1000);

  if (temperature < 22) {
    playMelody1();
  } 
  else if (temperature >= 30 ){
    playMelodyAU();
  }
  else {
    noTone(SPEAKER_PIN);
  }

  // 
}
```

---

# Smart-Diaper
# ESP32-C3 Smart Diaper Health Monitoring IoT System

A low-cost, ESP32-C3-based IoT health monitoring system designed for **infant and elderly care**, tracking **diaper wetness**, **skin temperature**, and **gas emissions**. The system continuously uploads sensor readings to **Firebase Realtime Database**, supports **Wi-Fi auto-reconnect**, and includes daily tracking of wetness frequency to help identify potential dehydration.



##  Project Overview

This system improves hygiene and health monitoring by integrating:

* **Capacitive moisture sensor** to detect diaper wetness
* **DS18B20 digital temperature sensor** for accurate skin/ambient temperature
* **Gas sensor (MQ-135 type)** to detect ammonia or other diaper-related emissions
* **ESP32-C3** for Wi-Fi connectivity & data transmission
* **Firebase RTDB** for cloud storage and remote monitoring

The system:

* Sends readings to Firebase every **5 seconds**
* Tracks moisture events across **24-hour cycles**
* Supports **auto Wi-Fi reconnection** for reliability
* Provides accurate, low-latency data for caregivers


## Components Required

* ESP32-C3 Development Board
* Capacitive Moisture Sensor
* DS18B20 Temperature Sensor
* Gas Sensor (MQ-135)
* 4.7kΩ pull-up resistor for DS18B20
* Jumper wires & breadboard



##  Wiring Diagram

### **Sensor → ESP32-C3 Pins**

| Sensor          | ESP32 Pin | Notes                          |
| --------------- | --------- | ------------------------------ |
| DS18B20 Data    | GPIO 3    | Requires 4.7kΩ pull-up to 3.3V |
| Moisture Sensor | GPIO 0    | Analog input                   |
| Gas Sensor      | GPIO 1    | Analog input                   |



##  Firebase Configuration

You must configure the following in your code:

```cpp
#define WIFI_SSID "YOUR_WIFI"
#define WIFI_PASSWORD "YOUR_PASS"
#define API_KEY "YOUR_FIREBASE_API_KEY"
#define DATABASE_URL "https://your-project.firebaseio.com/"
```

The system:

* Uses anonymous sign-in for Firebase auth
* Auto-reconnects to Wi-Fi and Firebase
* Writes to the RTDB paths:

```
Sensor/moisture
Sensor/gas
Sensor/tempC
```


##  How the System Works

### **1. Wi-Fi Initialization**

The ESP32 attempts to connect and prints progress. Auto-reconnect enabled.

### **2. Firebase Sign-In & Start**

Uses anonymous auth and initializes database communication.

### **3. Sensor Reading Cycle (Every 5s)**

* `analogRead(MOISTURE_PIN)` → moisture detection
* `analogRead(GAS_SENSOR_PIN)` → gas level
* `getTempCByIndex(0)` → DS18B20 temperature

### **4. Cloud Upload**

Each value is uploaded to Firebase in real time. Example path:

```
Sensor/moisture : 412
Sensor/gas      : 201
Sensor/tempC    : 36.2
```


##  Sample Serial Output

```
412 - successfully saved to: Sensor/moisture (int)
201 - successfully saved to: Sensor/gas (int)
36.20 - successfully saved to: Sensor/tempC (float)
```



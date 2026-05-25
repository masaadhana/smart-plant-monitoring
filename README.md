Smart Plant Monitoring & Security System 🌿💧🔒

An IoT-based smart agricultural management and security system that automates plant care while ensuring physical security. Driven by an ESP8266 microcontroller, this system continuously monitors environmental parameters, automates drip irrigation, and alerts users of localized disturbances through real-time push notifications.

🚀 Key Features

Real-Time Environmental Tracking: Constant data collection of ambient air temperature and humidity via a DHT11 sensor, along with live soil moisture tracking.

Automated Smart Irrigation: Closed-loop watering system that triggers a 5V relay and submersible water pump when soil moisture drops below a predefined threshold, automatically running for 10 seconds before re-checking.

Intrusion Detection Alert System: Utilizes a PIR (Passive Infrared) motion sensor to secure the plant area. Instantly pushes a localized warning notification to the user’s mobile device if motion is detected.

Dual-Platform Blynk Cloud Interface: Remote management and monitoring supported across both the Blynk mobile app (interactive gauges and pop-up warnings) and the Blynk Web Console.

Standalone Portability: Built to run independently in remote or outdoor settings using a 7.4V battery pack (powered by dual 18650 cells).

Local Information Display: Features a physical 16x2 I2C LCD screen on the device and a physical manual override ON/OFF toggle switch for the water pump.

🛠️ System Architecture

Hardware Components

Microcontroller: NodeMCU V1.0 ESP8266 (Wi-Fi Enabled)

Soil Sensor: FC-28 Soil Moisture Sensor (Analog interface)

Atmospheric Sensor: DHT11 Temperature & Humidity Sensor

Security Sensor: Passive Infrared (PIR) Motion Sensor

Control Unit: 5V Single-Channel Relay Module

Irrigation Unit: 5V Mini Submersible Water Pump

Visualizer: 16x2 I2C LCD Display

Manual Override: Push Button (Motor ON/OFF)

Power Source: 7.4V Battery Pack (2x 18650 Batteries)

Software & Cloud

IDE: Arduino IDE (for compilation and flashing)

IoT Ecosystem: Blynk IoT Cloud (Console + Mobile App)

Core Libraries:

ESP8266WiFi.h (Network management)

BlynkSimpleEsp8266.h (Blynk integration)

DHT.h (DHT11 parsing)

LiquidCrystal_I2C.h (LCD screen handling)

🔌 Circuit Interface & Wiring

<img width="831" height="568" alt="image" src="https://github.com/user-attachments/assets/18712803-47e2-4934-9931-47eb2547fe0c" />


⚙️ Operating Logic Flow

The software is configured to process environmental conditions sequentially while remaining responsive to rapid security interrupts:

                  +--------------------------+
                  |  Initialize Pins & IoT   |
                  +--------------------------+
                               |
                               v
                  +--------------------------+
                  | Connect to Wi-Fi & Blynk |
                  +--------------------------+
                               |
                               v
                  +--------------------------+
                  |    Read Sensor Array     |
                  | (Moisture, Temp, Motion) |
                  +--------------------------+
                               |
            +------------------+------------------+
            |                                     |
            v                                     v
  [Soil Moisture check]                    [PIR Motion check]
            |                                     |
    Is Moisture Low?                        Is Motion Detected?
      /          \                             /          \
  (Yes)          (No)                       (Yes)         (No)
    |              |                          |             |
    v              v                          v             v
Activate Pump   Keep Pump               Push Blynk     Keep Idle
for 10 seconds    Idle                  Notification
            |      |                          |             |
            +------+--------------------------+-------------+
                               |
                               v
                  +--------------------------+
                  | Stream Datasets to Blynk |
                  +--------------------------+
                               |
                               v
                  +--------------------------+
                  |      Wait 1 Second       |
                  +--------------------------+
                               |
                               +--- (Loop Back to Read Sensor Array)


🛠️ Installation & Deployment

1. Hardware Integration

Assemble the components according to the circuit pin definitions above. Ensure the NodeMCU, LCD, Relay, and PIR sensor share a common ground. Place the soil moisture probes deeply into the plant's root system.

2. Configure Your IoT Dashboard

Log into your Blynk Console.

Create a new Template named Smart Plant.

Set up your Datastreams:

Virtual Pin V1: Temperature (Double/Float)

Virtual Pin V2: Humidity (Integer)

Virtual Pin V3: Soil Moisture (Integer)

Virtual Pin V4: PIR Status / Motion Alert (Integer/String)

Virtual Pin V5: Water Pump Relay Control (Virtual Button)

Design your mobile application layout with gauges for temperature, humidity, and soil moisture alongside notification blocks.

3. Firmware Configuration

Open the project file inside your Arduino IDE and update your local credentials:

// Blynk Cloud Credentials
#define BLYNK_TEMPLATE_ID   "YOUR_TEMPLATE_ID"
#define BLYNK_TEMPLATE_NAME "YOUR_TEMPLATE_NAME"
#define BLYNK_AUTH_TOKEN    "YOUR_AUTH_TOKEN"

// Network Credentials
char ssid[] = "YOUR_WIFI_SSID";
char pass[] = "YOUR_WIFI_PASSWORD";


4. Upload Firmware

Connect the NodeMCU to your computer via USB. Go to Tools > Board, select NodeMCU 1.0 (ESP-12E Module), choose the corresponding port, and press Upload.

🔮 Future Enhancements

AI Edge Computer Vision: Integrate an ESP32-Cam to perform image-based classifications of nearby objects, verifying whether detected motion is a real threat (e.g., pests, animals) or harmless movement.

Micro-Weather Forecast Integration: Leverage external weather API servers to dynamically skip scheduled watering loops if rain is forecast.

Photovoltaic Energy Harvesting: Add a small solar panel coupled with a TP4056 charging circuit to achieve complete off-grid power self-sufficiency.

Comprehensive Soil Nutrient Sensing: Incorporate NPK (Nitrogen, Phosphorus, and Potassium) and pH sensors to track the complete biochemistry of the soil.

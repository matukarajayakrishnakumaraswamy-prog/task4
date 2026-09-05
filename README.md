Capstone: Complete IoT System
A beginner-friendly end-to-end IoT monitoring system demonstrating the complete chain:
ESP32 device → MQTT transport → Web dashboard
The example monitors temperature, humidity, and battery level. It also includes reliability considerations and keeps Wi-Fi/MQTT credentials out of the firmware source.
Project structure
iot-capstone/
├── firmware/
│   ├── esp32_iot.ino
│   └── secrets.example.h
├── dashboard/
│   └── index.html
├── docs/
│   └── system-diagram.svg
├── .gitignore
└── README.md
Hardware
The firmware is designed for an ESP32 with:
DHT22 (or compatible temperature/humidity sensor)
Optional battery-voltage measurement through an appropriate voltage-divider/ADC circuit
USB power for a simple demo
No real credentials are included in this repository.
Transport
MQTT is used because it is lightweight and well suited to IoT devices.
Example topics:
capstone/device01/telemetry
capstone/device01/status
Telemetry is published as JSON:
{
  "device": "device01",
  "temperature_c": 24.6,
  "humidity_pct": 51.2,
  "battery_pct": 87
}
Reliability and power
Device publishes at a controlled interval instead of continuously.
Wi-Fi connection is retried without blocking forever.
MQTT reconnection is handled automatically.
A Last Will message marks the device offline when the broker detects an unexpected disconnect.
Sensor failures are reported instead of silently producing bad readings.
Publishing can be slowed down to reduce power consumption.
Keeping secrets out of firmware
Copy:
firmware/secrets.example.h
to:
firmware/secrets.h
and enter your local Wi-Fi/MQTT credentials there.
secrets.h is ignored by Git and must not be committed.
Dashboard
Open dashboard/index.html in a browser and enter the MQTT broker's WebSocket address and credentials if required.
The dashboard displays:
online/offline state
latest temperature
latest humidity
latest battery percentage
last update time
recent telemetry values
For a browser dashboard, the MQTT broker must expose a WebSocket listener. The example defaults to a public test broker only as a learning/demo configuration; use your own broker for a real deployment.
Demo flow
Configure the ESP32 credentials locally.
Upload firmware/esp32_iot.ino.
Start an MQTT broker with WebSocket support.
Open dashboard/index.html.
Enter the same broker/topic settings.
Watch telemetry arrive from the device.
Security note
This is an educational capstone. For production, use TLS (mqtts/WSS), authentication, unique device credentials, certificate validation, and a private broker.
Submission checklist
Device/firmware
MQTT transport
Dashboard
System diagram
Power/reliability considerations
Secrets kept out of firmware source
README explaining how to run the project

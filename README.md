# 🐔 Smart Poultry House

An ESP32 and Arduino Mega-based IoT poultry management system that automates climate control, feed and water monitoring, and secure access, with AI-powered anomaly detection for smart monitoring.

## Features

- **Climate control:** a DHT11 sensor tracks temperature and humidity. An infrared heating bulb (switched by a relay) turns on when it's too cold, and a ventilation fan turns on when temperature or humidity is too high.
- **Water monitoring:** a water level sensor drives two LEDs. Green means the tank is full, red means it is low or empty.
- **Feed monitoring:** an ultrasonic sensor measures the feed level and shows it on a common-cathode RGB LED: green (sufficient), orange (medium to low), red (little or none).
- **Low-feed alert:** a buzzer sounds once when feed drops to the red level.
- **Access control:** an RFID reader (RC522) allows only authorized tags to enter the poultry house.
- **AI anomaly detection:** sensor data from the ESP32 is analyzed to flag abnormal patterns, such as sudden temperature spikes, unusual humidity, rapid feed or water depletion, and repeated unauthorized access attempts.

## System Architecture

| Layer | Role |
|---|---|
| Arduino Mega | Reads sensors and drives local actuators (relay, fan, LEDs, buzzer, RFID) |
| ESP32 | Wi-Fi connectivity, data upload, and IoT dashboard/alerts |
| AI module | Detects anomalies in the collected sensor data |

## Hardware

| Component | Purpose |
|---|---|
| Arduino Mega | Main controller for sensors and actuators |
| ESP32 | IoT gateway and data transmission |
| DHT11 | Temperature and humidity |
| Water level sensor + green/red LEDs | Water tank status |
| HC-SR04 ultrasonic sensor | Feed level |
| Common-cathode RGB LED | Feed status indicator |
| Buzzer | Low-feed alert |
| Relay module + IR bulb | Heating |
| Fan (via relay/transistor) | Ventilation |
| RC522 RFID reader + tags | Access control |

## Logic Summary

| Condition | Response |
|---|---|
| Temp below lower threshold | IR bulb ON |
| Temp or humidity above upper threshold | Fan ON |
| Water sufficient / low | Green LED / Red LED |
| Feed sufficient / medium / low | RGB green / orange / red (+ one beep) |
| Authorized / unauthorized RFID tag | Access granted / denied |
| Abnormal sensor pattern | AI flags anomaly and sends alert |

## Getting Started

1. Install these libraries from the Arduino Library Manager:
   - `DHT sensor library` (Adafruit)
   - `MFRC522`
2. Open `smart_poultry_house.ino` and check the pin definitions against your board (make sure `RST_PIN` doesn't clash with the RGB LED pins).
3. Calibrate the thresholds for your setup: temperature, humidity, water level, and feeder distance.
4. Replace `authorizedUID1` with your own RFID tag UID (print it to the Serial Monitor by scanning a tag).
5. Upload the sketch to the Arduino Mega, then set up the ESP32 for Wi-Fi and data upload.

> ⚠️ The IR bulb runs on mains voltage. Use a properly rated relay and insulate all high-voltage connections.

## Repository Structure

```
├── smart_poultry_house.ino           # Main Arduino Mega sketch
├── Smart_Poultry_House_Report.docx   # Full project report
└── README.md
```

## Future Improvements

- LCD/OLED status display
- Door lock/solenoid control on RFID grant
- Live web dashboard and SMS alerts
- Data logging and model retraining for the AI anomaly detector

## License

MIT

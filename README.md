# SmartFarm – Smart Irrigation Management System for Sloping Arecanut Plantations

SmartFarm is a modern, responsive, and multilingual IoT web application designed to solve the critical challenges of irrigating sloping agricultural lands—particularly arecanut (*betel nut*) plantations situated on hillsides and undulating Western Ghats terrains.

---

## 🌴 The Core Challenge: Sloping Land Hydrology

On sloping agricultural land, gravity pulls irrigation water downhill. When a farmer applies uniform flood or sprinkler watering:
- **Upper Slope (Zone 1)** drains rapidly, leaving palm roots dry and water-stressed.
- **Middle Slope (Zone 2)** experiences moderate infiltration.
- **Lower Slope (Zone 3)** accumulates excess water, causing nutrient wash-out, topsoil erosion, and dangerous root diseases (*Koleroga*).

### The SmartFarm Solution
SmartFarm provisions **three independently monitored and controlled elevation zones** for every registered land:
1. **Zone 1 – Upper Slope:** Monitors crest moisture; provides targeted replenishment.
2. **Zone 2 – Middle Slope:** Regulates steady gradient infiltration.
3. **Zone 3 – Lower Slope:** Monitors for downhill water accumulation and triggers safety shutoffs when runoff is detected.

The system supports **any number of separate agricultural lands** across different geographic locations, with independent dashboards, weather forecasts, and safety controls.

---

## 🌟 Key Features

| Feature | Description |
| :--- | :--- |
| **Multi-Land Management** | Register any number of lands. Adding a land automatically provisions 3 calibrated slope zones. |
| **Independent 3-Zone Monitoring** | Live moisture (%), status (Dry, Normal, Wet), valve state, and flow metrics for each elevation. |
| **Automatic & Manual Irrigation** | Soil moisture threshold rules, max duration watchdogs, and manual Start/Stop/Pause controls. |
| **Runoff Safety Guard** | Detects downhill water pooling and immediately halts uphill valves to prevent soil erosion. |
| **Weather Forecast Integration** | Hyper-local Open-Meteo rainfall forecasts based on land coordinates. Recommends rain delays. |
| **Water Usage Analytics** | Interactive Chart.js graphs, comparative land charts, estimated ~38% water savings, and CSV export. |
| **Rule-Based Smart Recommendations** | Transparent recommendations based on slope physics, moisture disparity, and rain outlook. |
| **Complete Multilingual Support** | Full native translations for **Kannada (ಕನ್ನಡ)**, **Hindi (हिंदी)**, and **English**. |
| **SpeechSynthesis Voice Alerts** | Spoken audio announcements in Kannada (`kn-IN`), Hindi (`hi-IN`), and English (`en-IN`). |
| **Demo Mode & Live ESP32 Firmware** | Complete simulation engine for browser testing + production ESP32 Arduino sketch for real hardware. |

---

## 🚀 Quick Start Guide

### Option 1: Instant Browser Execution (Zero Setup)
SmartFarm is engineered to run directly in any modern browser without needing dependencies:
1. Double-click [index.html](file:///c:/Users/varsh/Downloads/SmartFarm%20%231/index.html) or open it in Chrome / Edge / Firefox / Safari.
2. The application will start in **Demo Mode** with realistic Malnad arecanut plantations pre-loaded.
3. Test language switching (ಕನ್ನಡ, हिंदी, English), voice alerts, and demo simulations!

### Option 2: Full-Stack Node.js Express Server
To run with the persistent REST backend:
```bash
# 1. Install dependencies
npm install

# 2. Start the server
npm start
```
Open your browser and navigate to:
```
http://localhost:3000
```

### Option 3: Run the Automated Test Suite
To verify multiple-land creation, 3-zone slope provisioning, and safety interlocks:
```bash
npm test
# or
node backend/tests/api.test.js
```

---

## 🌐 Multilingual Voice Alerts

SmartFarm utilizes the browser's Web SpeechSynthesis API:
- **Kannada (ಕನ್ನಡ):** `kn-IN` voice tags (e.g. *"ಭೂಮಿ 1, ವಲಯ 1 ರಲ್ಲಿ ಮಣ್ಣಿನ ತೇವಾಂಶ ಕಡಿಮೆಯಾಗಿದೆ."*)
- **Hindi (हिंदी):** `hi-IN` voice tags (e.g. *"खेत 1, ज़ोन 1 में मिट्टी की नमी कम है।"*)
- **English:** `en-IN` / `en-US` voice tags (e.g. *"Land 1, Zone 1 has low soil moisture."*)

Includes deduplication cooldowns (prevents alert spam), user toggle, and safety decoupling (voice failure never halts hardware control).

---

## 🛠️ Hardware Integration (ESP32)

The firmware sketch is located at [`firmware/SmartFarm_ESP32.ino`](file:///c:/Users/varsh/Downloads/SmartFarm%20%231/firmware/SmartFarm_ESP32.ino):
- Reads 3 Capacitive Soil Moisture Sensors (ADC pins GPIO 34, 35, 32).
- Controls 3 Solenoid Relays (GPIO 25, 26, 27).
- Monitors runoff sensor (GPIO 14) and water flow meter (GPIO 13).
- Features a **local hardware watchdog**: if Wi-Fi disconnects or runoff occurs, relays shut off locally to safeguard the plantation.

See [`firmware/README.md`](file:///c:/Users/varsh/Downloads/SmartFarm%20%231/firmware/README.md) and [`docs/HARDWARE_SETUP.md`](file:///c:/Users/varsh/Downloads/SmartFarm%20%231/docs/HARDWARE_SETUP.md) for detailed wiring schematics and calibration steps.

---

## 📊 Demo Mode vs. Physical Hardware

| Feature | In Demo Mode | In Live Hardware Mode |
| :--- | :--- | :--- |
| **Slope Hydrology** | Simulated via `simulation.js` physics model | Real readings from capacitive sensors |
| **Runoff Trigger** | Triggered via UI test buttons or simulated wetness | Float switch / conductivity sensor at slope base |
| **Water Consumption** | Calibrated flow rate calculations (24 L/min) | Measured by YF-S201 flow pulse meter |
| **Weather Forecast** | Real-time live Open-Meteo API query | Real-time live Open-Meteo API query |
| **Valve Actuation** | Simulated status changes in UI & database | 12V DC solenoid relay actuation |

---

## 📂 Project Structure

```
SmartFarm/
├── index.html                 # Main web application entrypoint
├── css/
│   ├── main.css              # Layout, typography, colors, sidebar
│   ├── components.css        # Cards, badges, slope zone cards, modals
│   └── responsive.css        # Mobile and tablet layouts
├── js/
│   ├── app.js                # Core app controller, page router, modals
│   ├── i18n.js               # Kannada, Hindi, and English dictionaries
│   ├── voice.js              # SpeechSynthesis voice alerts engine
│   ├── weather.js            # Open-Meteo live API integration
│   ├── simulation.js         # Slope hydrology & runoff simulation
│   ├── charts.js             # Chart.js analytics & CSV export
│   ├── storage.js            # Persistent state management & models
│   └── api-client.js         # REST client connecting to Node backend
├── backend/
│   ├── server.js             # Express.js backend server
│   ├── db.js                 # Structured JSON database with atomic transactions
│   ├── routes/
│   │   ├── lands.js          # Multi-land & 3-zone CRUD routes
│   │   ├── telemetry.js      # ESP32 sensor ingestion & safety route
│   │   ├── irrigation.js     # Valve command & interlock route
│   │   └── alerts.js         # Alerts management route
│   └── tests/
│       └── api.test.js       # Automated validation test suite
├── firmware/
│   ├── SmartFarm_ESP32.ino   # Complete ESP32 Arduino C++ firmware
│   └── README.md             # Pinouts, wiring diagram, calibration
├── docs/
│   ├── API.md                # Comprehensive REST API reference
│   ├── ARCHITECTURE.md       # Hydrology physics & safety architecture
│   └── HARDWARE_SETUP.md     # Sloping plantation field setup guide
├── package.json              # NPM dependencies and scripts
└── README.md                 # Complete documentation
```

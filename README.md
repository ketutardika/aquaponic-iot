# Aquamonia - ESP8266 NodeMCU Receiver

ESP8266 NodeMCU firmware for aquaponic system monitoring. Receives sensor data from Arduino via serial communication, provides web-based configuration, and transmits data to remote APIs.

## Features

- **Serial Communication**: Receives 6 sensor readings (Temperature, Humidity, TDS, Turbidity, Water Temperature, pH) from Arduino via CSV format
- **Web Interface**: Bootstrap-based dashboard for device configuration and real-time monitoring
- **Remote API Integration**: HTTP POST transmission with Bearer token authentication
- **Persistent Storage**: EEPROM-based configuration and sensor data caching
- **WiFi Manager**: Easy WiFi setup via "AquamoniaOS" access point
- **LED Status Indicator**: Visual feedback for system activity

## Hardware Requirements

- ESP8266 NodeMCU board
- Arduino Uno/Mega (for sensor data collection)
- Serial connection: D6 (RX), D5 (TX)
- LED on pin D0

## Quick Start

### Arduino IDE Setup

1. **Install ESP8266 Board Support:**
   - Open Arduino IDE
   - Go to **File > Preferences**
   - Add to "Additional Board Manager URLs": `http://arduino.esp8266.com/stable/package_esp8266com_index.json`
   - Go to **Tools > Board > Boards Manager**
   - Search for "ESP8266" and install **esp8266 by ESP8266 Community**

2. **Install Required Libraries:**

   Go to **Sketch > Include Library > Manage Libraries** and install:
   - WiFiManager by tzapu
   - ArduinoJson by Benoit Blanchon

   The following libraries are included with ESP8266 board support:
   - ESP8266WiFi
   - ESP8266WebServer
   - ESP8266HTTPClient
   - EEPROM

3. **Upload Firmware:**
   - Open `aquamonia_arduino_nodemcu_recieve.ino`
   - Select **Tools > Board > ESP8266 Boards > NodeMCU 1.0 (ESP-12E Module)**
   - Select your COM port under **Tools > Port**
   - Click **Upload** button

## Configuration

1. **WiFi Setup**: On first boot, connect to "AquamoniaOS" WiFi network and configure your WiFi credentials

2. **Web Interface**: Access the NodeMCU's IP address in your browser (shown in serial monitor)
   - Default credentials: `admin` / `admin`

3. **Device Setup**: Navigate to `/device-setup` to configure:
   - API endpoint URL
   - Authorization secret key
   - Device API keys for each sensor
   - Send interval (in minutes)

## Serial Communication

**From Arduino to NodeMCU:**
- Format: CSV string
- Example: `25.5,60.2,450,12.3,24.8,7.2`
- Baud rate: 9600

**From NodeMCU to Arduino:**
- Format: JSON with IP address
- Sent automatically after receiving data

## Web Routes

- `/login` - Authentication
- `/` - Dashboard home
- `/device-setup` - Configure API settings
- `/device-monitor` - View live sensor readings
- `/menu-reset` - System reset options

## Architecture

The firmware is modular with the following components:

- `wifi_manager.*` - WiFi connectivity management
- `esp_server_portal.*` - Web server and UI
- `read_serial.*` - Serial communication with Arduino
- `recieve_send_data.*` - API data transmission
- `helper_function.*` - EEPROM and utility functions

See [CLAUDE.md](CLAUDE.md) for detailed architecture documentation.

## License

See [LICENSE](LICENSE) file for details.

## Sensor Types

1. **Temperature** (°C) - Air temperature
2. **Humidity** (%) - Relative humidity
3. **TDS** (PPM) - Total Dissolved Solids
4. **Turbidity** (NTU) - Water clarity
5. **Water Temperature** (°C)
6. **pH Level** (PH) - Water acidity/alkalinity
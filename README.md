# TrailCurrent In-Vehicle Trailer Monitoring

Touchscreen display firmware for an ESP32-S3 Box that receives and displays trailer monitoring data via ESP-NOW wireless protocol. Part of the [TrailCurrent](https://trailcurrent.com) open-source vehicle platform.

## Hardware Overview

- **Microcontroller:** ESP32-S3 Box (16MB Flash, PSRAM)
- **Display:** Built-in 320x240 IPS LCD with capacitive touch (GT911)
- **Communication:** ESP-NOW (peer-to-peer wireless, no WiFi router needed)
- **Key Features:**
  - Real-time speed, GPS coordinates, altitude, and course display
  - Date/time from GNSS module
  - Temperature and humidity monitoring
  - Power distribution module (PDM) device status indicators
  - LVGL-based touchscreen UI designed with EEZ Studio
  - Settings menu with brightness control

## Hardware Requirements

### Components

- **Display:** ESP32-S3 Box development board
- **Communication:** ESP-NOW broadcast from CAN-to-ESP-NOW gateway

## Firmware

See `src/` directory for PlatformIO-based firmware.

**Setup:**
```bash
# Install PlatformIO (if not already installed)
pip install platformio

# Build firmware
pio run

# Upload to board (serial)
pio run -t upload
```

### ESP-NOW Configuration

- Copy `src/secrets.h.example` to `src/secrets.h` and configure the broadcast address
- Never commit `secrets.h` to version control (it's in `.gitignore`)

### ESP-NOW Protocol

**Received Messages (from CAN-to-ESP-NOW gateway):**

| ID | Description |
|----|-------------|
| 6 | Date and time (year, month, day, hour, minute, second) |
| 7 | Speed (knots), course over ground, satellite count, GNSS mode |
| 8 | Altitude |
| 9 | Latitude and longitude |
| 27 | PDM device status (8 devices) |
| 31 | Temperature (C/F) and humidity |
| 35 | Battery voltage and state of charge |
| 36 | Shunt data |
| 44 | Solar charge controller data |

## Project Structure

```
├── GUI/                          # EEZ Studio UI design files
├── lib/                          # Local libraries
│   ├── GFX_Library_for_Arduino/  # Display driver library
│   └── lv_conf.h                 # LVGL configuration
├── src/                          # Firmware source
│   ├── main.cpp                  # Display initialization and LVGL setup
│   ├── espNowHelper.h            # ESP-NOW message handling and protocol
│   ├── globals.h                 # Debug macros
│   ├── actions.cpp               # UI action callbacks
│   ├── vars.cpp                  # UI variable bindings
│   ├── touch.h                   # Touch input driver
│   ├── secrets.h.example         # ESP-NOW config template
│   └── ui/                       # EEZ Studio generated UI code
├── include/                      # Header files
├── partitions/                   # ESP32 flash partition layout
├── platformio.ini                # Build configuration
└── test/                         # Test directory
```

## License

MIT License - See LICENSE file for details.

## Contributing

Improvements and contributions are welcome! Please submit issues or pull requests.

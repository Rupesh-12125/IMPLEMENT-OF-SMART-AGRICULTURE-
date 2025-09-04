# Smart Agriculture for Efficient Cultivation in Hilly Regions (Hardware Demo)

This sample shows a **hardware-aware irrigation controller** and **web dashboard** designed for hilly regions. It runs on a Raspberry Pi (or any Linux PC in mock mode).

## Features
- Soil moisture (ADS1115 analog probe) → moisture % with calibration
- DHT22 temp/humidity
- Water tank level switch (safety interlock)
- Relay-based pump control (active LOW)
- **Slope-safe pulsed irrigation** during safe time windows (dawn/dusk)
- Manual override & configurable thresholds from web UI
- REST API (`/api/metrics`, `/api/control`, `/api/thresholds`)

## Hardware (recommended)
- Raspberry Pi (any recent model)
- DHT22 on GPIO 4
- ADS1115 I2C ADC (addr 0x48), soil probe on A0
- Float switch on GPIO 17 (active LOW = water present)
- Relay on GPIO 27 (active LOW)
- (Optional) flow sensor on GPIO 22

> **Safety:** Use proper isolation, fuses, and outdoor-rated enclosures. Never switch mains directly without a rated relay/SSR and protection. Add a non-return valve on sloped lines.

## Install & Run
```bash
sudo apt update && sudo apt install -y python3-pip python3-venv
python3 -m venv venv && source venv/bin/activate
pip install flask Adafruit_DHT adafruit-ads1x15 RPi.GPIO
python app.py
```

## Calibration
Edit values in `Config` (app.py):
- `MOISTURE_DRY_ADC` and `MOISTURE_WET_ADC` from your soil probe readings in dry and saturated soil.
- `SAFE_WINDOWS`, `PULSE_ON_SEC`, `PULSE_OFF_SEC` to match your slope and crop needs.

## File layout
```
smart-agri-hills/
├── app.py
├── templates/
│   └── index.html
└── static/
    └── styles.css
```

## API quick test
```bash
curl http://localhost:5000/api/metrics
curl -X POST http://localhost:5000/api/control -H "Content-Type: application/json" -d '{"pump":"on","duration_sec":30}'
curl -X POST http://localhost:5000/api/thresholds -H "Content-Type: application/json" -d '{"moisture_target":60,"moisture_hyst":6}'
```

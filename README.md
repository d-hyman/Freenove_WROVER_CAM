# Freenove WROVER Cam

Firmware for a Freenove ESP32-WROVER camera board. It connects to WiFi (via
WiFiManager, so no hardcoded credentials), advertises itself on the local
network as `esp32.local` (mDNS), and serves an MJPEG video stream over HTTP.

## Files

main.cpp:
Runs once at runtime. Setsup camera settings, connects board to local wifi, and calls startCameraServer()

app_httpd.cpp:
Runs for the duration esp32 board is powered. Defines startCameraServer() which sets up two httpd instances(port 80 and 81). Called on by main.cpp.

## Relation to AI_vision

This board is the camera source for the [AI_vision](https://github.com/d-hyman/AI_vision/blob/main/main.py) repo. AI_vision's `main.py` looks up `esp32.local` on the network, pulls the video stream from this firmware, and runs YOLOv8 object detection on the frames. This repo only handles capturing and streaming video — no detection happens on the ESP32 itself.

## Running it

1. Open this folder in PlatformIO (VS Code extension or CLI).
2. Set `upload_port` / `monitor_port` in `platformio.ini` to match the board's
   COM port.
3. Build and upload:
   ```
   pio run -t upload
   ```
4. Open the serial monitor (`pio device monitor`). On first boot, the board
   creates a WiFi access point called `ESP32-CAM-Setup` — connect to it and
   use the captive portal to give it your WiFi credentials.
5. Once connected, the board prints its IP address and becomes reachable at
   `esp32.local`. The video stream is available at `http://esp32.local:81/stream`.

To view the stream directly, open that URL in a browser or VLC. To run object
detection on it, use the AI_vision repo instead.

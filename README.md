# AGRIBOTT # AgriBot

AgriBot is a student project for a small ESP32-controlled farming rover. The prototype plan is to read soil moisture, detect nearby obstacles, use GPS outdoors, and control a water pump through a relay. These are project goals; check the test records before describing any feature as working.

## What AgriBot does

The planned core cycle is to sense soil and obstacles, move safely, water briefly when appropriate, and check the soil again. The project guide and roadmap also describe a phone screen and website, but they are not connected to live robot data yet. The large farm map, camera-based crop analysis, and multi-robot fleet are future ideas, not current prototype capabilities.

## Hardware and sensors

The project documents describe this ESP32 + L298N pin plan. Confirm the exact board and module labels against the parts you have before wiring.

| Part | ESP32 connection | Purpose |
| --- | --- | --- |
| L298N motor driver | ENA 13, IN1 12, IN2 14, IN3 27, IN4 26, ENB 25 | Controls the rover motors |
| Soil-moisture sensor | Analog output to GPIO 34 | Reads soil moisture |
| HC-SR04 ultrasonic sensor | TRIG GPIO 5, ECHO GPIO 18 | Detects nearby obstacles |
| NEO-6M GPS | Module TX to GPIO 16 (ESP32 RX2); module RX to GPIO 17 (ESP32 TX2) | Provides outdoor location data |
| Pump relay | IN to GPIO 19 | Switches pump power |
| Other planned parts | 4WD chassis, motors, battery/power converter, pump, tank and tubing | Robot body and watering system |

The ESP32 GPIO uses 3.3 V logic. The roadmap calls for a resistor divider on the HC-SR04 ECHO line and setting the LM2596 output to 5.0 V with a multimeter before connecting it to ESP32 VIN. Power the pump through the relay circuit, never from an ESP32 GPIO. Verify the relay's active level and pump-off state before connecting the pump.

## Open, build, and upload the ESP32 code

The starter sketch is currently embedded in [`esp32 code master.docx`](<esp32 code master.docx>), rather than stored as an Arduino `.ino` file. To build it:

1. Create an Arduino sketch named `AgriBot.ino` in a folder named `AgriBot` and copy the code from the Word document into it. Copy the sketch only, not the diagnostics checklist beneath it.
2. Open `AgriBot.ino` in Arduino IDE and install ESP32 board support if it is not already installed.
3. Select the ESP32 board model that matches your physical board and its connected port.
4. Click **Verify** to compile. The sketch references `WiFi.h` and `HardwareSerial.h`; it does not currently configure Wi-Fi or list any third-party libraries.
5. After checking the wiring and power, click **Upload**. Open Serial Monitor at **115200 baud** to see the startup and obstacle messages.

The sketch reads soil and ultrasonic inputs, checks for an obstacle closer than 15 cm, and defines motor and irrigation functions. The main loop does not currently call the movement or irrigation functions; GPS characters are read but not parsed or displayed, and soil readings are not printed. Treat it as starter firmware and test each function before relying on it.

## Wiring guide and project documents

- [ESP32 pin map](<Device.docx>) — module names and GPIO assignments for the current ESP32 plan.
- [Plain-language wiring notes](<Here is the breakdown of the wiring guide in plain.docx>) — an older Arduino Uno + L293D simulator example. It uses different pins and parts; do not use its pin numbers to wire the ESP32 + L298N robot.
- [App, website, and 3D tutorial guide](<AgriBot_App_Web_and_3D_Tutorial_Guide.docx>) — tutorial ideas and notes distinguishing the current prototype from future concepts.
- [Development roadmap](<AgriBot_AI_Further_Process (4).pptx>) — assembly, firmware, testing, safety, and demo plan.
- [ESP32 starter sketch and diagnostics](<esp32 code master.docx>) — firmware source currently stored in a Word document.

## What still needs work

- [ ] Move the starter sketch into an `.ino` file and confirm it compiles for the actual ESP32 board.
- [ ] Check the final wiring, voltage ratings, common ground, HC-SR04 ECHO divider, motor direction, and relay behavior against the physical modules.
- [ ] Test sensor readings and calibrate the moisture sensor in dry and wet soil; document the chosen threshold.
- [ ] Complete and test obstacle-stop behavior and motor control on the assembled rover.
- [ ] Verify pump-off behavior, then run a short, controlled pump test and check moisture again.
- [ ] Parse and display GPS data; record repeatable test results with date, setup, expected result, actual result, and fixes.
- [ ] Connect a dashboard or app only after the robot can provide reliable readings. Mark sample values as sample data.

## Test status

Add dated results here as the hardware is tested. The source documents are plans and tutorials; they do not by themselves prove that the assembled robot or any feature has passed testing.

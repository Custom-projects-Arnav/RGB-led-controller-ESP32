# RGB-led-controller-ESP32
ESP32-based Wi-Fi RGB LED remote that lets you control a 12V RGB controller from your phone using a custom web interface and NEC IR commands.

🌈 ESP32 RGB Wi-Fi Remote

A DIY ESP32-based remote controller for a 12V RGB LED strip. The ESP32 connects to a phone's Wi-Fi hotspot and hosts a web-based remote interface. Buttons on the webpage send NEC commands to the original RGB controller through its IR receiver signal line.

✨ Features

- 📱 Control RGB LEDs from a phone
- 🌐 Web-based remote interface
- 📡 Works over Wi-Fi
- 💡 RGB color controls
- 🌈 Fade and Smooth modes
- ⏻ Power / OFF controls
- ⚡ Uses NEC IR protocol
- 🔧 No separate IR LED required with the tested controller setup

🧰 Required Hardware

- ESP32 development board
- 12V RGB LED controller
- Original IR receiver from the RGB controller
- 12V RGB LED strip
- USB cable for powering the ESP32
- Jumper wires

🔌 Wiring

For the tested setup:

RGB Controller IR Receiver| ESP32
VCC| VIN
GND| GND
SIGNAL| GPIO 15

«⚠️ Make sure the grounds are connected together.»

GPIO 15 is used to reproduce the signal normally received by the RGB controller's IR receiver.

💻 Software

Install:

- Arduino IDE
- ESP32 board support for Arduino IDE

The sketch uses:

#include <WiFi.h>
#include <WebServer.h>

These libraries are included with the ESP32 Arduino environment.

📲 Setup

1. Edit the Wi-Fi settings

In the Arduino sketch, change:

const char* ssid = "your_wifi_name";
const char* password = password";

Set "ssid" to your phone's hotspot name.

If your hotspot has a password:

const char* password = "YOUR_PASSWORD";

2. Upload the code (download code)

Connect the ESP32 to your computer using USB.

Select the correct:

- ESP32 board
- COM port

Then upload the sketch.

3. Turn on your phone hotspot

Make sure the hotspot is enabled before powering the ESP32.

For compatibility, use a 2.4 GHz hotspot.

4. Find the ESP32 IP address

Open the Arduino Serial Monitor at:

115200 baud

After connecting, the ESP32 will display something similar to:

Wi-Fi CONNECTED!

ESP32 IP: http://192.168.x.x

5. Open the remote

Connect your phone to the same hotspot.

Open the IP address shown in Serial Monitor in your phone's browser.

Example:

http://192.168.200.98

You should see the ESP32 RGB Remote interface.

🎛️ Command Mapping

The tested NEC address is:

0xEF00

Current command mapping:

Function| NEC Command
OFF| "0x02"
POWER| "0x03"
RED| "0x04"
GREEN| "0x05"
BLUE| "0x06"
WHITE| "0x07"
ORANGE| "0x08"
LIGHT GREEN| "0x09"
LIGHT BLUE| "0x10"
FADE| "0x13"
SMOOTH| "0x16"
MAGENTA| "0x17"

🔧 How It Works

The phone connects to the ESP32 through Wi-Fi.

📱 Phone
   │
   │ Wi-Fi
   ▼
ESP32 Web Server
   │
   │ Button command
   ▼
NEC Signal Generator
   │
   │ GPIO 15
   ▼
RGB Controller IR Signal Input
   │
   ▼
🌈 RGB LED Strip

When a button is pressed, the browser requests a URL such as:

/red

The ESP32 receives the request and sends the corresponding NEC command.

The project currently sends each NEC command twice with a short delay between transmissions to improve reliability with the tested controller.

🛠️ Customizing Buttons

To add another command, create a handler:

void handleExample() {
  sendCommand(0xXX);
  server.send(200, "text/plain", "EXAMPLE sent");
}

Then add a route:

server.on("/example", handleExample);

And add a button to the webpage:

<button onclick="location.href='/example'">
EXAMPLE
</button>

Replace "0xXX" with the NEC command for your controller.

⚠️ Important

This project was developed and tested with a specific 12V RGB controller using:

NEC protocol
Address: 0xEF00
GPIO: 15

Different RGB controllers may use different protocols, addresses, commands, or electrical signal levels.

Do not assume the wiring is safe for another controller without checking its pinout and voltage levels first.

🚀 Future Improvements

Possible upgrades:

- 🎤 Google Assistant / voice control
- 📱 Better mobile UI
- 🌈 RGB color picker
- 💾 Save favorite colors
- 🔆 Brightness control
- 🎨 Custom RGB color control
- 📶 ESP32 access-point mode
- 🔄 Automatic Wi-Fi configuration

📜 License

This project is provided for learning, experimentation, and personal DIY use.
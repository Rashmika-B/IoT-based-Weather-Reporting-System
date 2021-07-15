# IoT based Weather Reporting System

This is a simple Smart Weather Reporting system using **Raspberry Pi 4B+** and weather-related **sensors**. The system monitors and reports the weather parameters such as: Temperature,Humidity,Pressure, and gives alerts whenever it rains. We have also used [ThingSpeak Cloud](https://thingspeak.com/) platform for data collection and performed advanced data analysis using MATLAB and the [IFTTT](https://ifttt.com/) service to send alerts and notification.

## Installation

Use the package manager [sudo apt install](https://www.raspberrypi.org/documentation/) to install the necessary packages for Raspberry Pi.

```bash
sudo apt-get install python-smbus 
```
## Sensors
1. DHT11 Temperature Sensor
2. BMP180 Barometric Pressure Sensor
3. YL-83 Rain Sensor 
## Hardware Setup
<img src="https://github.com/Rashmika-B/IoT-based-Weather-Reporting-System/blob/main/Code%20Snapshots/RaspberryPi%20Setup/Capture.JPG" width="600" height="500">

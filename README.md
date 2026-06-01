# PicoMMu---Arduino-nano-
The Project about PCB and code connect to klipper on raspbery pi 

# BOM
- x2 15 pins female dupont header  (hàng rào cái 15 pin)
- x4 1 pin male dupont header (hàng rào đực 1 pin)
- x2 8 pins female dupont header  (hàng rào cái 8 pin)
- Arduino Nano ATmega328P
- driver drv8825 ( mạch điều khiển động cơ step drv8825)
- 30V-3A Efficient Adjustable Power Supply ( mạch giãm áp điều chĩnh mini 30v-3a)
- jack xh2.54 3pin
- jack xh2.54 4pin
- dc-dc conecter
- cap 100uf
- wire (red,black,green)

# How to use
# Software configuration
Once built and connected to your Klipper host, use the provided Klipper config file.

You need at least to set the serial path in the [mcu ...] of the provided klipper configuration file.
To find the serial of your Arduino Nano:
- BEFORE connecting the Nano with USB on the Raspberry Pi, connect to the Pi using SSH and run the command ls /dev/serial/by-id/. Note the result.
- Connect the Nano with USB on the Pi, wait a few seconds for the Pi to detect the USB device, and re-run the same command as before.
- A new "file" should be there, looking like usb-1a86_USB2.0-Serial-if00-port0: it represents your Arduino Nano
- Change the serial setting value in the Klipper configuration using the full path to the serial file. e.g.: serial: /dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0
## D

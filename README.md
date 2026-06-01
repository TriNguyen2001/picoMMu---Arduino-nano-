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
## Software configuration
Once built and connected to your Klipper host, use the provided Klipper config file.

You need at least to set the serial path in the [mcu ...] of the provided klipper configuration file.
To find the serial of your Arduino Nano:
- BEFORE connecting the Nano with USB on the Raspberry Pi, connect to the Pi using SSH and run the command ls /dev/serial/by-id/. Note the result.
- Connect the Nano with USB on the Pi, wait a few seconds for the Pi to detect the USB device, and re-run the same command as before.
- A new "file" should be there, looking like usb-1a86_USB2.0-Serial-if00-port0: it represents your Arduino Nano
- Change the serial setting value in the Klipper configuration using the full path to the serial file. e.g.: serial: /dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0
## Flashing firmware

To flash the firmware, you do not need to have the configuration file ready, but you need:
- The Nano connected to the Raspberry Pi
- The path of the serial representing the Nano on the Pi (see previous section)
- Klipper installed on the Pi
- AVR compilation package installed on the Rapsberry Pi hosting Klipper: sudo apt install gcc-avr avr-libc avrdude binutils-avr

To flash the Nano with Klipper:
- Connect to the Pi using SSH.
- Create a klipper_config directory in you home directory, if it not exists already: mkdir -p ~/klipper_config.
- Go the the directory containing klipper (usually: cd ~/klipper).
- Run make clean KCONFIG_CONFIG=~/klipper_config/config.arduinonano.
- Run make menuconfig KCONFIG_CONFIG=~/klipper_config/config.arduinonano and configure as followed, and then save'n'quit:
  - Enable extra low-level configuration options: NOT checked
  - Micro-controller Architecture: Atmega AVR
  - Processor model: atmega328p
- Run make KCONFIG_CONFIG=~/klipper_config/config.arduinonano to build the firmware.
Note: If the firmware compilation fails with errors like '.data' is not within region 'data', you may sufffer a known bug with AVR toolchain on Debian Bullseye (check your linux distribution codename with lsb_release -a). If so, you can try a workaround describe here to install a previous and non-buggy AVR toolchain. Once installed, re-roll the current procedure from the make clean step (a reboot of the Pi may be usefull between new toolchain installation and the retry).
- Stop Klipper if it's running (usually: sudo systemctl stop klipper)
- Run make flash KCONFIG_CONFIG=~/klipper_config/config.arduinonano FLASH_DEVICE=/dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0, replacing the FLASH_DEVICE= value with the full path of you Nano on your Pi! This will flash the firmware to the Arduino Nano.
- if cant , do this
  avrdude -v \
  p atmega328p \
  c arduino \
  P /dev/ttyUSB0 \
  b 57600 \
  D \
  U flash:w:/home/pi/klipper/out/klipper.elf.hex:i
- Start Klipper (usually: sudo systemctl start klipper)

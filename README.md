RF24SN nodejs Server
====================

NOTE: This is a fork! Testing is ongoing.. The following have been added/changed:
- Updated to latest packages of yargv, mqtt and nrf
- Fixed errors in arguments and instructions
- Added 1 param for changing the rate (250kbps/1Mbps/2Mbps)
- Added 1 param for checking that radio input is received. I've noticed that after running for 3-5 days it stops receiving data and I have not identified where in the code (what package) the problem resides. With this parameter you can specify the max number of seconds between received messages before a restart is made.

Full implementation of [RF24SN](https://github.com/VaclavSynacek/RF24SN) with little dependencies. Should run on all
platforms where there is nodejs and the [node-nrf](https://github.com/natevw/node-nrf) driver / [pi-spi](https://github.com/natevw/pi-spi) driver - currently it has been tested on Raspberry Pi.

For full description of protocol, client server setup or alternative implementations, see [RF24SN](https://github.com/VaclavSynacek/RF24SN)


Installation (as this is a fork use the git url):
```Shell
npm install https://github.com/nidayand/RF24SN_nodejs_Server.git --global
```
Uninstallation:
- Delete /usr/lib/node_modules/rf24sn folder
- Delete /usr/bin/rf24sn

Usage:
```Shell
sudo rf24sn -b mqtt://localhost:1883 --spi /dev/spidev0.0 --ce 25 --irq 24 -vvv --rate 1Mbps --check 600
```

The -v parameter sets logging level:
* (no v) : almost silent, only errors and warnings
* -v : only received radio packets are reported
* -vv : received radio packets and MQTT communication is reported
* -vvv : debug info
* -vvvv : silly amount of data including underlaying nrf pipes statuses

The sudo is required in standard Raspbian instalation unless access to /dev/spidevX.X and the GPIO pins has been granted to other user (via [quick2wire](https://github.com/quick2wire/quick2wire-gpio-admin) or similar).


## Wiring

![Wiring](https://raw.githubusercontent.com/VaclavSynacek/RF24SN_nodejs_Server/master/nRF24L01-RPi.png "Wiring")

The SPI wires (yellow) have to go exactly to their counterparts:
* MOSI to MOSI
* MISO to MISO
* SCK to SC(L)K

The VCC (red) has to go to any **3.3V** pin. Connecting it to 5V pin will damage the nRF24L01.

GRN (black) can go to any ground.

The CSN (blue) has to go to either CS0 or CS1. This determines the spi device. To use the /dev/spidev0.**0** use the CS **0**.

The CE and IRQ (cyan) can go to any GPIO pin. The diagram follows the rf24sn defaults - CE 25, IRQ 24.

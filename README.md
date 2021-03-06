# Simple Open EtherCAT Master Library for Arduino

This is SOEM (Simple Open EtherCAT Master) library ported for Arduino.

It's just for educational or experimental purposes.

The original SOEM is at https://github.com/OpenEtherCATsociety/SOEM

## Supported hardware

* Arduino Due + Ethernet Shield 2 (W5500)
* ESP32 + WIZ850io (W5500)
* M5Stack + M5Stack LAN Module (W5500)
* M5 ATOM Matrix + WIZ850io (W5500)
* GR-SAKURA (Renesas RX63N)
* GR-ROSE (Renesas RX65N)
* chipKIT Max32 (PIC32MX) + Ethernet Shield 2 (W5500)

## limitations 
Many of MCUs don't have large memory, and has only one LAN port. SOEM4Arduino reduces memory usage, and does not support redundant LAN ports.

Doe to reducing memory usage, some functions are limited.
| item | constant name | original SOEM | SOEM4Arduino |
| ---- | ---- | ---- | ---- |
| max entries in Object Dictionary list | EC_MAXODLIST | 1024 | 64 |
| max entries in Object Entry list | EC_MAXOELIST | 256 | 64 |
| max number of slaves in array | EC_MAXSLAVE | 200 | 64 |
| number of frame buffers | EC_MAXBUF | 16 | 2 |

## Notes for ESP32

Connect ESP32 and W5500 as shown below.

| ESP32-DevKitC | WIZ850io (W5500) |
| ---- | ---- |
| GND  | GND  |
| 3V3  | 3.3V |
| IO23 (VSPI MOSI) | MOSI |
| IO19 (VSPI MISO) | MISO |
| IO18 (VSPI SCK)  | SCLK |
| IO5  (VSPI SS)   | SCSn |  

However, M5Stack's SS is not IO5 but IO26.

## Notes for M5 ATOM Matrix

M5 ATOM Matrix's IO pins are remappable. The pin assignment is determined as follows. It's just for ease of wiring.

| M5 ATOM Matrix | WIZ850io (W5500) |
| ---- | ---- |
| GND  | GND  |
| 3V3  | 3.3V |
| IO21 | MOSI |
| IO19 | MISO |
| IO25 | SCLK |
| IO22 | SCSn | 

Please choose <b>ESP32 Pico KIT</b> as the board configuration, and use 115200 for the baud rate.

## Notes for GR-ROSE

You must create a user task to use SOEM.

GR-ROSE's main task (setup & loop) has only 512 byte stack.
On the other hand, SOEM uses more than 3k byte local variables.
Therefore the memory corruption will occur when you call SOEM's API on the main task.
You must creat a task with more than 4k byte stack and then call SOEM's API on the task.
Please see the sample sketch (slaveinfo.ino).

## Documentation of original SOEM
See https://openethercatsociety.github.io/doc/soem/

## Article about this library (written in Japanese)
See https://lipoyang.hatenablog.com/entry/2019/08/13/125008

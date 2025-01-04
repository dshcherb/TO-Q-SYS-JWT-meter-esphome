# Overview

This repository contains a configuration file that can be used to flash [TO-Q-SYS-JWT](https://www.tongou.com/product/single-phase-din-rail-smart-meter) with ESPHome using Libretiny as it has a [CBU](https://docs.libretiny.eu/boards/cbu/) (BK7231N) chip built-in.

# UART Buffer Size Notice

This config relies on a version of ArduinoCore-API based on upstream 1.5.1 with two patches:

- The buffer size increase from default 64 bytes to 1024 bytes: https://github.com/dshcherb/ArduinoCore-API/commit/365122ef16e4065a4cc06c4f1db786a5f6d0b7d8
- The printf feature patch from the [Libretiny fork](https://github.com/libretiny-eu/ArduinoCore-API/commit/3a4cbfcc88f8c2c6b52e406745cd51db1370f621) of ArduinoCoreAPI: https://github.com/dshcherb/ArduinoCore-API/commit/ce57327c3bbcb70d007f915a1c5c92dd6f16a5f1

Without this config the device will not function properly as the MCU sends datapoints quickly and needs 300+ bytes of RX buffer space available. The main CBU chip is not able to process the default 64-byte buffer fast enough and some data is discarded: as a result only a small amount of data points is observed by the Tuya module in esphome.

As it is [not possible to override](https://github.com/esphome/issues/issues/6240#issuecomment-2349516017) this config easily, this config just incorporates a downstream change to alter the buffer size.

# Flashing

Flashing can be done via [UART1](https://docs.libretiny.eu/boards/cbu/#pinout) using an UART:

```bash
python3 -m venv esphome-venv
. esphome-venv/bin/activate
pip3 install esphome ltchiptool
esphome run --device /dev/<your-serial-device-name> <path-to>/config-tongou-to-q-sys-jwt.yaml
```

# OTA

Make sure the device has a stable Wi-Fi connection, otherwise attaching to UART again will be needed again if OTA is interrupted.

```bash
python3 -m venv esphome-venv
. esphome-venv/bin/activate
pip3 install esphome
esphome run --device <device-ip> <path-to>/gas-alarm-DY-RQ400A.yaml
```

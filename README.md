# Overview

This repository contains a configuration file that can be used to flash [TO-Q-SYS-JWT](https://www.tongou.com/product/single-phase-din-rail-smart-meter) with ESPHome using Libretiny as it has a [CBU](https://docs.libretiny.eu/boards/cbu/) (BK7231N) chip built-in.

More details about the device are documented at the [ESPHome devices page](https://devices.esphome.io/devices/Tongou-TO-Q-SYS-JWT-power-meter).

# UART Buffer Size Notice

The UART buffer size needs to be increased since the device will not function properly otherwise. The MCU sends datapoints quickly and needs 300+ bytes of RX buffer space available at the receiving side. The main CBU chip is not able to process the default 64-byte buffer fast enough and some data is discarded: as a result only a small amount of data points is observed by the Tuya module in esphome.

As of [Libretiny 1.8.0](https://github.com/libretiny-eu/libretiny/releases/tag/v1.8.0) it is possible to adjust the RX buffer size using a framework option so make sure to set `LT_SERIAL_BUFFER_SIZE: 512` which is large enough for the incoming messages from the MCU.

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

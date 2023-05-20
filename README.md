# OpenHT-hw
Hardware part of the OpenHT - an open-source, SDR handheld transceiver.

<img src="https://github.com/M17-Project/OpenHT-hw/blob/main/render_iso_back.png" width="800">

## Description
The device is a complete, low RF power I/Q transceiver, supporting virtually any mode (including [M17](https://m17project.org) and [FreeDV](https://freedv.org)).
It is built around the [AT86RF215 chip](https://www.microchip.com/en-us/product/AT86RF215) by Microchip/Atmel
and [LIFCL-40-9SG72C](https://www.latticesemi.com/Products/FPGAandCPLD/CrossLink-NX) FPGA from Lattice as a shield for [32F469IDISCOVERY](https://www.st.com/en/evaluation-tools/32f469idiscovery.html). It is possible to use the board with other MCU platforms.

Technical details:
- frequency range: 389.5 - 480, 2400 - 2483.5 MHz (RX, TX frequency range is limited by your local laws)
- RF power output: <14dBm (<25mW)

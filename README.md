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

## Assembly tips and U204 issue
After the board has been populated, perform the steps below:
1) Remove U204 (you need to take the lower shield off).
2) Using a copper wire, connect `VIN` and `VBAT`.
3) Secure goldpin headers in place by putting them on the DISCO board (Arduino male connector and the 2x8 female connector) and solder them.
4) Solder JP401 and short the two left pins (`JTAG_EN` to `GND`).
5) Prepare a micro-SD card with the FPGA image, its filename has to be `fpga_00.bit`. The SD card has to be preformatted with FAT32.
6) Plug the mini-USB port and flash the firmware from the `main` branch.
7) In the GUI, tap on the cog icon (upper left corner), then press `Load FPGA`.

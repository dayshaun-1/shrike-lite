# Controlling FPGA's led with SPI 
In this example we have demonstrated how to turn on and off the on board led on shrike using a SPI command from the RP2040 or any other SPI controller.

For this a SPI target is implemented on the fpga and when it receives the data 0xAB it turn's on the led and when it receives 0xFF it turn's it off.

## Overview on FPGA side
- This project consists of two modules :   
    1) `top :` This module controls LED based on input.
        - When 0xAB is received, it turns ON the LED
        - When 0xFF is received, it turns OFF the LED
        - Instantiate `spi_target` module.
    2) `spi_target : ` This module implements a SPI Target.
        - This module is used to send and transmit data through SPI protocol.

---

## Top Module Interface

| Signal        | Direction | Description                    |
|---------------|-----|--------------------------------------|
| `clk`         | In  | System clock (50 MHz typical)        |
| `clk_en`      | Out | Clock enable (always 1)              |
| `rst_n`       | In  | Reset Pin (active low)               |
| `led`         | Out | Output LED state                     |
| `led_en`      | Out | LED enable (always 1)                |
| `spi_ss_n`    | In  | Input target select signal (activel low) |
| `spi_sck`     | In  | Input SPI clock signal               |
| `spi_mosi`    | In  | Input from controller                |
| `spi_miso`    | Out | Output to controller                 |
| `spi_miso_en` | Out | miso enable signal                   |

---

## Pin Usage for Testing
This design was tested using configuration given below:

### FPGA 
| FPGA GPIO Pin | Signal Name | Direction | Description                       |
|----------|-------------|-----------|-----------------------------------|
| 3        | spi_sck     | Input     | SPI clock                 |
| 4        | spi_ss_n    | Input     | chip select               |
| 5        | spi_mosi    | Input     | mosi (receiver) line      |
| 6        | spi_miso    | Output    | miso (transmission) line  |
| 6        | spi_miso_en | Output    | miso enable signal        |
| 18       | rst_n       | Input     | For Reseting the FPGA     |
| 16       | led         | Output    | Led logic (state)         |
| 16       | led_en      | Output    | Led enable signal         |

### RP2040
| RP2040 Pin | Signal Name | Direction | Description                       |
|----------|-------------|-----------|-----------------------------------|
| 2        | SCK         | Output    | SPI clock                         |
| 1        | CS          | Output    | chip select                       |
| 3        | MOSI        | Output    | Master output                     |
| 0        | MISO        | Input     | Master input                      |
| 14       | Reset       | Output    | reset signal (active low)         |

When testing using RP2040 MCU, first load the bitstream to FPGA then run the `blink.py`. The pin number in your FPGA constraints must match what is used in the micropython code.

---
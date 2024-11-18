# Quirks

## NodeMCU

[DATASHEET](./nodemcu-datasheet.pdf)
[PINOUT](./nodemcu-pinout.avif)

- NodeMCU is a development board that uses the ESP8266 chip, packaged as a ESP-12E module.
- The NodeMCU Board maps the ESP 12E's pins to custom pins named as DX pins.

## To Program

- Using the Arduino IDE.
    1. Install the ESP8266 board package.
    2. Select the board as NodeMCU 1.0 (ESP-12E Module).
    3. Code like you would for an Arduino.
    > NOTE: The pin numbers in the code are the DX pin numbers, not the GPIO numbers. So, D8  GPIO8.

- Using platformio.
    - PlatformIO has native support for it, go look at the website.


## Quirks

- Most boards use CP2102 Serial to USB converter, so if on windows, download it's driver, and if on linux, gentoo in particular, compile that in.
- One may think that the NodeMCU shouldn't be powered by more than 3.3V, but it can be powered by 5V.
- However, you can't have the signal be 5V, as the ESP8266 is a 3.3V device. You can fry GPIOs like this.
- It also has an upper current limit of 12mA per GPIO pin, that's not much.
- Don't connect it directly to relays, have an optocoupler.
- Sudden current demand spikes can also crash it.
- Remember to regularly `yield()` when taking up a core for too long, otherwise it will crash.
- Default Baud rate for errors is 76800.
- D3 is mapped to GPIO0, D4 is mapped to GPIO2, D8 is mapped to GPIO15
- GPIO0, GPIO2 and GPIO15 determine the boot mode, so if you are unable to upload code to it, check whether the states of these pins is in the correct state at early boot.
- GPIO15 also acts as chip select for SPI.

|                                            | GPIO0 (D3) | GPIO2 (D4) | GPIO15 (D8) |
| ------------------------------------------ | ---------- | ---------- | ----------- |
| UART (aka. When you can upload code to it) | LOW        | HIGH       | LOW         |
| Flash Boot (aka. When code runs)           | HIGH       | HIGH       | LOW         |

- You can pull down a pin by attaching a high value resistor to a ground.
- You can pull up a pin by attaching a high value resistor to a VCC.
- If there is literally no response from the nodeMCU, check the EN pin and the RST Pin, both of these should be high and not fluctuating.
- Serial.print sends data to both USB and the RX, TX pins on board, if using them for something, make sure you are printing only relevant stuff.
- The antenna sucks a little, may dire wiring the antenna manually, some people have reported success, haven't verified personally.
- SPI can be finicky, when a connection is broken, it needs to be reset. My current solution is to just reset SPI and begin a new connection everytime data needs to be sent.
- In particular, MFRC522 module fks with it, sending it to a different boot mode if you connect it's pins to the ones specified above without care.

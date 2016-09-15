# Hardware documentation for Tessel modules

## Scope

This document provides hardware overviews for each Tessel module:

*  [Accelerometer](https://github.com/tessel/hardware/blob/master/modules-overview.md#accelerometer)
*  [Ambient](https://github.com/tessel/hardware/blob/master/modules-overview.md#ambient)
*  [Climate](https://github.com/tessel/hardware/blob/master/modules-overview.md#climate)
*  [GPS](https://github.com/tessel/hardware/blob/master/modules-overview.md#gps)
*  [MicroSD Card](https://github.com/tessel/hardware/blob/master/modules-overview.md#micro-sd-card)
*  [Relay](https://github.com/tessel/hardware/blob/master/modules-overview.md#relay)
*  [RFID](https://github.com/tessel/hardware/blob/master/modules-overview.md#rfid)
*  [Servo](https://github.com/tessel/hardware/blob/master/modules-overview.md#servo)

## Accelerometer

Measures acceleration in the x, y, and z directions.

### Quick overview:

Parameter | Value
----------|------
TM part # | TM-01-XX
Latest version | TM-01-02
Key components | [MMA8452Q](http://www.freescale.com/files/sensors/doc/data_sheet/MMA8452Q.pdf)
Current consumption (rated max) | 165 microamps
Current consumption (average) | 165 microamps
Communication protocol | I2C, GPIO

### Notes
*  I2C address can be modified by shorting J3
*  +x = towards the Tessel, +y = G3 --> GND, +z = up

## Ambient

Measures ambient light and sound. Sound gain adjustable via R6.

### Quick overview:

Parameter | Value
----------|------
TM part # | TM-08-XX
Latest version | TM-08-03
Key components | [PT15-21C/TR8](http://www.everlight.com/file/ProductFile/PT15-21C-TR8.pdf) photodiode, [SPU0410HR5H-PB](http://ke-www2.knowles.com/search/prods_pdf/SPU0410HR5H.pdf) microphone
Current consumption (rated max) | 10 mA
Current consumption (average) | 10 mA
Communication protocol | SPI, GPIO

### Notes

*  Light signal path includes low pass filters with *f<sub>c</sub>* = 1.6 Hz
*  Sound signal path measures loudness via a half wave rectifier

## Climate

Measure temperature and humidity

### Quick overview:

Parameter | Value
----------|------
TM part # | TM-02-XX
Latest version | TM-02-02
Key components | [SI7005-B-GM](http://www.silabs.com/Support%20Documents/TechnicalDocs/Si7005.pdf) sensor
Current consumption (rated max) | 565 microamps
Current consumption (average) | 320 microamps
Communication protocol | I2C, GPIO

### Notes

* There is an important errata with the Si7005 chip (documented only in [this application note](http://www.silabs.com/Support%20Documents/TechnicalDocs/AN607.pdf)) which says:

> The Si7005 should not be on the same bus as other I2C devices when it is active. It acknowledges data
bytes that match its address. This issue has been reolved with other members of the Si70xx family

In practice, this means that the TM-12-04 Climate module should always be used on its own I2C bus. Fortunately, two I2C busses exist on Tessel: one is shared between module ports A and B, the other between C and D.

## GPS

Global positioning system: position reference, movement speed & heading, time reference

### Quick overview:

Parameter | Value
----------|------
TM part # | TM-09-XX
Latest version | TM-09-02
Key components | A2235-H GPS module ([user manual](http://www.maestro-wireless.com/download-dp40), [product page](http://www.maestro-wireless.com/a2235-h))
Current consumption (rated max) | 42 mA
Current consumption (average) | 36 mA
Communication protocol | UART, GPIO

### Notes

*  The A2235-H is based on CSR SiRFstarIV chipset, which supports a novel GPS fix mode and a variety of low power states.
*  The A2235-H supports two different data modes: NMEA and SiRF binary mode (also called OSP mode). Technical Machine's default APIs use the module in NMEA mode.
*  A-GPS is supported with client generated extended ephemeris up to 3 days.

## Infrared

Communicate with and command household electronics using IR light

### Quick overview:

Parameter | Value
----------|------
TM part # | TM-11-XX
Latest version | TM-11-03
Key components | [TSOP38238](http://www.vishay.com/docs/82491/tsop382.pdf) 38 kHz IR demodulator, [SFH-4646-Z](http://www.osram-os.com/Graphics/XPic6/00116152_0.pdf) IR LED
Current consumption (rated max) | 95 mA
Current consumption (average) | 5 mA
Communication protocol | UART, GPIO

### Notes

*  Receiving IR takes very little power. Transmission, on the other hand, uses a good deal of power.
*  Receiver frequency is fixed at 38 kHz, although pin-compatible parts which operate at other frequencies presumably exist. Check your appliance's manual and/or the internet to make sure your hardware is compatible.

## Relay

Switch large, high voltage loads

### Quick overview:

Parameter | Value
----------|------
TM part # | TM-04-XX
Latest version | TM-04-04
Key components | IM02DGR ([DigiKey page](http://www.digikey.com/product-detail/en/IM02DGR/PB1171CT-ND/1828461))
Current consumption (rated max) | 90 mA
Current consumption (average) | 10 mA
Communication protocol | GPIO

### Notes

*  Relays are normally open (load disconnected) and will open should the module/Tessel lose power
*  Current draw increases when the relays close (*I<sub>draw</sub>* = 10mA + 40mA * [number of relays on])
*  Rated to 240 V, 5 A (but please be careful)
*  Insert and remove wires by pressing down on the connectors with a ballpoint pen (or similar).

## RFID

Read and write from/to 13.56 MHz RFID cards and tags

### Quick overview:

Parameter | Value
----------|------
TM part # | TM-10-XX
Latest version | TM-10-03
Key components | PN532 ([datasheet](http://www.adafruit.com/datasheets/pn532longds.pdf), [user manual](http://www.nxp.com/documents/user_manual/141520.pdf), [MiFARE tag IC/protocol docs](http://www.nxp.com/documents/data_sheet/MF1S503x.pdf)) 13.56 MHz RFID transceiver
Current consumption (rated max) | 150 mA
Current consumption (average) | 60 mA
Communication protocol | I2C, GPIO

### Notes

*  Module can be switched to communicate over SPI with appropriate resistor pop option swaps
*  Current is highest when transmitting and much lower most of the time
*  A MiFare Classic card ships with the module
*  Hardware-compatible with [ISO-14443 RFID tags](http://nfc-tools.org/index.php?title=ISO14443A), including but not limited to: MiFare (Classic 1k, Classic 4k, Ultralight), DesFire, and FeliCa. Further information in the link.
*  Compatible cards (consumer): Charlie, Clipper


## Servo

Control up to 16 servos

### Quick overview:

Parameter | Value
----------|------
TM part # | TM-03-XX
Latest version | TM-03-02
Key components | [PCA9685](http://www.adafruit.com/datasheets/PCA9685.pdf) LED driver
Current consumption (rated max) | 50 mA
Current consumption (average) | 10 mA
Communication protocol | I2C, GPIO

### Notes

*  An I2C LED PWM driver makes for a great servo driver too!
*  16 channels
*  Tessel is a 3.3V device, so *V<sub>out</sub>* on the PWM channels is 3.3 V (works for standard hobby servos)
*  5.5 mm barrel jack (center positive) for powering servos
*  Can be used to control anything that needs PWM (speed controllers, gate drivers-to-FETs-big LEDs, etc.)

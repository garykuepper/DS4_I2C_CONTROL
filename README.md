
# DS4_I2C_CONTROL
PS4 Dualshock I2C USB Host Controller library for Arduino microcontrollers.

This is a library for the Hobbytronics Microchip 24FJ64GB002-based USB Host Controller board pre-programmed for the bluetooth PS4 Dualshock Controller.
http://www.hobbytronics.co.uk/usb-host/ps3-ps4-controller-bluetooth
   
**SEMU Consulting 2017**, based on the Hobbytronics ps4_hex_tank example.

**GGBYTES**  Edits to allow static functions to decode ps4 data

## Connections:
   * 5V on board --> 5V on Arduino
   * 0V on board --> GND on Arduino
   * SDA on board --> pin A4 (SDA) on normal Arduino (SDA1 on Arduino Due)
   * SCL on board --> pin A5 (SCL) on normal Arduino (SCL1 on Arduino Due)
   * non-latching SPST push switch between 0V and SS pin on board to enable bluetooth pairing mode (blue LED flashes faster)
   * [compatible bluetooth controller](http://www.hobbytronics.co.uk/bluetooth-4-dongle) inserted into USB connection on board
   * In some installations (e.g. when stacking multiple I2C boards), it may be necessary to add 4.7k pull-up resistors between the SCL and SDA lines and VCC **but check your specific microcontroller and I2C board datasheets before doing this. Inappropriate use of pull-up resistors may damage the board and/or microcontroller**.

To pair the PS4 Dualshock controller, press and release the push switch which connects the SS and 0V pins on the board. The blue LED on the board should start to flash faster. Then press the PS4 and Share buttons on the Dualshock controller simultaneously. The LED on the Dualshock should flash for a few seconds. Once paired, the Dualshock rumbles briefly and its LED turns to solid blue. If the Dualshock fails to pair, try pressing the reset button on the board and repeating the process.

The default I2C address of the board is 0x29 (decimal 41). This can be amended using the UART configuration instructions below.

**NB:** in order for the host controller board to work in I2C mode, it is necessary to turn off SERIAL mode. This needs to be done via an  direct UART (Rx/Tx) connection to the board, either using an [FDTI cable or breakout board](http://www.hobbytronics.co.uk/ftdi-basic), or via the Arduino itself.

## Temporary connections for UART configuration via Arduino 

**This configuration is only necessary if not using an FDTI cable or breakout board.**

   * 5V on board --> 5V on Arduino
   * 0V on board --> GND on Arduino
   * TX on board --> TX-> on Arduino
   * RX on board --> ->RX on Arduino
   * RESET pin on Arduino --> GND on Arduino (this is to bypass the Arduino's internal UART chip so we're talking directly to the host controller board)

## Instructions for UART configuration via Arduino 

**e.g. to turn off Serial mode or amend the I2C address on the host controller board:**

   1. Remove the bluetooth dongle from the host controller
   2. Open up the Serial Monitor in the Arduino IDE and set the line mode to "Carriage return" and the baud rate to whatever rate the Arduino was using (recommend 115200).
   3. Enter the command HELP in the command line and click Send (if HELP doesn't work, try ? instead). 
   4. You should see a report of the current settings of the host controller board. By default, Serial mode is ON.
   5. Enter the command SERIAL OFF and click Send.
   6. Then type HELP (or ?) again and you should see that SERIAL mode is now OFF.
   7. If necessary, the default I2C address can be changed in a similar fashion.
   8. Now turn everything off, reinsert the bluetooth dongle into the host controller and reconnect as per the normal connections shown above.

<!-- START COMPATIBILITY TABLE -->

## Compatibility:

MCU                | Tested Works | Doesn't Work | Not Tested  | Notes
------------------ | :----------: | :----------: | :---------: | -----
Atmega328 @ 16MHz  |      X       |             |            | 
Atmega328 @ 12MHz  |              |             |     X      | 
Atmega32u4 @ 16MHz |      X       |             |            | 
Atmega32u4 @ 8MHz  |              |             |     X      | 
ESP8266            |              |             |     X      | 
Atmega2560 @ 16MHz |      X       |             |            | 
ATSAM3X8E          |      X       |             |            | Use SDA1/SCL1
ATSAM21D           |              |             |     X      | 
ATtiny85 @ 16MHz   |      X       |             |     X      | 
ATtiny85 @ 8MHz    |      X       |             |     X      | 
MK20DX256VLH7 @ 72MHz |      X       |             |            |
Intel Curie @ 32MHz |             |             |     X       | 
STM32F2            |             |             |     X       | 

  * ATmega328 @ 16MHz : Arduino UNO, Adafruit Pro Trinket 5V, Adafruit Metro 328, Adafruit Metro Mini
  * ATmega328 @ 12MHz : Adafruit Pro Trinket 3V
  * ATmega32u4 @ 16MHz : Arduino Leonardo, Arduino Micro, Arduino Yun, Teensy 2.0
  * ATmega32u4 @ 8MHz : Adafruit Flora, Bluefruit Micro
  * ESP8266 : Adafruit Huzzah
  * ATmega2560 @ 16MHz : Arduino Mega
  * ATSAM3X8E : Arduino Due
  * ATSAM21D : Arduino Zero, M0 Pro
  * ATtiny85 @ 16MHz : Adafruit Trinket 5V
  * ATtiny85 @ 8MHz : Adafruit Gemma, Arduino Gemma, Adafruit Trinket 3V
  * MK20DX256VLH7 @ 72MHz : Teensy 3.x (not overclocked)

<!-- END COMPATIBILITY TABLE -->

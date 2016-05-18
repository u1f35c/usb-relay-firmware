This is a basic firmware for the cheap USB relay boards that are sold on eBay
and report themselves as being from www.dcttech.com. They're based on an Atmel
ATtiny chip and run [V-USB](https://www.obdev.at/products/vusb/) as the core of
their firmware. I wanted to play around with some ATtiny programming so figured a good way to start was to build a new firmware for the device I already had that mimicked the existing firmware. Currently it only supports the 1 port board, but I'm sure it could be easily extended by anyone with access to the boards with more relays on them. Patches welcome.

`apt install avr-libc avrdude` should install the appropriate build requirements on Debian (`gcc-avr` will be automatically pulled in) assuming you already have `build-essential` installed for `make`.

`make` will then build you a main.hex which you can program to your device using
`avrdude`. With my [Bus Pirate](http://dangerousprototypes.com/docs/Bus_Pirate) I use:

    avrdude -p attiny45 -c buspirate -P /dev/ttyUSB0 -U flash:w:main.hex:i

Pin outs on the 1 port relay board look like:

<pre>
  |RELAY|
  +-----+               [Bus Pirate connections]
           o RESET      CS (white)
           o SCK        CLK (purple)
   +--+    o MISO       MISO (black)
   |AT|    □ MOSI       MOSI (grey)
   +--+
           □ 5V         +5V (orange)
           o GND        GND (brown)
  | USB |
  +-----+
</pre>

USB D- is connected to Port B pin 1, USB D+ to Port B pin 2 (INT0) and the relay is hanging off Port B pin 3.

Given that V-USB is GPLv2+ or commercial all of my code is released as GPLv3+, available at [https://the.earth.li/gitweb/?p=usb-relay-firmware.git;a=summary](https://the.earth.li/gitweb/?p=usb-relay-firmware.git;a=summary) or on GitHub for easy whatever at [https://github.com/u1f35c/usb-relay-firmware](https://github.com/u1f35c/usb-relay-firmware)

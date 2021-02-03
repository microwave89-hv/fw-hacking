# Preparation
1. Get ISP-capable SPI programmer with flashrom (e.g. Raspi) & Pomona SOIC8/16 clip.
2. Connect programmer's MISO to MOSI over a 470 ohms resistor.
3. Have the following python script test the programmer's SPI functionality:
```
#/usr/bin/env python2

import spidev

spi=spidev.SpiDev()
spi.open(bus=0,device=0)
spi.max_speed_hz=500000

data=spi.xfer2([0x00, 0xff, 0xaa, 0x55, 0xcc, 0x33, 0x3c, 0xc3])

hexx_str = ''
for i in data:
        hexx_str += hex(i).replace('0x','') + ' '

print hexx_str
```
  * MISO over resistor to MOSI: Must read 0x00, 0xff, 0xaa, 0x55, 0xcc, 0x33, 0x3c, 0xc3
  * MISO over resistor to GND: Must read all zeroes
  * MISO over resistor to VCC: Must read all ff's

# Connection
1. Connect programmer's MISO MOSI SCK !CE over 215 ohms resistors to corresponding pins on the Pomona clip (as if it was attached to the SPI ROM right now).
2. Connect programmer's GND without any resistor to corresponding pin on the Pomona clip.
3. Connect programmer's VCC over a 215 ohms resistor to !HOLD pin on the Pomona clip.
4. Connect green 20 mA LED over 820 ohms {citation} resistor from Pomona MISO to GND

# Read Verification
1. Put Mac to sleep but don't disconnect the battery!
2. Carefully clip on Pomona clip to ROM, noting the exact chip designation (e.g. "MX25L6406E")
3. Issue 3 times ```$ flashrom --programmer linux_spi:dev=/dev/spidev0.0,spispeed=5000 -c MX25L6406E/MX25L6408E -VVV -r ./romN.bin``` | N € (1, 3), and wait for completion each time.
4. Compare sha256sum hashes of those 3 files and repeat reading (perhaps with lower clock frequency) until they are 3 times the same.
5. __Unclip Pomona clip!__
6. Ensure that Mac returns from its sleep just fine
7. Backup one of the readouts to somewhere safe, for instance to your controlling Mac:
```
Control-MacBookPro$ scp pi@raspberrypi.local:readN.bin ./
```

# Brick
1. Put Mac back to sleep as before
2. Carefully clip on Pomona clip to ROM
3. Read 3 times as before, and compare readouts to each other in order to verify electrical connection
4. Erase entire ROM: ```$ flashrom --programmer linux_spi:dev=/dev/spidev0.0,spispeed=5000 -c MX25L6406E/MX25L6408E -VVV -E```
5. __Unclip Pomona clip!__
6. Press any key on the victim Mac and *see how your Mac completely crashes if it tries to resume from sleep XD*
7. Try to turn on and off your Mac a few times, note how it doesn't even want to stay turned off when releasing the power key shortly after powering down!

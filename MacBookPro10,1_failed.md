# How NOT flash MBP10,1 BIOS ROM chip with external programmer

# DISCLAIMER: IT GOES WITHOUT SAYING THAT BELOW PROCEDURE IS RIDICULOUSLY DANGEROUS AND THE TINIEST MISTAKE MIGHT PHYSICALLY DAMAGE YOUR MACBOOK PRO HARDWARE! THESE STEPS DID NOT WORK FOR ME AND HAVE PROVEN TO (TEMPORARILY) BRICK MBP10,1 (Mid-2012 15" Retina)!

# Preparation
0. (Catalina on controller MacBook Pro, (any) on MBP10,1)
1. Get Raspberry Pi 3B+
2. Download Raspberry Pi Imager (imager_1.8.5.dmg)
3. In Imager select 64 bit Raspberry Pi OS (that internally means e.g. Linux 6.6.51+rpt-rpi-v8 (aarch64) and flashrom v1.3 neded to accept e.g. "SST26VF064B(A)" flash)
4. Make sure ssh access is configured in installation options and make sure username is "pi" and password "raspberry"
5. Start installation (e.g. on micro SD card inserted into SD card adaptor inserted into SD card reader
6. SSH into raspi according to "fw-hacking/flasher.md"
7. Prepare "linux_spi" programmer according to "fw-hacking/flasher.md"
8. Wire up raspi to something like a SOIJ8/SOIC8/SOP8 breakout board, consider flash datasheet and "https://www.researchgate.net/profile/Manikandan-L-C/publication/346085998/figure/fig2/AS:1003233170444288@1616200835027/Raspberry-Pi-3-B-Pin-out-Diagram-From-Fig-3-out-of-40-pins-26-are-used-as-a-digital.png"
9. Have the following python script test the programmer's SPI functionality:
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

# Read old flash
1. Shutdown MBP10,1 if running
2. __DISCONNECT BATTERY AND CHARGER FROM MBP10,1!!!__
3. Unsolder flash from MBP10,1
4. Solder flash to breakout board
5. Issue 3 times ```$ flashrom --programmer linux_spi:dev=/dev/spidev0.0,spispeed=5000 -c MX25L6406E/MX25L6408E -VVV -r ./romN0.bin``` | N0 € [0, 2], and wait for completion each time.
6. Compare sha256sum hashes of those 3 files and repeat reading (perhaps with lower clock frequency) until they are 3 times the same.

# Write new flash
1. Unsolder MBP10,1 flash from breakout board
2. Solder new flash (e.g. __"SST26VF064B(A)"__) to breakout board
3. Flash the new foreign chip by ```$ flashrom --programmer linux_spi:dev=/dev/spidev0.0,spispeed=5000 -c "SST26VF064B(A)" -VVV -w ./romN0.bin```
4. Unsolder new flash chip from breakout board

# Install new flash
1. Solder new flash chip into MBP10,1
2. Connect battery and or charger
3. Press power
4. Observe fans spinning but MBP10,1 not starting.

# Install original flash chip (not tested)
1. Press power button long to shutdown MBP10,1
2. __DISCONNECT BATTERY AND CHARGER FROM MBP10,1!!!__
3. Unsolder new flash chip from MBP10,1
4. Solder original flash into MBP10,1
5. Connect battery and or charger
6. Press power
7. MBP10,1 should start normally

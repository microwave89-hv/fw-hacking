# Hastily written notes..

* for i in {150..169}; do ping -c 2 192.168.2.$i | grep ‘bytes’ ; done

* press | enter to get `'s

* https://galem.wordpress.com/2014/10/14/configuring-the-raspberry-pi-to-share-a-macs-internet-connection/

* https://pihw.wordpress.com/guides/direct-network-connection/

* ssh pi@raspberrypi.local ( = "ping raspberrypi.local (need mDNS (==> avahi, Bonjour))" + ssh pi@returned-ip-address-by-ping)

* new raspbian ==> flashrom already installed

* modprobe: uses proper linux version, and loads deps

* sudo vi /boot/config.txt (uncomment #dtparam=spi=on)

* sudo shutdown --reboot

* sudo modprobe spi_bcm2835

* sudo modprobe spidev

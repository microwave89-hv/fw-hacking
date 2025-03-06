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



* OR

* /Users/microwave89hv/Documents/workbench/project-hwa113/flasher/theory/ConfiguringtheRaspberryPitoshareaMacsinternetconnection_MartinGalesblog.html

* /Users/microwave89hv/Documents/workbench/project-hwa113/flasher/theory/GuideToDirectNetworkConnection_MeltwatersRaspberryPiHardware.html

* - macOS network settings ==> Thunderbolt bridge,TCP/IP ==> Configure IPv6: Automatically

* - figure which interface is Thunderbolt bridge ==> ping6 ff02::1%<thunderbolt_interface_name>

* - figure which address is not your control Mac (it will appear little later in ping answer than your Mac)

* - ssh pi@<that_ipv6_addr>%<thunderbolt_interface_name>

(user: pi, passwd: raspberry)

# Using second method (ipv6!) makes it difficult to get Internet on raspi

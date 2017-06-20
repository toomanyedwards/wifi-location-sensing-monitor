# Convective Core Wifi Location Sensing Monitor

This is the repo for the monitor component of the Convective Core Wifi Location Sensing application. Monitors measure wifi signal strength from wifi clients and send this information to the Convective Core Wifi Location Sensing server. The server uses signal strength information from three monitors and performs [trilateration](https://en.wikipedia.org/wiki/Trilateration) to determine the location of wifi clients in range.

## Setup

This section describes the setup of monitor.  Monitors leverage the [OpenWrt](https://openwrt.org/) Linux distribution for embedded devices. First, a device capable of wifi monitoring is flashed with the OpenWrt operating system, then it is configured to be a client of a well known access point, finally the appropriate Convective Core Wifi Location Sensing monitor files are copied and configured on the monitor and it is restarted beginning the monitor process. Each of the these steps is described in detail below.

## Flashing OpenWrt

*Note: The following instructions apply to flashing OpenWrt on [Rav Power WD-03](https://www.ravpower.com/rp-wd03-filehub-6000mah-power-bank-portable-wireless-router.html) devices. These mobile wifi devices were used for the initial version of this project because of their portable size and battery power. Limitations of these devices are that they only have one radio. This requires that the monitoring channel is locked to the same channel that is used by the wifi client. Also, the radio is 2.4Ghz only. Future monitors may leverage devices with multiple radios and or DSPs and software radios to overcome the channel locking and frequency limitations.*

As described [here](https://forum.openwrt.org/viewtopic.php?id=60360), the WD-03 uses the same chipset as the [HooToo TM-05](https://www.hootoo.com/hootoo-tripmate-ht-tm05-wireless-router.html) so the [instructions for flashing OpenWrt](https://wiki.openwrt.org/toh/hootoo/hootoo_ht-tm05) onto a TM-05 can be followed to flash OpenWrt onto the WD-03. Specifically:

1. Download [this](http://www.gl-inet.com/firmware/mt300n/clean/openwrt-gl-mt300n-clean-1.0.bin) OpenWrt firmware.
2. Place it in the root of a clean TFTP server running on your computer.
* *Note: The TFTP client for Windows found [here](https://tftpd64.codeplex.com/) was used to flash the WD-03 monitors.*
3. Rename the image to kernel — be sure there is no file extension, and that there are no other files on the TFTP server.
4. Plug the WD-03 into your computer via ethernet.
5. Set your computer to use 10.10.10.254 as its IP address.
* *Note: The steps for assigning a staic IP to w Windows machine are described [here](https://www.howtogeek.com/howto/19249/how-to-assign-a-static-ip-address-in-xp-vista-or-windows-7/) *
6. With your WD-03 shut down, hold down the power button until the first white LED lights up.
7. Push and hold the reset button and release the power button. Continue holding the reset button for 30 seconds or until it begins searching for files on your TFTP server, whichever comes first.
8. The WD-03 will look for your computer at 10.10.10.254 and install the kernel file. Once it has finished installation of the kernel file, it will search for a (nonexistent) rootfs file — when it begins searching for this file, shut down the WD-03 by holding the power button normally.
9. Start up your WD-03 normally. When the Wi-Fi indicator turns green, OpenWrt has booted successfully.
10. OpenWrt uses 192.168.1.1 as the default router IP address, so you will need to switch your computer back to a dynamic IP address, or change your static IP address to 192.168.1.X to configure OpenWrt.


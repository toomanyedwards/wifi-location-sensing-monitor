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
* *Note: The steps for assigning a staic IP to w Windows machine are described [here](https://www.howtogeek.com/howto/19249/how-to-assign-a-static-ip-address-in-xp-vista-or-windows-7/).*
6. With your WD-03 shut down, hold down the power button until the first white LED lights up.
7. Push and hold the reset button and release the power button. Continue holding the reset button for 30 seconds or until it begins searching for files on your TFTP server, whichever comes first.
8. The WD-03 will look for your computer at 10.10.10.254 and install the kernel file. Once it has finished installation of the kernel file, it will search for a (nonexistent) rootfs file — when it begins searching for this file, shut down the WD-03 by holding the power button normally.
9. Start up your WD-03 normally. When the Wi-Fi indicator turns green, OpenWrt has booted successfully.
10. OpenWrt uses 192.168.1.1 as the default router IP address, so you will need to switch your computer back to a dynamic IP address, or change your static IP address to 192.168.1.X to configure OpenWrt.

## Configure as Access Point Client

The following steps describe configuring the monitor a wifi access point client. This is required so that signal strength information can be forwarded to the Convective Core Wifi Location Sensing server.

1. Login to OpenWrt on 192.168.1.1 (root/no password)
2. Choose System>Administration
3. Set a password for root user
4. Choose Network>Wifi>Scan
5. Click “Join Network” for “CCNet_AP”
* *Note: CCNet_AP is a well known access point for all Convective Core Wifi Location Sensing Monitors*
6. Browse to Join Network>Settings
7. Set WPA passphrase: 5124238782
8. Set firewall zone to lan
9. Click “Submit”
10. Click “Save and Apply”
11. Wireless should now be connected and show signal strength
12. To verify setup. Telnet into 192.168.1.1 and ping a well known domain (e.g. - cnn.com)

## Configuring the Monitor Interface

The following section describes how to configure the wifi monitor interface that is used for monitoring network traffic.

1. Browse to Network>Wifi>Add
2. Select "Monitor" mode
3. Select "lan" network
4. Click "Save and Apply"

## Installing tcpdump

Convective Core Wifi Location Sensing Monitors require that *tcpdump* be installed on the monitor machine. The following describes the installation steps.

1. SSH into 192.168.1.1
2. Run: opkg update
3. Run: opkg install tcpdump
4. Verify the installation by running: *tcpdump -i wlan0-1"*
5. tcpsump should start and you should see the network traffic captured by the monitor interface

## Installing curl

Convective Core Wifi Location Sensing Monitors require that *curl* be installed on the monitor machine. The following describes the installation steps.

1. SSH into 192.168.1.1
2. Run: opkg update
3. Run: opkg install curl

## Installing the Startup Scripts

Convective Core Wifi Location Sensing Monitors include several scripts that run automatically when the monitor is started. These scripts monitor wifi tcp traffic and forward the appropriate data in json format to the server. This section describes how to install the scripts.

1. SSH into 192.168.1.1
2. Copy the files from the scripts/etc folder from the github repo https://github.com/toomanyedwards/wifi-location-sensing-monitor to the /etc folder on the monitor
3. Use chmod +x on each copied script to ensure that it is executable
4. Restart the monitor device
5. You should start seeing traffic on the Convective Core Wifi Location Sensing Server's debug page for devices on the same channel as the monitor


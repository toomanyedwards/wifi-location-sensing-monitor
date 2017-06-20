# Convective Core Wifi Location Sensing Monitor

This is the repo for the monitor component of the Convective Core Wifi Location Sensing application. Monitors measure wifi signal strength from wifi clients and send this information to the Convective Core Wifi Location Sensing server. The server uses signal strength information from three monitors and performs [trilateration](https://en.wikipedia.org/wiki/Trilateration) to determine the location of wifi clients in range.

## Setup

This section describes the setup of monitor.  Monitors leverage the [OpenWrt](https://openwrt.org/) Linux distribution for embedded devices. First, a device capable of wifi monitoring is flashed with the OpenWrt operating system, then it is configured to be a client of a well known access point, finally the appropriate Convective Core Wifi Location Sensing monitor files are copied and configured on the monitor and it is restarted beginning the monitor process. Each of the these steps is described in detail below. 

## Flashing 


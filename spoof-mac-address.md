Spoof MAC Address
=================

Useful for getting around public wifi time limits, e.g. at the airport.

## Steps

### 1/ Figure out which interface is connected to the internet

Go to `System Preferences > Network > Wifi > Advanced > Hardware`.
That's your current MAC.

You need to find the interface name associated with that MAC so you can
manually change it. Start with `en0` (common defaul for macOS setups):

    $ ifconfig en0 | grep ether

Check that the MAC matches the one in System Preferences.

If not, try other interfaces e.g. `en1`, `en2` ...

### 2/ Generate a New, Random MAC to Use

    $ openssl rand -hex 6 | sed 's/\(..\)/\1:/g; s/.$//'

### 3/ Disconnect from Wifi

This is important!

### 4/ Reset the interface from step 1 with the MAC from step 2

  $ sudo ifconfig en0 XX:XX:XX:XX:XX:XX

Change `en0` to whatever interface you determined in step 1.

Substitute the MAC generated in step 2 in for `XX:XX:XX:XX:XX:XX`

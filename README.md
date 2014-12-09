Hardware Simulation
===================

Simulate the low-level hardware functions - used for testing Mother on VirtualBox.

The Mother component on Beoga Beag is comprised of an ALIX 3d2 board from
[PC Engines](http://www.pcengines.ch/).
The board has an LX800 processor, 256MB of RAM, two USB ports, a serial port, and
two miniPCI slots.

One of the miniPCI slots is used for an Atheros CM9 dual-band WiFi radio.
This is usually configured on the 5.8GHz band.

The serial port connects to Igor and the two USB slots are used for a
USB GPS and the RockBlock (satellite comms).

The operating system is nanoBSD, version 8.3.
The kernel is scaled down to remove a lot of the extraneous drivers and components.
Likewise, the file system is scaled back to remove dross.
The whole thing fits on a Compact Flash card, usually either 4GB or 8GB, depending on
storage.
The root filesystem takes up around 600MB.

```bash
# uname -a
FreeBSD wrap-app 8.3-RELEASE FreeBSD 8.3-RELEASE #0: Mon Apr  9 21:47:23 UTC 2012     root@almeida.cse.buffalo.edu:/usr/obj/usr/src/sys/GENERIC  i386
```

To simulate the boat navigation, a similar image is used,
running on [VirtualBox](https://www.virtualbox.org/).
Due to the nature of VirtualBox versus the real board, the full Generic
kernel is used and some of the boot options are disabled.

This particular directory houses the boat simulation system.

VirtualBox is configured so that the Mother image has three serial ports.
One to talk to the two Atmel boards, one for the GPS and one for the RockBlock.
These are wired to UNIX-domain pipes on the host file system.
Specifically, `/tmp/nano_sio0` is the serial port for talking to the Atmel,
`/tmp/nano_sio1` has the GPS data, and `/tmp/nano_sio2` is the RockBlock.

This simulator attempts to mimic the operation of all three systems, in unison.

Movements to the rudder and navigation will be simulated using a fake polar for the boat,
which will then update the GPS location every second.

The simulator outputs the GPS data into a KML file so that the simulation
output can be mapped onto Google Earth.

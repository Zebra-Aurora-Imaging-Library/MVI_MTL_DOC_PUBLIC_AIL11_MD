---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_under_Linux
section: Additional_Linux_notes_Boards_and_drivers
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / Linux / Additional Linux notes Boards and drivers"
---

# Miscellaneous user guide information for Linux: Boards and drivers

This section includes additional user guide information for Linux regarding boards and drivers. The information found in this section might be a reiteration of content previously documented.

> **Note:** The XEN kernels are not supported.

## Zebra Radient

Zebra Radient has 1, 2, or 4 serial interfaces (each controlled by a UART); the number of serial interfaces depends on the version of the board.

By default, the standard Linux serial port driver does not properly identify the Zebra Radient serial interfaces. They will therefore not be available as /dev/ttyS* as ordinary serial port. The Radient or Radient eV-CL driver will create /dev/RadientS* or /dev/RadienteVCLS* device which can be used like any ttyS*.

## Zebra GigE Vision

The following subsections discuss using Zebra GigE Vision with Linux.

### Bandwidth optimization

If you are using a high bandwidth camera, you should raise the MTU (Ethernet packet size) value above the 1500 default. The optimal value is dependent on your machines and network configuration. You can change this in NetworkManager on Ubuntu. You can also use either of these two commands:

```
# ifconfig eth0 mtu 9000
 # ip link set eth0 mtu 9000
```

You must change eth0 for whichever interface your camera is connected to and 9000 for the value you want for the MTU.

When running in a debugger, the camera heartbeat will be disabled (if supported) or increased to 5 minutes.

### Firewall

If your firewall is enabled, you will need to allow UDP packets coming from port 3956 (gvcp) to pass or Aurora Imaging Library will not be able to detect cameras on your network. To do so, you can issue this command:

```
# iptables -I INPUT 1 -p udp --sport gvcp -j ACCEPT
```

This command will need to be done at every reboot. You will need to consult your distro's documentation to make it permanent.

### Multiple interfaces, in general

If you have more than one interface on the same subnet, as is the case when using multiple cameras with auto-ip (a 169.254.X.X address), you will need to configure a bridge between those interfaces.

### Multiple interfaces, in Ubuntu

Under Ubuntu, to make the change permanent you need to change /etc/network/interfaces with these lines:

```
iface eth3 inet manual
iface eth4 inet manual
```

```
auto br0
iface br0 inet static
     bridge_ports eth3 eth4
     address 169.254.5.67
     broadcast 169.254.255.255
     netmask 255.255.0.0
     bridge_fd 0
```

You can have more than two interfaces in a bridge and you should change eth3 and eth4 for whichever interfaces your cameras are connected to. Your bridge doesn't have to be named br0. To make those change active you will need to restart your networking or reboot.

## Video4Linux2 (V4L2) and Zebra USB3 Vision driver

Aurora Imaging Library supports compatible Video 4 Linux 2 devices (some V4L2 image formats might not be supported, and some cameras might not have any supported formats). Multiple cameras with autofocus/autolighting or similar features will require grabbing a few frames before obtaining a clear picture. Feature browser does not support V4L2.

Only one process can use a USB3 Vision system at a time. Some other programs, like camera manufacture software, can interfere with the usage of the USB3 Vision camera.

## Zebra Clarity UHD

Zebra Clarity UHD is compatible with distributions other than Ubuntu 24.04.

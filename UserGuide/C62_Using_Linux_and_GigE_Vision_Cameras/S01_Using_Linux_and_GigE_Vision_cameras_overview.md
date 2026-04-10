---
doctype: UserGuide
part: "Miscellaneous"
chapter: Using_Linux_and_GigE_Vision_Cameras
section: Using_Linux_and_GigE_Vision_cameras_overview
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / using-linux-and-gigE-vision-cameras / Using Linux and GigE Vision cameras overview"
---

# Using Linux and GigE Vision cameras overview

GigE Vision™ cameras are designed for high-bandwidth and Ethernet-based data transfer. Proper configuration of the network is vital to achieving reliable camera operation, reducing packet loss, and minimizing latency. This section introduces the key concepts for configuring GigE Vision™ cameras on Linux systems, covering IP addresses, netmasks, and private network addresses.

## Conventions

The conventions used in this chapter are listed below:

- Commands prefixed with # require sudo.
- Commands prefixed with $ can be used by an unprivileged user.
- Commands, command-line output, and configuration files are written in monospace.
- Menu items and button text are bold.
- Most command examples include eth0 as a representation of the network interface name. It should be changed to the correct name on your particular setup.
- The information in this chapter is accurate for Ubuntu 24.04.

## Important network concepts

Each device on a network must have a valid Internet protocol (IP) address to communicate with other devices on the same network. There are two types of IP addresses:

- IPV6 addresses are not currently supported by the GigE Vision™ standard.
- An IPV4 address is a 32-bit number, usually written as four numbers, each separated by a dot (such as, 169.254.4.2). Each number has a range from 0 to 255, but multiple numbers at the start and end of that range are used by default for special devices or protocols. An IP address can be obtained dynamically with a Dynamic Host Configuration Protocol (DHCP) request, link local negotiation, or set statically. Typically, all devices on a network use the same method for acquiring an IP address for each device.

The following is a list of networking terms that are used frequently with IP networks:

1. A netmask is a bit mask for the IP address. The netmask is used to show the amount of the address that doesn't change in relation from one section of a network (a subnet) to another. Routers will not transmit packet addresses from one subnet to another, and devices on a subnet will not answer to packets from a different subnet. Netmasks are written as either an IP address (for example, 255.255.0.0) or a /16 (where 16 is the number of bits in the IP address which don't change between subnets).
2. The link local addresses (sometimes called Auto IP, Auto Configuration, or Zeroconf) are a group of IP addresses ranging from 169.254.0.0 to 169.254.255.255 (in the subnet /16 or 255.255.0.0). These are the typical addresses for your camera when it is directly plugged into your computer. Typically, to acquire an IP in this subnet, a device sends a request to all connected devices. If its randomly chosen IP address is not in use (and no device answers the request), your device will use its randomly chosen IP address.
3. The private network addresses are 10.0.0.0/8, 172.16.0.0/12, and 192.168.0.0/16. They are called private because no router will move packets addressed to those IP addresses through to the Internet. Private network addresses are typically distributed by a DHCP server running on the subnet. Your organization probably uses private network addresses, allowing you to configure your cameras within this range and stay off the Internet. To run a DHCP server on your computer, contact your IT coordinator or system administrator.

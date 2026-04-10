---
doctype: BoardSpecificNotes
part: ""
chapter: GigE_Vision
section: GigE_Vision_network_adapters
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / gige / GigE Vision network adapters"
---

# Configuring IP addresses

There are three ways to configure the IP address of your Gbit Ethernet network adapter(s):

- **Using DHCP** (corporate network). DHCP relies on a network server to automatically assign a unique IP address to each network device. If using DHCP, you do not need to configure your network devices.
- **Using a point-to-point connection**. A point-to-point connection (link-local addressing or LLA, or AutoIP) relies on your computer to generate a unique IP address for your computer's network adapter and connected GigE Vision cameras on the 169.254 subnet (that is, an IP address starting with 169.254.X.X).
- **Using a static (or persistent) IP address**. A static IP address is used when there is no DHCP available, and you want control over the IP address used. You can also use a static IP address to reduce the time it takes to assign all the associated devices' IP addresses. If your computer uses a static IP address, other connected devices on your network should also use a static IP address.

A device might become inaccessible if the IP address used is incompatible with its network segment. In most cases, Aurora Imaging Library identifies this issue and assigns a valid IP address. However, there are situations where the library cannot resolve configuration issues, particularly when the network segment in question has a DHCP service. The allocated IP address can lead to problems if the DHCP server assigns the same IP address to a different device, which causes an IP address conflict and the device becomes inaccessible. The GigE Vision driver will identify these devices as "unreachable", and in Aurora Imaging Capture Works, these devices will be denoted with a red icon.

You can resolve this issue using the GigE Vision device IP configuration tool in Aurora Imaging Capture Works. To do so, right-click on the device and select **IP resolve**. This will temporary assign a valid IP address to the device, making it addressable, and the device's IP configuration will be set to a mode that is compatible with its network segment.

Then, it will send a command requesting the device to initiate its IP configuration cycle using the new configuration mode. If a DHCP server is available, the device will request an IP address using the DHCP method, which will properly configure the device. Alternatively, you can access the **Tools GigE Vision Device IP Configuration** menu item for more control over how to resolve the conflict.

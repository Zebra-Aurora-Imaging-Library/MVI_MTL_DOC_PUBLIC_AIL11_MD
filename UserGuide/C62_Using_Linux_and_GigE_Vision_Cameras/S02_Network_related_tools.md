---
doctype: UserGuide
part: "Miscellaneous"
chapter: Using_Linux_and_GigE_Vision_Cameras
section: Network_related_tools
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / using-linux-and-gigE-vision-cameras / Network related tools"
---

# Network related tools

This section describes useful network related tools.

## Network configuration

Ubuntu uses NetworkManager by default. NetworkManager is an ensemble of programs and configuration files used to detect and configure networks. NetworkManager provides two configuration programs; one is graphical (nm-connection-editor) and one is console based (nmtui-edit). We recommend using nm-connection-editor. Both configuration programs are available on Ubuntu.

### nm-connection-editor

nm-connection-editor typically doesn't have a system menu option. To start it, you must open a terminal and enter the following command:

```
$ nm-connection-editor 
```

The + button allows you to add a bond or team. The – button allows you to remove an interface. The cog button allows you to change the current configuration of an interface.

*[Image: nm_connection_editor.png]*

### nmtui-edit

nmtui-edit doesn't have a system menu option as it is a console program. To start it, you must open a terminal and enter the following command:

```
$ nmtui-edit 
```

You can use the Tab key or Shift+Tab to navigate the interface. You can also use the arrow keys to move the highlight between some (but not all) elements. The current selection is highlighted in red. You must press Enter to activate the selection.

*[Image: nmtui_edit.png]*

## Firewall

On Ubuntu, the firewall is disabled by default and there is no GUI to configure it.

If your firewall is enabled, you must allow UDP packets coming from port 3956 (gvcp) to pass or Aurora Imaging Library will not be able to detect cameras on your network. To do so, you can issue the following command:

```
# iptables -I INPUT 1 -p udp --sport gvcp -j ACCEPT 
```

This command must be executed at every reboot.

## Network status

There are multiple ways to view the current status of the network. The most useful is probably the ip command.

### ip

Running the following command in a terminal will give you a complete view of the current running network:

```
$ ip address
```

The output should be similar to the following:

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp12s0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc mq state DOWN group default qlen 1000
    link/ether 00:20:fc:32:32:b4 brd ff:ff:ff:ff:ff:ff
3: enp13s0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc mq state DOWN group default qlen 1000
    link/ether 00:20:fc:32:32:b5 brd ff:ff:ff:ff:ff:ff
4: enp0s25: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 18:66:da:1a:c0:e7 brd ff:ff:ff:ff:ff:ff
    inet 10.100.202.54/24 brd 10.100.202.255 scope global dynamic noprefixroute enp0s25
       valid_lft 689061sec preferred_lft 689061sec
    inet6 fe80::1115:580e:f86:6c36/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
5: enp14s0: <BROADCAST,MULTICAST,SLAVE,UP,LOWER_UP> mtu 9000 qdisc mq master bond0 state UP group default qlen 1000
    link/ether ca:a3:4d:09:93:e6 brd ff:ff:ff:ff:ff:ff permaddr 00:20:fc:32:32:b6
6: enp15s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9000 qdisc mq state UP group default qlen 1000
    link/ether 00:20:fc:32:32:b7 brd ff:ff:ff:ff:ff:ff
    inet 172.16.0.109/16 brd 172.16.255.255 scope global dynamic noprefixroute enp15s0
       valid_lft 3236sec preferred_lft 3236sec
    inet6 fe80::9365:2bce:6478:611a/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
7: enp8s0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc fq_codel state DOWN group default qlen 1000
    link/ether 00:20:fc:2a:03:28 brd ff:ff:ff:ff:ff:ff
8: bond0: <BROADCAST,MULTICAST,MASTER,UP,LOWER_UP> mtu 9000 qdisc noqueue state UP group default qlen 1000
    link/ether ca:a3:4d:09:93:e6 brd ff:ff:ff:ff:ff:ff
    inet 172.16.0.110/16 brd 172.16.255.255 scope global dynamic noprefixroute bond0
       valid_lft 3240sec preferred_lft 3240sec
    inet6 fe80::7eb5:29bb:8cb3:b542/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
9: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    link/ether 02:42:6c:9e:44:e0 brd ff:ff:ff:ff:ff:ff
    inet 172.26.0.1/16 brd 172.26.255.255 scope global docker0
       valid_lft forever preferred_lft forever
```

To make reading the output easier, you might want to adjust the size of your terminal to prevent line wrap. In this example, there are 9 interfaces. The first interface is always lo. The loopback interface is used to indicate the current computer and is used by local programs to communicate with local services. Each interface is defined by its device name, followed by flags between &lt;>, followed by attributes. The next line should describe the Ethernet layer associated with the interface. It might be followed by an additional device name (altname line). The inet line describes IPv4 details, while the inet6 line describes the IPv6 details.

Of particular interest for GigE Vision are the following flags and attributes:

- NO-CARRIER indicates no signal is detected on the interface; either the Ethernet cable is not connected or the camera at the end is not powered.
- MASTER/SLAVE* indicates whether the interface is an aggregated link or a member of one. In this case of a member, the attribute master will indicate which device is the aggregated link.
  *Note that although we no longer use these terms in our documentation, we retain their use here to accurately reflect the ip command output.
- LOWER_UP indicates the Ethernet link is working properly.
- mtu indicates the maximum size of Ethernet packets usable on this interface. See [Bandwidth optimization](S03_Bandwidth_optimization.md).
- state indicates whether the interface is usable or not. If you see an interface with a LOWER_UP flag but a DOWN state it is probably due to a configuration error. For example, a typo in a manually edited configuration file.

If you need to identify which interface corresponds to which Ethernet port on your computer, you can look at the NO-CARRIER and LOWER_UP flag. Connect a camera to each port one by one and look at which interface goes from NO-CARRIER to LOWER_UP. The IP address of each interface must match that of the connected camera as explained in [Important network concepts](S01_Using_Linux_and_GigE_Vision_cameras_overview.md).

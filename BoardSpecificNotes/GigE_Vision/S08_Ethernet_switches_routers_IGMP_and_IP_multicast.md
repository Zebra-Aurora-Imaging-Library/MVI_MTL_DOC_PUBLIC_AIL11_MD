---
doctype: BoardSpecificNotes
part: ""
chapter: GigE_Vision
section: Ethernet_switches_routers_IGMP_and_IP_multicast
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / gige / Ethernet switches routers IGMP and IP multicast"
---

# Ethernet switches, routers, IGMP, and IP multicast

When configuring your GigE Vision camera and Gbit Ethernet network adapter for multicasting, there are a few networking concepts with which you should be familiar. This section provides some background information for the requirements of a network designed to deliver a multicast service.

A network designed to deliver a multicast service can include the following hardware components:

- A multicast transmitter (for example, your GigE Vision camera).
- Ethernet routers.
- Ethernet switches.
- Host multicast receivers (for example, your Aurora Imaging Library GigE Vision applications with a controller, worker, or monitor multicast digitizer).

Each component fills a role. To deliver a multicast service, the Internet Group Management Protocol (IGMP) is used your local network.

## Internet Group Management Protocol (IGMP)

IGMP is a standard that specifies how a Host can register with a router to receive specific multicast traffic. IGMP operates between the client computer (such as, the computers running your Aurora Imaging Library GigE Vision applications using a controller, worker, or monitor multicast digitizer) and a local multicast router.

An Aurora Imaging Library digitizer allocated with [`M_GC_MULTICAST_CONTROLLER`](../../Reference/dig/MdigAlloc.md), [`M_GC_MULTICAST_WORKER`](../../Reference/dig/MdigAlloc.md), or [`M_GC_MULTICAST_MONITOR`](../../Reference/dig/MdigAlloc.md) will automatically send IGMP requests to a network router asking for membership to a multicast group. The network router will then issue periodic membership queries to the registered members of a multicast group. Upon reception of these queries, Aurora Imaging Library will respond with IGMP report messages. IGMP allows a router to correctly forward multicast traffic.

## Ethernet switches and multicast

To correctly establish packet forwarding, an Ethernet switch uses and maintains an address table. Each packet that enters an Ethernet switch is added to this table; the incoming packet's source MAC address is put in the address table along with the port from which the packet came. To know on which port to output the packet, the switch will lookup the packet's destination MAC address in the address table. If the address is found, the packet will be output on the appropriate port. If the destination MAC address is not found, the packet is flooded on all ports. Eventually the address table will allow the switch to avoid flooding ports because almost every packet received can be looked up in the table.

Unfortunately this mechanism causes Ethernet switches to flood the broadcast domain with multicast traffic. This is because a switch usually learns MAC addresses by looking into the source address field of all the Ethernet packets it receives. A multicast MAC address is never used as source address for a packet. Such addresses do not appear in the MAC address table, and the switch has no method for learning them. This consumes a lot of bandwidth since all multicast packets received on one port of the switch are output to all other ports of the switch.

The diagram below illustrates multicast datagrams transmitted over a network. The GigE Vision camera (A) is sending multicast IP data to clients B and C. The router correctly forwards the data to the networks of B and C because of the IGMP messages gathered. While the router correctly forwards the data to the client's networks, because of the IGMP messages gathered, the LAN switches flood the data to all PCs connected to them.

*[Image: BSN_GigE_Multicast_Diagrams.png]*

## IGMP snooping and Ethernet switches

To eliminate the flooding problem discussed previously, some Ethernet switches support IGMP snooping. With IGMP snooping, the switch intercepts IGMP packets and uses the information it gathers to establish appropriate forwarding rules for the multicast traffic.

*[Image: BSN_GigE_Multicast_Diagrams_02.png]*

IGMP snooping is disabled by default in LAN switches. To avoid your Ethernet switch causing issues with IP multicast flooding, you must enable IGMP snooping in your Ethernet switch using the managed (or other) interface of your switch.

## Using IP multicast without a router

IGMP operates between a computer and a local router. If your network does not have a multicast router, IGMP will fail since there is no source for IGMP membership queries. Some Ethernet switches support being the source for IGMP membership queries (IGMP querying). If you do not have a multicast router in your network, you must use an Ethernet switch that supports IGMP querying and IGMP snooping (typically, if one is supported, then they both are).

If both IGMP snooping and IGMP querying are supported by your Ethernet switch, IP multicast becomes possible without the need of a router. Make sure both IGMP snooping and IGMP querying are enabled in your switch since these features are typically disabled by default; enable them using the managed (or other) interface of your Ethernet switch. When IGMP querying is enabled, it sends out periodic IGMP queries that trigger IGMP report messages from computers that want to receive multicast traffic. IGMP querying works with IGMP snooping since your Ethernet switch listens to these IGMP queries to establish appropriate forwarding.

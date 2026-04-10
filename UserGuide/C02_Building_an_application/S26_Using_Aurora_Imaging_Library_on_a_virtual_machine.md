---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Using_Aurora_Imaging_Library_on_a_virtual_machine
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Using Aurora Imaging Library on a virtual machine"
---

# Using Aurora Imaging Library on a virtual machine

Aurora Imaging Library supports virtual machines. This section presents important considerations when using the library with a virtual machine, as software running on a virtual machine can behave differently than when it is running on a physical machine.

The following Aurora Imaging Library systems have been verified for compatibility on Windows and Ubuntu virtual machines, operating within VMware environments on both Windows and Ubuntu hosts. Other virtualization platforms and Linux distributions can also be compatible.

- USB3 Vision system.
- Video4Linux2 system.
- GigE Vision system.
- GenTL system.

License dongles functioned seamlessly with VMware. VMware is highly recommended over other virtualization platforms because of its optimal performance and reliability. It is also recommended to use a solid-state drive (SSD) instead of a hard disk drive (HDD) to maximize performance. The USB3 Vision and Video4Linux2 systems worked as expected on the virtual machines. Findings for the remaining systems are discussed below.

## System-specific findings

You might encounter limitations when using a GigE Vision system or GenTL system on a virtual machine. Due to the performance limitations of virtual machines, it is recommended to use lower-bandwidth cameras to reduce the risk of frame issues.

The following table summarizes the findings for GigE Vision systems on virtual machines:

| Host operating system | Configuration | Findings |
| --- | --- | --- |
|  |
| Windows | Enable interrupt modification, set the jumbo packet size to 9014 bytes, and assign 2048 receive buffers to your network adapter. | On both Windows and Ubuntu virtual machines, the acquisition might produce corrupted frames if the virtual machine's data handling capacity is exceeded. |
| Ubuntu | Configure the host MTU with the same MTU value as the virtual machine (9194). | On both Windows and Ubuntu virtual machines, the acquisition might produce corrupted frames if the virtual machine's data handling capacity is exceeded. |

The following table summarizes the findings for GenTL systems on virtual machines:

| Host operating system | Findings |
| --- | --- |
|  |
| Windows | On Ubuntu virtual machines, the acquisition might produce corrupted frames if the virtual machine's data handling capacity is exceeded. GenTL systems work as expected on Windows virtual machines. |
| Ubuntu | The acquisition might produce corrupted frames on Windows virtual machines and frame errors on Ubuntu virtual machines if the virtual machine's data handling capacity is exceeded. |

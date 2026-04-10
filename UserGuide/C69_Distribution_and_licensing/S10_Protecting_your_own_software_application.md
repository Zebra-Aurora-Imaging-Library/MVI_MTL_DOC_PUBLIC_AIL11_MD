---
doctype: UserGuide
part: "Miscellaneous"
chapter: Distribution_and_licensing
section: Protecting_your_own_software_application
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / licensing / Protecting your own software application"
---

# Protecting your own Aurora Imaging Library software application using a Zebra hardware fingerprint

You might want to protect your own Aurora Imaging Library software application using a third-party licensing package that requires a hardware fingerprint. In this case, you can use the hardware fingerprint of the Zebra Imaging board that your application uses. The Aurora Imaging Library hardware fingerprint is a numerical value, unique to the Zebra hardware component.

You can inquire about the fingerprint of your Zebra Imaging board using [`MappInquire`](../../Reference/app/MappInquire.md) with [`M_..._FINGERPRINT`](../../Reference/app/MappInquire.md). For example, to inquire the hardware fingerprint of your Zebra Rapixo CXP board, use [`M_RAPIXOCXP_FINGERPRINT`](../../Reference/app/MappInquire.md).

If the inquired Zebra hardware component is present and properly installed, the requested hardware fingerprint is returned. If it is not present or properly installed, 0 is returned.

If using an Aurora Imaging Library runtime dongle, it is possible to store multiple licenses on the dongle using WIBU-Systems' CodeMeter API. You can therefore store your own Aurora Imaging Library software application license and an Aurora Imaging Library runtime license on a single dongle that you can provide to your client.

---
doctype: UserGuide
part: "Miscellaneous"
chapter: Using_Linux_and_GigE_Vision_Cameras
section: Connecting_multiple_cameras
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / using-linux-and-gigE-vision-cameras / Connecting multiple cameras"
---

# Connecting multiple cameras

You can use multiple cameras with the same computer if it has multiple network interfaces. However, pay special attention to your network and/or camera's configuration. Problems can occur when the network stack considers all interfaces with the same netmask as part of the same network and only sends packets on one of them. Therefore, while all cameras can communicate with your computer, your computer can only communicate with one camera. There are four possible configurations to fix this problem:

1. Put each interface on a bridge. Using a bridge allows each camera to communicate with each other (and guarantees the cameras will not all take the same IP address). It also allows you to leave them in dynamic IP configuration. This method is not very effective as your computer will resend every packet from one camera to all other cameras connected to your computer. This method is supported on all Linux distributions.
2. Put each interface in a bond. A bond is more effective than a bridge, as your computer will not resend all packets from one camera to all other cameras connected to your computer. However, your cameras can't communicate with each other and might take the same IP address (which would prevent them from working properly). You must configure your cameras with static IP addresses to prevent that, or configure a DHCP server on your computer. Using a bond allows you to connect your camera in any Ethernet port member of the bond; you do not have to match each camera to a specific Ethernet port on your computer. If you configure your cameras to use static IP addresses in the link-local range, you can leave your computer in link-local configuration.
3. Put each interface in a team. Network teaming in the newest solution and not all distributions currently support it. It is very similar to a bond but is handled in a different manner in the kernel and is slightly more efficient.
4. Putting each Ethernet port into a specific subnet is a traditional way of dealing with this problem; it will work, but it is a tedious configuration. You need to configure both your cameras and your computer to use static IP. In addition, you must match each camera to a specific Ethernet port. If you plug your cameras into different computers, they will not work until you configure that computer with a static IP on the correct subnet, or reconfigure all your cameras to match the new computer.

Of the options available, we recommend using a bond on all Linux distributions which support it.

## Configuring a bond with nm-connection-editor

To configure a bond using nm-connection-editor, follow the steps outlined below:

1. Start nm-connection-editor.
2. Click on the + button at the bottom of the dialog, select **Bond** from the drop-down list, and click **Create**.
   *[Image: bond_nm_connection_editor_1.png]*
3. Click on the **Add** button, select the **Ethernet** option from the drop-down list, and click **Create**. On the **Editing bond port** dialog box's **Ethernet** tab, select the proper **Device** and change the **MTU** to a value as high as your interface can support, then click on the **Save** button.
   *[Image: bond_nm_connection_editor_2.png]*
4. Repeat step 3 once for each camera to be added to the bond.
5. In the **Editing Bond connection** dialog box, set the bond **Mode** to **Broadcast** from the drop-down list, and the **MTU** to the same value as the MTU specified in step 3. In some versions of NetworkManager, the default mode appears to be broadcast. This is a display bug as the default is always round-robin. For the bond to function properly, you must set the mode to something else, save the configuration, and then set it to broadcast.
   *[Image: bond_nm_connection_editor_3.png]*
6. Select the **IPv4 Settings** tab and set the **Method** to the **Link-Local Only** drop-down item. Then click on the **Save** button.
   *[Image: bond_nm_connection_editor_4.png]*
   Your computer is now properly configured to grab from multiple cameras that are directly connected to it.
7. Test your cameras, as there is still a slim chance that they will try to take the same IP address. You must configure your cameras to use static IP addresses in the link-local range before releasing your final product.

## Configuring a bond with nmtui-edit

To configure a bond using nmtui-edit, follow the steps outlined below:

1. Start nmtui-edit.
2. Select the **Add** option, and select **Bond** from the drop-down list, then select **Create**.
   *[Image: bond_nmtui_edit_1.png]*
3. Select the **Add** option.
   *[Image: bond_nmtui_edit_2.png]*
4. Select the **Ethernet** option from the drop-down list, then select **Create**.
   *[Image: bond_nmtui_edit_3.png]*
5. Enter the name of the interfaces in the **Device** field. See [ip command](S02_Network_related_tools.md) for tips on identifying device name.
   *[Image: bond_nmtui_edit_4.png]*
6. Select the **Show** Option to display the **MTU** field and set it to 9000.
   *[Image: bond_nmtui_edit_5.png]*
7. Repeat step 3 to 6 once for each camera to be added to the bond.
8. Set the bond **Mode** to **Broadcast** from the drop-down list.
   *[Image: bond_nmtui_edit_6.png]*
9. Select the **Show** option next to **IPv4 CONFIGURATION**, then select the IP configuration **Link-Local** from the drop-down list. Lastly, click on the **Save** button.
   *[Image: bond_nmtui_edit_7.png]*
10. Select **OK** to save this configuration.
   *[Image: bond_nmtui_edit_8.png]*
11. Follow the step in [Setting MTU for bond in NetworkManager Console (nmtui-edit)](S03_Bandwidth_optimization.md) to configure the MTU of your new bond. Your computer is now properly configured to grab from multiple cameras directly connected to it.
12. Test your cameras, since there is a possibility that the cameras will attempt to use the same IP address. You must configure your cameras to use static IP addresses in the link-local range before releasing your final product.

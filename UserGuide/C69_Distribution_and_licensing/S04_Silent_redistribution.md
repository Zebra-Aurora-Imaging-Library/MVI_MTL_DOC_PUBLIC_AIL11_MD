---
doctype: UserGuide
part: "Miscellaneous"
chapter: Distribution_and_licensing
section: Silent_redistribution
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / licensing / Silent redistribution"
---

# Silent redistribution

A silent redistribution does not prompt your user for any Aurora Imaging Library or Aurora Imaging Library Lite information; instead, it uses a response file to provide the necessary setup parameters for the intended computer. You would use a silent redistribution when you are including Aurora Imaging Library or Aurora Imaging Library Lite within your application and you do not want to have any Aurora Imaging Library setup dialog boxes appear. You could also use a silent redistribution if you wanted to control the setup parameters for your client.

To redistribute the Aurora Imaging Library or Aurora Imaging Library Lite DLLs and device drivers using a silent redistribution:

1. Copy the contents of the Aurora Imaging Library or Aurora Imaging Library Lite installation media to your installation directory. Alternatively, you can use the _Redist.exe_ program to select the redistribution packages and drivers to copy in your installation directory. For more details on how to use this program, see [Interactive redistribution using your custom installation media](S03_Interactive_redistribution.md).
2. Record your setup options. To do so, start the _AILSetup.exe_ and follow the presented instructions for an interactive Aurora Imaging Library or Aurora Imaging Library Lite redistribution. Any selected setting is recorded and stored in the _ailsetup11.custom_ file. The _ailsetup11.custom_ file will be created in the _11_ folder under _%ALLUSERSPROFILE%\Aurora Imaging Library_ and must be moved to the root folder of your installation directory next to _AILSetup.exe_.
3. Have your redistribution program call the _AILSetup.exe_ program with the /s switch. The setup will silently install Aurora Imaging Library or Aurora Imaging Library Lite with the options that were selected during the recording phase and stored in the _ailsetup11.custom_ file.

> **Note:** You can also silently install Aurora Imaging Library updates by having your redistribution program call the Aurora Imaging Library update _AILSetup.exe_ program with the /s switch.

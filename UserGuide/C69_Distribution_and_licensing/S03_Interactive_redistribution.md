---
doctype: UserGuide
part: "Miscellaneous"
chapter: Distribution_and_licensing
section: Interactive_redistribution
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / licensing / Interactive redistribution"
---

# Interactive redistribution using your custom installation media

To redistribute the Aurora Imaging Library or Aurora Imaging Library Lite DLLs and device drivers, you can have your application's setup program call Aurora Imaging Library or Aurora Imaging Library Lite's redistribution setup program using the interactive redistribution mechanism. In this case, your customer will be prompted for information during the setup. There are two ways to implement this mechanism.

## Using the default Aurora Imaging Library redistribution media

To redistribute the DLLs and device drivers and use the default installation options, do as follows:

1. Copy the contents of the Aurora Imaging Library or Aurora Imaging Library Lite installation media to your installation directory.
2. Have your installation program call the Aurora Imaging Library setup executable file, located in your installation directory. The setup program will prompt your customer to choose which features to install. Your customer should only select the **Aurora Imaging Library runtime** option. The **Aurora Imaging Library runtime** option installs the required Aurora Imaging Library or Aurora Imaging Library Lite DLL files and device drivers on your client's computer.

## Building an Aurora Imaging Library redistribution with limited content

To limit the Aurora Imaging Library or Aurora Imaging Library Lite installation content to include in your installation directory and restrict the installation options, use the _Redist.exe_ program. To use this program, perform the following:

1. Launch the _Redist.exe_ program located in the _Redist_ directory of your Aurora Imaging Library or Aurora Imaging Library Lite installation media. The **Aurora Imaging Library Redistribution** dialog box is presented.
2. Specify the location of the original setup executable file used to install Aurora Imaging Library. To do so, click on the **Browse** button next to the **Original AILSetup.exe file** edit field. From the presented **Open** dialog box, select the _AILSetup.exe_ file located in the root of your Aurora Imaging Library or Aurora Imaging Library Lite installation media, and then click on the **Open** button.
3. Specify the location of the directory in which to output the generated installation. To do so, click on the **Browse** button next to the **Output Directory** edit field. From the presented **Browse For Folder** dialog box, select your output directory, and then click on the **OK** button.
4. Select the packages (for example, Distributed Aurora Imaging Library and/or Aurora Imaging Intellicam), the Aurora Imaging Library systems, and other services (for example, Memory and PCI services) that you want to make available to your customer during installation. Select the options that best fit your situation.
5. Click on the **Generate Installation** button to begin creating the disk image of the limited installation version of Aurora Imaging Library or Aurora Imaging Library Lite into your output directory.
6. Have your installation program call the _AILSetup.exe_ file located in the root of the disk image generated with _Redist.exe_. The _AILSetup.exe_ program will prompt your customer to choose which features to install from the features that you have made available.

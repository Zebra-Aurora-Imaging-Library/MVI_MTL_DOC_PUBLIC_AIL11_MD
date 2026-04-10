---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_under_Linux
section: Creating_interactive_redistribution_using_Linux
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / Linux / Creating interactive redistribution using Linux"
---

# Creating an interactive redistribution using Linux

To redistribute Aurora Imaging Library or Aurora Imaging Library Lite libraries and device drivers under Linux, you can have your application's setup program call the library's redistribution setup program using the interactive redistribution mechanism. In this case, your customer will be prompted for information during the setup. There are two ways to implement this mechanism.

To redistribute the libraries and device drivers and use the default installation options, perform the following:

1. Copy the _AIL_X.X-YYYY_Installer.run_ file from the library's installation media to your installation directory, where _X.X_ is the version number and _YYYY_ is the build number. The _AIL_X.X-YYYY_Installer.run_ file is a packed file that includes all library files to install.
2. Have your redistribution program call the _AIL_X.X-YYYY_Installer.run_ file, located in your installation directory.
   The _AIL_X.X-YYYY_Installer.run_ program will prompt your customer to choose which features to install. The first question asked is "Do you want to install the development components?". If your client is installing Aurora Imaging Library from your application's installation program, they should answer **no**.

To limit the content of the library installation media to include in your application's installation directory and restrict the installer options, you must generate a new _AIL_X.X-YYYY_Installer.run_ file that only includes the required components. To do so:

1. Launch the _AIL_X.X-YYYY_Installer.run_ program, located on the library's installation media, with the _-- --redist_ option.
   ```
   AIL_X.X-YYYY_Installer.run -- --redist 
   ```
   The **Aurora Imaging Library Redistribution** dialog box is presented.
2. Specify the location of the _AIL_X.X-YYYY_Installer.run_ file to copy to your installation directory. To do so, click the **Browse** button next to the **Source** edit field. From the presented **Open** dialog box, select the _AIL_X.X-YYYY_Installer.run_ file located in the root of the library's installation media, and then click on the **Open** button.
3. Specify the location to write your newly generated _AIL_X.X-YYYY_Installer.run_ file. To do so, click the **Browse** button next to the **Destination** edit field. From the presented **Choose folder** dialog box, select your destination directory, and then click on the **Select** button.
4. Select the Linux distribution, and the boards (frame grabbers/systems/hardware) that you want to make available to your customer during installation. If your customer is not using a hardware license-key or a hardware ID key, you can deselect the **Wibu (Hardware Key)** option to avoid copying the Wibu driver to your installation directory.
5. Click the **Generate** button to begin building the new _AIL_X.X-YYYY_Installer.run_ file in your destination directory.
6. Have your redistribution program call the newly generated _AIL_X.X-YYYY_Installer.run_ file, located in the specified destination directory.
   The _AIL_X.X-YYYY_Installer.run_ program will prompt your customer to choose which features to install from the features that you have made available.

> **Note:** In Linux, Aurora Imaging Library drivers are compiled during the installation process. If you are generating a redistribution file for a deployment that includes Zebra boards (frame grabbers/systems/hardware), you must ensure that the driver compilation environment exists on the client computer.

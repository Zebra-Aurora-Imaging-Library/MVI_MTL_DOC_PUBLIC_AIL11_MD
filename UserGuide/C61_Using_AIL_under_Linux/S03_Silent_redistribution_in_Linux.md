---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_under_Linux
section: Silent_redistribution_in_Linux
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / Linux / Silent redistribution in Linux"
---

# Silent redistribution under Linux

A silent redistribution does not prompt your user for any Aurora Imaging Library information; instead, it uses a configuration file to provide the necessary setup parameters for the intended computer. You would use a silent redistribution when you are including Aurora Imaging Library within your application and you do not want to have any setup questions appear at the command prompt. You could also use a silent redistribution if you want to control the setup parameters for your client.

If you are going to use a configuration file, you must provide answers to all of the setup questions.

To redistribute the Aurora Imaging Library libraries and device drivers using a silent redistribution:

1. Copy the _ail-11_XXXX-installer.run_ file from the library installation media to an empty working directory on a temporary computer. Your working directory should be on a computer that does not have the library installed. The _ail-11_XXXX-installer.run_ file is a packed file that includes all library files to install. Optionally, to reduce the number of library components (files) that will be installed on your customer's computer, remove the unnecessary files in the _ail-11_XXXX-installer.run_ file as described in [Creating an interactive redistribution using Linux](S02_Creating_interactive_redistribution_using_Linux.md).
2. Record in the configuration file the setup options that should be used when the library is installed on your customer's computer. To record your setup options, you must install Aurora Imaging Library on your temporary computer with the required options. To install the library and record the choices made during the installation, execute:
   ```
   ail-11_XXXX-installer.run -- --recordfile=/home/AIL11/ail-install-recording.ini
   ```
   where `/home/AIL11/ail-install-recording.ini` can be any absolute path to a configuration file that will record all the options you will select during the installation process.
   > **Note:** If you do not specify a path with the `--recordfile` parameter, your Linux _/var/tmp_ directory is used by default.
   The _ail-11_XXXX-installer.run_ file will unpack and then begin installing Aurora Imaging Library. If the library is already installed, an error message appears asking you to first uninstall.
   The library installation program poses a series of questions via a command prompt. You can select which library setup options to use during the installation on your customer's computer. Once all selections are made, Aurora Imaging Library is installed. Note that the library is installed in a predetermined directory.
3. Copy the configuration file along with the _ail-11_XXXX-installer.run_ file to the location from which you are building your installation media or package.
4. Have your installation program call the library installer as follows:
   ```
   ail-11_XXXX-installer.run -– --play=ail-install-recording.ini
   ```
   The library installer will silently install with the options that you selected during the recording phase and stored in the _ail-install-recording.ini_ file.
   > **Note:** When running the installation with a configuration file (_ail-install-recording.ini_), use the same version of the library installer file (_ail-11_XXXX-installer.run_) as the one used to generate the configuration file; otherwise, an error will occur.

---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_under_Linux
section: Additional_Linux_notes_Security_permissions_SecureBoot_Firewall
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / Linux / Additional Linux notes Security permissions SecureBoot Firewall"
---

# Miscellaneous user guide information for Linux: Security, permissions, SecureBoot, and firewalls

This section includes additional user guide information for Linux regarding security, permissions, SecureBoot and firewalls. The information found in this section might be a reiteration of content previously documented.

## Security and permissions

To prevent permission access errors, two users should not be running Aurora Imaging Library at the same time on the same computer.

The Aurora Imaging Library installer changes the sudo configuration file (/etc/sudoers) so that Aurora Imaging Library can shutdown a user's computer after updating the firmware of their installed Zebra boards. Changing the configuration file is required because when you run an Aurora Imaging Library example/application, the library automatically detects whether the firmware of each installed Zebra board is up-to-date. If it is not, the library will update the firmware of the board and then request to shutdown the computer. If the current user does not have sufficient privileges, they will not be able to proceed with the required shutdown and the current example/application will wait indefinitely.

After uninstalling Aurora Imaging Library, the sudo configuration file is restored.

If you get SELinux errors, execute the following command:

```
# ail-selinux
```

This will fix the SELinux context of the Aurora Imaging Library *.lib files.

## SecureBoot

Kernel modules must be signed if Secure Boot is enabled in the UEFI BIOS in order for them to be loaded. You can check the status of Aurora Imaging Library's kernel modules (i.e., drivers) through Aurora Imaging Configurator's Drivers compilation page.

To self-sign Aurora Imaging Library's kernel modules, follow the steps below.

- Generate a custom x509 key:
  ```
  > openssl req -new -x509 -newkey rsa:2048 -keyout MOK.priv -outform DER -out MOK.der -nodes -subj "/CN=Owner/"
  ```
  Keep it for reference to later sign the kernel modules.
- Enroll the new key:
  ```
  > sudo mokutil --import MOK.der
  ```
  This will request the public key to be added to the MOK (Machine Owner Key).
- Sign the kernel modules:
  ```
  > for f in /opt/aurora_imaging_library/11/drivers/modules/custom/$(uname -r)/*.ko; do sudo /lib/modules/$(uname -r)/build/scripts/sign-file sha256 ~/MOK.priv ~/MOK.der $f;done
  ```
- Reboot.
  After reboot, a dialog will appear to accept enrolling the new MOK key (MokManager). Use the password used to import the MOK key.

> **Note:** The kernel modules must be re-signed each time the Linux kernel is updated. As previously specified, check Aurora Imaging Configurator's Drivers compilation for the status. For more information on how to sign kernel modules, see the man pages for mokutil or the web site for your Linux distribution.

## Firewall

Some Aurora Imaging Library network services require changes to the firewall settings to function correctly. Unlike Windows, the Linux Aurora Imaging Library installer doesn't change the firewall rules, therefore the user must do it manually using the iptables utility or a system configuration tool.

Here is a list of Aurora Imaging Library services and default TCP/IP ports (to be opened):

```
Distributed Aurora Imaging Library Controller/Worker (single cluster) 57010
Distributed Aurora Imaging Library Controller (multiple clusters) 57010
Distributed Aurora Imaging Library Worker (multiple clusters) 57000 to 57009
Aurora Imaging Library                                                    Monitoring 57020
Aurora Imaging Library                                                    Cluster Manager 57030
```

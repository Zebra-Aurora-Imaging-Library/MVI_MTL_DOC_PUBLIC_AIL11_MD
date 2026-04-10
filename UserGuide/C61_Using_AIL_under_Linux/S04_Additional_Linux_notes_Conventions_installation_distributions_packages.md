---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_under_Linux
section: Additional_Linux_notes_Conventions_installation_distributions_packages
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / Linux / Additional Linux notes Conventions installation distributions packages"
---

# Miscellaneous user guide information for Linux: Conventions, installation, supported distributions, and packages required

This section includes additional user guide information for Linux regarding conventions, installation, and supported distributions. The information found in this section might be a reiteration of content previously documented.

## Conventions

Command lines starting with # must be executed with root permissions, either as the root user or using the sudo command. Command lines starting with $ can be executed without root permissions.

## Installation

Before installing Aurora Imaging Library, make sure the [required packages](S04_Additional_Linux_notes_Conventions_installation_distributions_packages.md) are installed.

To install Aurora Imaging Library, execute the following command:

```
# sh ./ail-11.YY_XXXX-installer64.run
```

Whereas, to install Aurora Imaging Library Lite, execute the following command:

```
# sh ./ail-lite-11.YY_XXXX-installer64.run
```

Replace YY.XXXX with the minor version and build number.

During installation, a new group named aurora_imaging is added to the /etc/group file, and you will be prompted to add at least one valid user to this group. Any user in this group can change the configuration using Aurora Imaging Configurator (modify examples, recompile Aurora Imaging Library drivers, and manage services for Distributed Aurora Imaging Library).

Note that you don't need to add the root user to the aurora_imaging group.

During installation, Aurora Imaging Library drivers will be compiled to match the version of the running kernel. After installation, any user in the aurora_imaging group can recompile the drivers by accessing the "Driver Status" page of Aurora Imaging Configurator, and then click on the "Compile Drivers" button.

Note, Ubuntu will silently keep the kernel up to date and then Aurora Imaging Library will silently recompile the drivers. This typically happens automatically. However, if a problem occurs, use Aurora Imaging Configurator to check the log file and recompile the drivers so they can be loaded by the new kernel.

If the drivers do not load, check Drivers Status under Boards in Aurora Imaging Configurator (AILConfig.exe).

### Uninstall

To uninstall Aurora Imaging Library completely, execute the following command (as root):

```
# /opt/aurora_imaging_library/11/tools/ail-uninstall.sh
```

## Supported distributions

The Ubuntu LTS 24.04 64-bit distribution is supported on Intel Platforms.

## Packages required

Typically, Aurora Imaging Library requires the following packages to run under Ubuntu. You will not be able to install Aurora Imaging Library if the required packages listed below are not installed.

### Ubuntu

Runtime: libxss1.

Development or Drivers compilation: build-essential.

Optional: needed for some features, examples, etc: setserial python3-pip libncurses-dev python3-tk libcanberra-gtk3-dev qt6-base-dev elfutils python3-matplotlib.

To install the packages, use the `apt-get` command.

```
For example: apt-get install setserial
```

## Gtk/Qt examples and additional packages

If you want to recompile the Gtk and Qt examples, the following packages must be installed:

```
gtk3-devel
qt6-devel
```

> **Note:** `qt5-devel` is still supported for those Linux distributions that do not support the Qt 6 package.

You might need to install additional packages, depending on the following:

- The installed edition of Linux (Desktop Edition/Alternate Edition/ Server Edition).
- The installed distribution of Linux is an unsupported distribution.

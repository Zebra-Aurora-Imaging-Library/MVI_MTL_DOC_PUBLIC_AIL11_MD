---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_with_Python
section: Installation_and_distribution_in_Python
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / Python / Installation and distribution in Python"
---

# Importing required libraries and distribution of an Aurora Imaging Library application in Python

This section discusses the libraries required to develop and distribute Aurora Imaging Library applications in Python.

## Importing required libraries

To develop Aurora Imaging Library applications in Python, you will need to import the Aurora Imaging Library Python wrapper package. During installation, Aurora Imaging Library looks for a supported Python installation in the _PATH_ variable and automatically calls pip to install the Aurora Imaging Library package in your Python environment. If a supported Python installation is not found during the installation of Aurora Imaging Library, you will have to install the Aurora Imaging Library package manually. The Aurora Imaging Library Python package can be found in the _&#123;&#125;Aurora Imaging Library\11\Library\Scripting\pythonwrapper\dist_ directory of your Aurora Imaging Library installation.

To install the Aurora Imaging Library package in Windows, use the following statement:

```
pip install ail11 --no-index --find-links="%AIL_PATH11%\..\scripting\pythonwrapper\dist"
```

To install the Aurora Imaging Library package in Linux, use the following statement:

```
pip install ail11 --no-index --find-links="${AILDIR11}/scripting/pythonwrapper/dist"
```

> **Note:** The above commands are not necessary if you install Python prior to installing Aurora Imaging Library 11.

For more information on the pip package installer for Python, see [pypi.org/project/pip/](https://pypi.org/project/pip/).

To import the Aurora Imaging Library wrapper, use the following statement:

```
import ail11 as AIL
```

> **Note:** Note that importing ail11 as `AIL` is just a convention, but one that will be used throughout this chapter.

## Distribution

To run Aurora Imaging Library applications developed in Python, your client must have a valid Aurora Imaging Library runtime installation, the Aurora Imaging Library Python wrapper (which is included in the Aurora Imaging Library installation), and a Python interpreter for the version of Python used in your application. Note that the version of Python must be one supported by Aurora Imaging Library (Python 3.6 or later depending on the operating system or distribution). See the release notes for the exact supported version.

To distribute an Aurora Imaging Library application written in Python, follow the exact same instructions as when distributing an Aurora Imaging Library application written in C/C++. For more information, see [Interactive redistribution using your custom installation media](../C69_Distribution_and_licensing/S03_Interactive_redistribution.md). Make sure to include all your Python scripts and dependencies when distributing your application. If you intend to distribute your application with limited Aurora Imaging Library/Aurora Imaging Library Lite content, ensure that the Python wrapper is included in the distribution.

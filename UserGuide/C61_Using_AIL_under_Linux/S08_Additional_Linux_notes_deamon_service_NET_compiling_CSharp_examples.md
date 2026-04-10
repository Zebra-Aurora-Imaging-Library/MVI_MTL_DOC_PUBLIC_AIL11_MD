---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_under_Linux
section: Additional_Linux_notes_deamon_service_NET_compiling_CSharp_examples
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / Linux / Additional Linux notes deamon service NET compiling CSharp examples"
---

# Miscellaneous user guide information for Linux: deamon/service

This section includes additional user guide information for Linux regarding deamon/services, .NET 8.0 support, and compiling C# examples. The information found in this section might be a reiteration of content previously documented.

## Using Aurora Imaging Library in a deamon/service

Aurora Imaging Library applications need some environment variables. These are normally provided by the ail.sh script installed by the library's setup program.

Services/deamons run in an environment with a minimal set of environment variables, which can require special considerations (which is not the case otherwise, when using Aurora Imaging Library by default).

- Services started using initd.
  The init script of the service must source the /etc/profile.d/ail.sh file.
- Services started using systemd.
  The following entry must be added to the [Service] section of the systemd config file of the service: `EnvironmentFile=/etc/environment`.

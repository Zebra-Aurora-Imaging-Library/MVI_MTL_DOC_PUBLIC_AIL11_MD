---
doctype: UserGuide
part: "Miscellaneous"
chapter: Distribution_and_licensing
section: Hiding_the_licensing_process
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / licensing / Hiding the licensing process"
---

# Hiding the Aurora Imaging Library licensing process

It is possible to make the presence of Aurora Imaging Library licensing hidden so that your customer can install your Aurora Imaging Library application and not have to deal with the Aurora Imaging Library licensing process.

One way of hiding the Aurora Imaging Library licensing process is to purchase and redistribute hardware license-keys. You will have to purchase a hardware license-key for each application that you are distributing.

If purchasing and distributing hardware license-keys is not possible or preferable, there is a command-line utility, gencode, that allows you to get a software license-key while hiding the Aurora Imaging Library licensing process. gencode is essentially the silent command-line version of the **License** item in the Aurora Imaging Configurator utility. gencode can generate the lock code needed to get an Aurora Imaging Library software license-key, as well as register the license. For more detailed and up-to-date information on gencode's parameters and functionality, refer to the gencode help screen. You can access the help screen with the following command:

```
gencode /h 
		
```

> **Note:** gencode commands are case-sensitive.

## Generating the lock code

There are many different ways of getting the information that the gencode utility needs to generate a lock code. For example, you can create an interactive interface, or have a series of prompts during the setup of your application. If you know the hardware and the packages that your client will be using, you can build a batch file which calls gencode with the predetermined information. For example, if you know that your client will be using a Zebra Rapixo CXP board, you could hardcode gencode's command-line parameter **HardwareFingerprint** to the constant that means Zebra Rapixo CXP.

To generate a lock code from a hardware fingerprint, the setup program of your application must internally call gencode as follows:

```
gencode /g Filename HardwareFingerprint Packages 
		  
```

Replace the _Filename_ parameter with the path and file name in which to store the lock code. Replace the _HardwareFingerprint_ parameter with the fingerprint of the hardware component you will use to generate the lock code. Replace the _Packages_ parameter with the value corresponding to packages you want to license.

You can retrieve a list of available hardware, in your computer, that can be used as the fingerprint for the license, as well as their corresponding constant, using the following command:

```
gencode /i 
		  
```

You can retrieve a list of available hardware, in your computer, that can be used as the fingerprint for an upgradable license, as well as their corresponding constant, using the following command:

```
gencode /u 
		  
```

You can retrieve a list of possible packages, as well as the accepted values for each, using the following command:

```
gencode /a 
		  
```

## Getting a software license-key

Once the lock code is actually created, there are some steps that must be followed:

1. Your customer must send you the lock code, by whichever means you designate.
2. Take the lock code and obtain the software license-key by contacting your local representative or using the Zebra licensing portal website.
3. Your customer must input the software license-key into whatever activation interface you have created.

## Entering the software license-key

You can activate your customer's Aurora Imaging Library license by entering the software license-key with the following command:

```
gencode /r LicenseKey 
		  
```

Replace the _LicenseKey_ parameter with the software license-key that you obtained from your local representative or using the Zebra licensing portal website.

---
title: How to Perform Fuzz Tests with IoSpy and IoAttack
description: This topic describes how to perform fuzz tests by using the IoSpy and IoAttack tools
ms.assetid: f800e962-2a0f-4039-a479-395a62428b06
ms.author: windowsdriverdev
ms.date: 04/20/2017
ms.topic: article
ms.prod: windows-hardware
ms.technology: windows-devices
---

# How to Perform Fuzz Tests with IoSpy and IoAttack


To perform fuzz tests by using [IoSpy](iospy.md) and [IoAttack](ioattack.md), do the following:

1.  Install [IoSpy](iospy.md):

    To install IoSpy and enable fuzz tests on specific device(s), run the **Enable I/O Spy** test. The *DQ* parameter controls which devices the IoSpy filter driver is installed on.

2.  Run IOCTL and WMI tests on the specified devices:

    After you restart the test system, [IoSpy](iospy.md) is ready to filter IOCTL and WMI requests to the devices that were enabled for fuzz tests. You must now exercise the IOCTL and WMI code paths in drivers for these devices by using whatever tests are appropriate. This allows IoSpy to record as many details as possible based on these IOCTL and WMI requests. IoSpy saves these details in the [IoSpy Data File](iospy.md).

3.  Uninstall IoSpy:

    After you have fully exercised the IOCTL and WMI code paths, you must first uninstall IoSpy before you can run IoAttack to perform the fuzz tests. To uninstall [IoSpy](iospy.md), run the **Disable I/O Spy** test.

    This command removes the [IoSpy](iospy.md) filter driver from all devices that were enabled for fuzz tests. After the command is run, reboot the test system in order to unload the IoSpy filter driver from memory.

    **Note**  When you uninstall IoSpy, it will not delete the IoSpy data file. The location of this file is set by the *DFD* parameter to the **Enable I/O Spy** test. The default location is %SystemDrive%\\DriverTest\\IoSpy. For more information, see [IoSpy Data File](iospy.md).

     

4.  Run [IoAttack](ioattack.md):

    The test system is now ready to run the fuzz tests by running the **Run I/O Attack** test. For more information about how to run IoAttack, see [IoAttack](ioattack.md).

    **Note**   In order to verify the access privileges of your driver's IOCTL and WMI interfaces, you should run [IoAttack](ioattack.md) accounts with different privileges, such as a guest account and an administrator account.

     

 

 






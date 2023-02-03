--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# Working with VTL Devices<a name="resource_vtl-devices"></a>

Your Tape Gateway setup provides the following SCSI devices, which you select when activating your gateway\.

**Topics**
+ [Selecting a Medium Changer After Gateway Activation](#change-mediumchanger-vtl)
+ [Updating the Device Driver for Your Medium Changer](#update-vtl-device-driver)
+ [Displaying Barcodes for Tapes in Microsoft System Center DPM](#enable-barcode)

For medium changers, AWS Storage Gateway works with the following: 
+ AWS\-Gateway\-VTL – This device is provided with the gateway\.
+ STK\-L700 – This device emulation is provided with the gateway\.

  When activating your Tape Gateway, you select your backup application from the list and Storage Gateway uses the appropriate medium changer\. If your backup application is not listed, you choose **Other** and then choose the medium changer that works with backup application\.

  The type of medium changer you choose depends on the backup application you plan to use\. The following table lists third\-party backup applications that have been tested and found to be compatible with Tape Gateways\. This table includes the medium changer type recommended for each backup application\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/tgw/resource_vtl-devices.html)
**Important**  
We highly recommend that you choose the medium changer that's listed for your backup application\. Other medium changers might not function properly\. You can choose a different medium changer after the gateway is activated\. For more information, see [Selecting a Medium Changer After Gateway Activation](https://docs.aws.amazon.com/storagegateway/latest/tgw/resource_vtl-devices.html#change-mediumchanger-vtl)\.

For tape drives, Storage Gateway works with the following:
+ IBM\-ULT3580\-TD5—This device emulation is provided with the gateway\.

## Selecting a Medium Changer After Gateway Activation<a name="change-mediumchanger-vtl"></a>

 After your gateway is activated, you can choose to select a different medium changer type\. 

**To select a different medium changer type after gateway activation**

1. Stop any related jobs that are running in your backup software\.

1. On the Windows server, open the iSCSI initiator properties window\.

1. Choose the **Targets** tab to display the discovered targets\.

1. On the Discovered targets pane, choose the medium changer you want to change, choose **Disconnect**, and then choose **OK**\. 

1. On the Storage Gateway console, choose **Gateways** from the navigation pane, and then choose the gateway whose medium changer you want to change\.

1. Choose the **VTL Devices** tab, select the medium changer you want to change, and then choose **Change Media Changer**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/tgw/images/Change_MediumChanger.png)

1. In the Change Media Changer Type dialog box that appears, select the media changer you want from the drop\-down list box and then choose **Save**\.

## Updating the Device Driver for Your Medium Changer<a name="update-vtl-device-driver"></a>

1. Open Device Manager on your Windows server, and expand the **Medium Changer devices** tree\.

1. Open the context \(right\-click\) menu for **Unknown Medium Changer**, and choose **Update Driver Software** to open the **Update Driver Software\-unknown Medium Changer** window\.

1. In the **How do you want to search for driver software?** section, choose **Browse my computer for driver software**\.

1. Choose **Let me pick from a list of device drivers on my computer**\. 
**Note**  
We recommend using the Sony TSL\-A500C Autoloader driver with the Veeam Backup & Replication 11A and Microsoft System Center Data Protection Manager backup software\. This Sony driver has been tested with these types of backup software up to and including Windows Server 2019\.  
 

1. In the **Select the device driver you want to install for this hardware** section, clear the **Show compatible hardware** check box, choose **Sony** in the **Manufacturer** list, choose **Sony \- TSL\-A500C Autoloader** in the **Model** list, and then choose **Next**\.

1. In the warning box that appears, choose **Yes**\. If the driver is successfully installed, close the **Update drive software** window\.

## Displaying Barcodes for Tapes in Microsoft System Center DPM<a name="enable-barcode"></a>

If you use the media changer driver for Sony TSL\-A500C Autoloader, Microsoft System Center Data Protection Manager doesn't automatically display barcodes for virtual tapes created in Storage Gateway\. To display barcodes correctly for your tapes, change the media changer driver to Sun/StorageTek Library\.

**To display barcodes**

1. Ensure that all backup jobs have completed and that there are no tasks pending or in progress\.

1. Eject and move the tapes to offline storage \(S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive\) and exit the DPM Administrator console\. For information about how to eject a tape in DPM, see [Archiving a Tape by Using DPM](backup-DPM.md#dpm-archive-tape)\.

1. In **Administrative Tools**, choose **Services** and open the context \(right\-click\) menu for **DPM Service** in the **Detail** pane, and then choose **Properties**\.

1. On the **General** tab, ensure that the **Startup type** is set to **Automatic** and choose **Stop** to stop the DPM service\.

1. Get the StorageTek drivers from [Microsoft Update Catalog](http://www.catalog.update.microsoft.com/Search.aspx?q=storagetek%20-%20sun%2Fstoragetek%20library) on the Microsoft website\. 
**Note**  
Take note of the different drivers for the different sizes\.

   For **Size** 18K, choose **x86 drivers**\.

   For **Size** 19K, choose **x64 drivers**\.

1. On your Windows server, open Device Manager, and expand the **Medium Changer Devices** tree\.

1. Open the context \(right\-click\) menu for **Unknown Medium Changer**, and choose **Update Driver Software** to open the **Update Driver Software\-unknown Medium Changer** window\.

1. Browse to the path of the new driver location and install\. The driver appears as **Sun/StorageTek Library**\. The tape drives remain as an IBM ULT3580\-TD5 SCSI sequential device\. 

1. Reboot the DPM server\.

1. In the Storage Gateway console, create new tapes\.

1. Open the DPM Administrator console, choose **Management**, then choose **Rescan for new tape libraries** \. You should see the **Sun/StorageTek library**\.

1. Choose the library and choose **Inventory**\.

1. Choose **Add Tapes** to add the new tapes into DPM\. The new tapes should now display their barcodes\.
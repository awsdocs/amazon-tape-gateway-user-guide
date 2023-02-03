--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# Moving your data to a new gateway<a name="migrate-data"></a>

You can move data between gateways as your data and performance needs grow, or if you receive an AWS notification to migrate your gateway\. The following are some reasons for doing this:
+ Move your data to better host platforms or newer Amazon EC2 instances\.
+ Refresh the underlying hardware for your server\.

The steps that you follow to move your data to a new gateway depend on the gateway type that you have\.

**Note**  
Data can only be moved between the same gateway types\.

## Moving virtual tapes to a new Tape Gateway<a name="migrate-data-tape"></a>



**To move your virtual tape to a new Tape Gateway**

1. Use your backup application to back up all your data onto a virtual tape\. Wait for the backup to finish successfully\.

1. Use your backup application to eject your tape\. The tape will be stored in one of the Amazon S3 storage classes\. Ejected tapes are archived in S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive, and are read\-only\.

   Before proceeding, confirm that the ejected tapes have been archived:

   1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

   1. In the navigation pane, choose **Tape Library > Tapes** to see your tapes\. By default, this list displays up to 1,000 tapes at a time, but the searches that you perform apply to all of your tapes\. You can use the search bar to find tapes that match a specific criteria, or to reduce the list to less than 1,000 tapes\. When your list contains 1,000 tapes or fewer, you can then sort your tapes in ascending or descending order by various properties\.

   1. In the **Status** column of the list, check the status of the tape\.

      The tape status also appears in the **Details** tab of each virtual tape\.

      For more information about determining tape status in an archive, see [Determining Tape Status in an Archive](managing-gateway-vtl.md#determine-tape-status-vts)\.

1. Using your backup application, verify that there are no active backup jobs going to the existing Tape Gateway before you stop it\. If there are any active backup jobs, wait for them to finish and eject your tapes \(see previous step\) before stopping the gateway\.

1. Use the following steps to stop the existing Tape Gateway:

   1. In the navigation pane, choose **Gateways**, and then choose the old Tape Gateway that you want to stop\. The status of the gateway is **Running**\.

   1. For **Actions**, choose **Stop gateway**\. Verify the ID of the gateway from the dialog box, and then choose **Stop gateway**\.

      While the old Tape Gateway is stopping, you might see a message that indicates the status of the gateway\. When the gateway shuts down, a message and a **Start gateway** button appear in the **Details** tab\.

   For more information about stopping a gateway, see [Starting and Stopping a Tape Gateway](MaintenanceShutDown-common.md#start-stop-classic)\.

1. Create a new Tape Gateway\. For detailed instructions, see [Creating a Gateway](https://docs.aws.amazon.com/storagegateway/latest/tgw/create-gateway-vtl.html)\.

1. Use the following steps to create new tapes:

   1. In the navigation pane, choose the **Gateways** tab\.

   1. Choose **Create tape** to open the **Create tape** dialog box\.

   1. For **Gateway**, choose a gateway\. The tape is created for this gateway\.

   1. For **Number of tapes**, choose the number of tapes that you want to create\. For more information about tape limits, see [AWS Storage Gateway quotas](resource-gateway-limits.md)\.

      You can also set up automatic tape creation at this point\. For more information, see [Creating Tapes Automatically](https://docs.aws.amazon.com/storagegateway/latest/tgw/GettingStartedCreateTapes.html#CreateTapesAutomatically)\.

   1. For **Capacity**, enter the size of the virtual tape that you want to create\. Tapes must be larger than 100 GiB\. For information about capacity limits, see [AWS Storage Gateway quotas](resource-gateway-limits.md)\.

   1. For **Barcode prefix**, enter the prefix that you want to prepend to the barcode of your virtual tapes\.
**Note**  
Virtual tapes are uniquely identified by a barcode\. You can add a prefix to the barcode\. The prefix is optional, but you can use it to help identify your virtual tapes\. The prefix must be uppercase letters \(A–Z\) and must be one to four characters long\.

   1. For **Pool**, choose **Glacier Pool** or **Deep Archive Pool**\. This pool represents the storage class in which your tape will be stored when it is ejected by your backup software\.

      Choose **Glacier Pool** if you want to archive the tape in S3 Glacier Flexible Retrieval\. When your backup software ejects the tape, it is automatically archived in S3 Glacier Flexible Retrieval\. You use S3 Glacier Flexible Retrieval for more active archives where you can retrieve a tape typically within 3\-5 hours\. For more information, see [Storage classes for archiving objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon Simple Storage Service User Guide*\.

      Choose **Deep Archive Pool** if you want to archive the tape in S3 Glacier Deep Archive\. When your backup software ejects the tape, the tape is automatically archived in S3 Glacier Deep Archive\. You use S3 Glacier Deep Archive for long\-term data retention and digital preservation where data is accessed once or twice a year\. You can retrieve a tape archived in S3 Glacier Deep Archive typically within 12 hours\. For more information, see [Storage classes for archiving objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon Simple Storage Service User Guide*\.

      If you archive a tape in S3 Glacier Flexible Retrieval, you can move it to S3 Glacier Deep Archive later\. For more information, see [Moving Your Tape from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive Storage Class](moving-tapes-vtl.md)\.
**Note**  
Tapes created before March 27, 2019, are archived directly in S3 Glacier Flexible Retrieval when your backup software ejects them\.

   1. \(Optional\) For **Tags**, enter a key and value to add tags to your tape\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your tapes\.

   1. Choose **Create tapes**\.

1. Use your backup application to start a backup job, and back up your data to the new tape\.

1. \(Optional\) If your tape is archived and you need to restore data from it, retrieve it to the new Tape Gateway\. The tape will be in read\-only mode\. For more information about retrieving archived tapes, see [Retrieving Archived Tapes](retrieving-archived-tapes-vtl.md)\.
**Note**  
Outbound data charges might apply\.

   1. In the navigation pane, choose **Tape Library > Tapes** to see your tapes\. By default, this list displays up to 1,000 tapes at a time, but the searches that you perform apply to all of your tapes\. You can use the search bar to find tapes that match a specific criteria, or to reduce the list to less than 1,000 tapes\. When your list contains 1,000 tapes or fewer, you can then sort your tapes in ascending or descending order by various properties\.

   1. Choose the virtual tape that you want to retrieve\. For **Actions**, choose **Retrieve Tape**\.
**Note**  
The status of the virtual tape that you want to retrieve must be `ARCHIVED`\.

   1. In the **Retrieve tape** dialog box, for **Barcode**, verify that the barcode identifies the virtual tape you want to retrieve\.

   1. For **Gateway**, choose the new Tape Gateway that you want to retrieve the archived tape to, and then choose **Retrieve tape**\.

   When you have confirmed that your new Tape Gateway is working correctly, you can delete the old Tape Gateway\.
**Important**  
Before you delete a gateway, be sure that there are no applications currently writing to that gateway's volumes\. If you delete a gateway while it is in use, data loss can occur\.

1. Use the following steps to delete the old Tape Gateway:
**Warning**  
When a gateway is deleted, there is no way to recover it\.

   1. In the navigation pane, choose **Gateways**, and then choose the gateway that you want to delete\.

   1. For **Actions**, choose **Delete gateway**\.

      In the confirmation dialog box that appears, make sure that the gateway ID listed specifies the old Tape Gateway that you want to delete, enter **delete** in the confirmation field, and then choose **Delete**\.

   1. Delete the VM\. For more information about deleting a VM, see the documentation for your hypervisor\.
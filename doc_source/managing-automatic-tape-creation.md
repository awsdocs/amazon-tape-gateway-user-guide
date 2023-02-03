--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# Managing Automatic Tape Creation<a name="managing-automatic-tape-creation"></a>

The Tape Gateway automatically creates new virtual tapes to maintain the minimum number of available tapes that you configure\. It then makes these new tapes available for import by the backup application so that your backup jobs can run without interruption\. Automatic tape creation removes the need for custom scripting in addition to the manual process for creating new virtual tapes\. 

**To delete an automatic tape creation policy**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose the **Gateways** tab\.

1. Choose the gateway for which you need to manage automatic tape creation\. 

1. In the **Actions** menu, choose **Configure tape auto\-create**\. 

1. To delete an automatic tape creation policy on a gateway, choose **Remove** to the right of the policy you want to delete\.

   To stop automatic tape creation on a gateway, delete all of the automatic tape creation policies for that gateway\.

   Choose **Save changes** to confirm deletion of tape auto\-create policies for the selected Tape Gateway\.
**Note**  
Deleting a tape auto\-creation policy from a gateway cannot be undone\.

**To change the automatic tape creation policies for a Tape Gateway**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose the **Gateways** tab\.

1. Choose the gateway for which you need to manage automatic tape creation\. 

1. In the **Actions** menu, choose **Configure tape auto\-create**, and change the settings on the page that appears\. 

1. For **Minimum number of tapes**, enter the minimum number of virtual tapes that should be available on the Tape Gateway at all times\. The valid range for this value is a minimum of 1 and a maximum of 10\. 

1. For **Capacity**, enter the size, in bytes of the virtual tape capacity\. The valid range for this value is a minimum of 100 GiB and a maximum of 15 TiB\. 

1. For **Barcode prefix**, enter the prefix that you want to prepend to the barcode of your virtual tapes\.
**Note**  
Virtual tapes are uniquely identified by a barcode, and you can add a prefix to the barcode\. The prefix is optional, but you can use it to help identify your virtual tapes\. The prefix must be uppercase letters \(A–Z\) and must be one to four characters long\.

1. For **Pool**, choose **Glacier Pool** or **Deep Archive Pool**\. This pool represents the storage class in which your tapes are stored when they are ejected by your backup software\. 
   + Choose **Glacier Pool** if you want to archive the tapes in the S3 Glacier Flexible Retrieval storage class\. When your backup software ejects the tapes, they are automatically archived in S3 Glacier Flexible Retrieval\. You use S3 Glacier Flexible Retrieval for more active archives, where you can retrieve a tape typically within 3\-5 hours\. For detailed information, see [Storage Classes for Archiving Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon Simple Storage Service User Guide*\.
   + Choose **Deep Archive Pool** if you want to archive the tapes in S3 Glacier Deep Archive\. When your backup software ejects the tape, the tape is automatically archived in S3 Glacier Deep Archive\. You use S3 Glacier Deep Archive for long\-term data retention and digital preservation, where data is accessed once or twice a year\. You can retrieve a tape archived in S3 Glacier Deep Archive typically within 12 hours\. For detailed information, see [Storage Classes for Archiving Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon Simple Storage Service User Guide*\.

   If you archive tapes in S3 Glacier Flexible Retrieval, you can move them to S3 Glacier Deep Archive later\. For more information, see [Moving Your Tape from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive Storage Class](moving-tapes-vtl.md)\.

1. You can find information about your tapes on the **Tape overview** page\. By default, this list displays up to 1,000 tapes at a time, but the searches that you perform apply to all of your tapes\. You can use the search bar to find tapes that match a specific criteria, or to reduce the list to less than 1,000 tapes\. When your list contains 1,000 tapes or fewer, you can then sort your tapes in ascending or descending order by various properties\.

   The status of available virtual tapes is initially set to **CREATING** when the tapes are being created\. After the tapes are created, their status changes to **AVAILABLE**\. For more information, see [Managing Your Tape Gateway](managing-gateway-vtl.md)\.

   For more information about enabling automatic tape creation, see [Creating Tapes Automatically](https://docs.aws.amazon.com/storagegateway/latest/tgw/GettingStartedCreateTapes.html#CreateTapesAutomatically)\.
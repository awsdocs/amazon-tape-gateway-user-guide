--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# Managing Your Tape Gateway<a name="managing-gateway-vtl"></a>

Following, you can find information about how to manage your Tape Gateway resources in AWS Storage Gateway\.

**Topics**
+ [Editing Basic Gateway Information](edit-gateway-information.md)
+ [Adding Virtual Tapes](creating-virtual-tapes-vtl.md)
+ [Managing Automatic Tape Creation](managing-automatic-tape-creation.md)
+ [Archiving Virtual Tapes](archiving-tapes-vtl.md)
+ [Moving Your Tape from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive Storage Class](moving-tapes-vtl.md)
+ [Retrieving Archived Tapes](retrieving-archived-tapes-vtl.md)
+ [Viewing Tape Usage](tape-usage.md)
+ [Deleting Tapes](deleting-tapes-vtl.md)
+ [Deleting Custom Tape Pools](deleting-tape-pools-vtl.md)
+ [Disabling Your Tape Gateway](disabling-gateway-vtl.md)
+ [Understanding Tape Status](#understand-tapes-status)

## Understanding Tape Status<a name="understand-tapes-status"></a>

Each tape has an associated status that tells you at a glance what the health of the tape is\. Most of the time, the status indicates that the tape is functioning normally and that no action is needed on your part\. In some cases, the status indicates a problem with the tape that might require action on your part\. You can find information following to help you decide when you need to act\.

**Topics**
+ [Understanding Tape Status Information in a VTL](#tape-status)
+ [Determining Tape Status in an Archive](#determine-tape-status-vts)

### Understanding Tape Status Information in a VTL<a name="tape-status"></a>

A tape's status must be AVAILABLE for you to read or write to the tape\. The following table lists and describes possible status values\.


| Status | Description | Tape Data Is Stored In | 
| --- | --- | --- | 
| CREATING | The virtual tape is being created\. The tape can't be loaded into a tape drive, because the tape is being created\. | — | 
| AVAILABLE | The virtual tape is created and ready to be loaded into a tape drive\. | Amazon S3 | 
| IN TRANSIT TO VTS | The virtual tape has been ejected and is being uploaded for archive\. At this point, your Tape Gateway is uploading data to AWS\. If the amount of data being uploaded is small, this status might not appear\. When the upload is completed, the status changes to ARCHIVING\.  |  Amazon S3  | 
| ARCHIVING | The virtual tape is being moved by your Tape Gateway to the archive, which is backed by S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive\. This process happens after the data upload to AWS is completed\.  | Data is being moved from Amazon S3 to S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive\. | 
| DELETING | The virtual tape is being deleted\. | Data is being deleted from Amazon S3 | 
| DELETED | The virtual tape has been successfully deleted\. | — | 
| RETRIEVING | The virtual tape is being retrieved from the archive to your Tape Gateway\.  The virtual tape can be retrieved only to a Tape Gateway\.   | Data is being moved from S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive to Amazon S3 | 
| RETRIEVED | The virtual tape is retrieved from the archive\. The retrieved tape is write\-protected\. | Amazon S3 | 
| RECOVERED |  The virtual tape is recovered and is read\-only\.  When your Tape Gateway is not accessible for any reason, you can recover virtual tapes associated with that Tape Gateway to another Tape Gateway\. To recover the virtual tapes, first disable the inaccessible Tape Gateway\.   | Amazon S3 | 
| IRRECOVERABLE | The virtual tape can't be read from or written to\. This status indicates an error in your Tape Gateway\. | Amazon S3 | 

### Determining Tape Status in an Archive<a name="determine-tape-status-vts"></a>

You can use the following procedure to determine the status of a virtual tape in an archive\.

**To determine the status of a virtual tape**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Tapes**\. 

1. In the **Status** column of the tape library grid, check the status of the tape\.

   The tape status also appears in the **Details** tab of each virtual tape\.

Following, you can find a description of the possible status values\.


| Status | Description | 
| --- | --- | 
| ARCHIVED | The virtual tape has been ejected and is uploaded to the archive\. | 
| RETRIEVING | The virtual tape is being retrieved from the archive\.  The virtual tape can be retrieved only to a Tape Gateway\.    | 
| RETRIEVED | The virtual tape has been retrieved from the archive\. The retrieved tape is read\-only\. | 

For additional information about how to work with tapes and VTL devices, see [Working With Tapes](managing-virtual-tapes-vtl.md)\.
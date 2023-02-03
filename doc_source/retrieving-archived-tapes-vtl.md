--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# Retrieving Archived Tapes<a name="retrieving-archived-tapes-vtl"></a>

To access data stored on an archived virtual tape, you must first retrieve the tape that you want to your Tape Gateway\. Your Tape Gateway provides one virtual tape library \(VTL\) for each gateway\.

If you have more than one Tape Gateway in an AWS Region, you can retrieve a tape to only one gateway\.

The retrieved tape is write\-protected; you can only read the data on the tape\.

 

**Important**  
If you archive a tape in S3 Glacier Flexible Retrieval, you can retrieve the tape typically within 3\-5 hours\. If you archive the tape in S3 Glacier Deep Archive, you can retrieve it typically within 12 hours\.

**Note**  
There is a charge for retrieving tapes from archive\. For detailed pricing information, see [Storage Gateway Pricing](http://aws.amazon.com/storagegateway/pricing/)\.

**To retrieve an archived tape to your gateway**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Tape Library > Tapes** to see your tapes\. By default, this list displays up to 1,000 tapes at a time, but the searches that you perform apply to all of your tapes\. You can use the search bar to find tapes that match a specific criteria, or to reduce the list to less than 1,000 tapes\. When your list contains 1,000 tapes or fewer, you can then sort your tapes in ascending or descending order by various properties\.

1. Choose the virtual tape you want to retrieve from the **Virtual Tape Shelf** tab, and choose **Retrieve tape**\. 
**Note**  
The status of the virtual tape that you want to retrieve must be ARCHIVED\.

1. In the **Retrieve tape** dialog box, for **Barcode**, verify that the barcode identifies the virtual tape you want to retrieve\.

1. For **Gateway**, choose the gateway that you want to retrieve the archived tape to, and then choose **Retrieve tape**\.

The status of the tape changes from ARCHIVED to RETRIEVING\. At this point, your data is being moved from the virtual tape shelf \(backed by S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive\) to the virtual tape library \(backed by Amazon S3\)\. After all the data is moved, the status of the virtual tape in the archive changes to RETRIEVED\. 

**Note**  
Retrieved virtual tapes are read\-only\.